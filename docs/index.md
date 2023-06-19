---
hide:
  - navigation
---

# FastAPI 最佳实践

我们在创业时使用的并且自以为是的最佳实践和惯例列表。

在过去 1.5 年的生产中，
我们一直在做出好的和坏的决定，这些决定极大地影响了我们的开发人员体验。
其中一些值得分享。

## 1. 项目结构。 一致且可预测

构建项目的方法有很多种，但最好的结构是一致、直接且没有意外的结构。

- 如果查看项目结构不能让您了解项目的内容，那么结构可能不清楚。
- 如果您必须打开包才能了解其中包含哪些模块，那么您的结构就不清楚了。
- 如果文件的重复率和位置感觉是随机的，那么您的项目结构很糟糕。
- 如果查看模块的位置及其名称不能让您了解其中的内容，那么您的结构非常糟糕。

[@tiangolo](https://github.com/tiangolo) 提供的项目结构（我们按文件类型（例如 api、crud、模型、模式）分隔文件）适用于微服务或范围较小的项目，但我们无法将它放入我们具有大量域和模块的整体中。

我发现更具可扩展性和可演化性的结构是受 Netflix 的 [Dispatch](https://github.com/Netflix/dispatch) 启发并稍作修改。

```text
fastapi-project
├── alembic/
├── src
│   ├── auth
│   │   ├── router.py
│   │   ├── schemas.py  # pydantic 模型
│   │   ├── models.py  # db 模型
│   │   ├── dependencies.py
│   │   ├── config.py  # 本地配置
│   │   ├── constants.py
│   │   ├── exceptions.py
│   │   ├── service.py
│   │   └── utils.py
│   ├── aws
│   │   ├── client.py  # 外部服务通信的客户端模型
│   │   ├── schemas.py
│   │   ├── config.py
│   │   ├── constants.py
│   │   ├── exceptions.py
│   │   └── utils.py
│   └── posts
│   │   ├── router.py
│   │   ├── schemas.py
│   │   ├── models.py
│   │   ├── dependencies.py
│   │   ├── constants.py
│   │   ├── exceptions.py
│   │   ├── service.py
│   │   └── utils.py
│   ├── config.py  # 全局配置
│   ├── models.py  # 全局模型
│   ├── exceptions.py  # 全局异常
│   ├── pagination.py  # 全局模块 例如. pagination 分页
│   ├── database.py  # 数据库连接相关的东西
│   └── main.py
├── tests/
│   ├── auth
│   ├── aws
│   └── posts
├── templates/
│   └── index.html
├── requirements
│   ├── base.txt
│   ├── dev.txt
│   └── prod.txt
├── .env
├── .gitignore
├── logging.ini
└── alembic.ini
```

1. 将所有域目录存储在 `src` 文件夹中
    1. `src/` - 应用程序的最高级别，包含通用模型、配置和常量等。
    2. `src/main.py` - 项目的根目录，用于启动 FastAPI 应用程序
2. 每个包都有自己的 router, schemas, models, 等。
    1. `router.py` - 每个模块的核心，是所有路由接口的入口。
    2. `schemas.py` - 每个模块的 pydantic的模型
    3. `models.py` - 每个模块的数据库模型
    4. `service.py` - 模块特有的业务逻辑
    5. `dependencies.py` - 路由依赖(Depends)
    6. `constants.py` - 模块特有的常量和错误码定义
    7. `config.py` - 例如，环境变量
    8. `utils.py` - 非业务逻辑功能， 例如. 响应规范化、数据丰富等。
    9. `exceptions` - 模块特有的一场，例如. `PostNotFound`, `InvalidUserData`
3. 当包需要来自其他包的服务或依赖项或常量时 - 使用显式模块名称导入它们

```python
from src.auth import constants as auth_constants
from src.notifications import service as notification_service
from src.posts.constants import ErrorCode as PostsErrorCode  # 如果我们在每个包的常量模块中都有标准错误代码
```

## 2. 尽可能的使用 Pydantic 进行数据验证

Pydantic 具有一组丰富的功能来验证和转换数据。

除了具有默认值的必填和非必填字段等常规功能外，

Pydantic 内置了全面的数据处理工具，如正则表达式、有限允许选项的枚举、长度验证、电子邮件验证等。

```python
from enum import Enum
from pydantic import AnyUrl, BaseModel, EmailStr, Field, constr

class MusicBand(str, Enum):
   AEROSMITH = "AEROSMITH"
   QUEEN = "QUEEN"
   ACDC = "AC/DC"


class UserBase(BaseModel):
    first_name: str = Field(min_length=1, max_length=128)
    username: constr(regex="^[A-Za-z0-9-_]+$", to_lower=True, strip_whitespace=True)
    email: EmailStr
    age: int = Field(ge=18, default=None)  # must be greater or equal to 18
    favorite_band: MusicBand = None  # only "AEROSMITH", "QUEEN", "AC/DC" values are allowed to be inputted
    website: AnyUrl = None

```

## 3. 使用Depends(依赖)进行与数据库有关的数据验证

Pydantic 可以只验证来自客户端输入的值。

使用依赖项根据数据库约束验证数据，例如电子邮件已存在、未找到用户等。

```python
# dependencies.py
async def valid_post_id(post_id: UUID4) -> Mapping:
    post = await service.get_by_id(post_id)
    if not post:
        raise PostNotFound()

    return post


# router.py
@router.get("/posts/{post_id}", response_model=PostResponse)
async def get_post_by_id(post: Mapping = Depends(valid_post_id)):
    return post


@router.put("/posts/{post_id}", response_model=PostResponse)
async def update_post(
    update_data: PostUpdate,  
    post: Mapping = Depends(valid_post_id), 
):
    updated_post: Mapping = await service.update(id=post["id"], data=update_data)
    return updated_post


@router.get("/posts/{post_id}/reviews", response_model=list[ReviewsResponse])
async def get_post_reviews(post: Mapping = Depends(valid_post_id)):
    post_reviews: list[Mapping] = await reviews_service.get_by_post_id(post["id"])
    return post_reviews
```

如果我们不将数据验证放在依赖项中，我们将不得不为每个接口添加 `post_id` 验证并为每个接口编写相同的测试。

## 4. 依赖(Dependency)链

依赖可以使用其他依赖，避免类似逻辑的代码重复。

```python
# dependencies.py
from fastapi.security import OAuth2PasswordBearer
from jose import JWTError, jwt

async def valid_post_id(post_id: UUID4) -> Mapping:
    post = await service.get_by_id(post_id)
    if not post:
        raise PostNotFound()

    return post


async def parse_jwt_data(
    token: str = Depends(OAuth2PasswordBearer(tokenUrl="/auth/token"))
) -> dict:
    try:
        payload = jwt.decode(token, "JWT_SECRET", algorithms=["HS256"])
    except JWTError:
        raise InvalidCredentials()

    return {"user_id": payload["id"]}


async def valid_owned_post(
    post: Mapping = Depends(valid_post_id), 
    token_data: dict = Depends(parse_jwt_data),
) -> Mapping:
    if post["creator_id"] != token_data["user_id"]:
        raise UserNotOwner()

    return post

# router.py
@router.get("/users/{user_id}/posts/{post_id}", response_model=PostResponse)
async def get_user_post(post: Mapping = Depends(valid_owned_post)):
    return post

```

## 5. 解耦和重用依赖关系。 缓存依赖(Dependency)调用结果

依赖项可以多次重用，并且不会重新计算 - 默认情况下，FastAPI 将依赖项的结果缓存在请求的范围内。

例如：如果我们有一个调用服务 `get_post_by_id` 的依赖项，我们将不会在每次调用该依赖项时都访问数据库 - 只有第一次函数调用时会用到。

知道了这一点，我们可以轻松地将依赖关系解耦到多个较小的函数上，这些函数在较小的域上运行并且更容易在其他路由中重用。

例如，在下面的代码中，我们使用了 3 次 `parse_jwt_data`：

1. `valid_owned_post`
2. `valid_active_creator`
3. `get_user_post`

但是 `parse_jwt_data` 在第一次调用时只被调用一次。

```python
# dependencies.py
from fastapi import BackgroundTasks
from fastapi.security import OAuth2PasswordBearer
from jose import JWTError, jwt

async def valid_post_id(post_id: UUID4) -> Mapping:
    post = await service.get_by_id(post_id)
    if not post:
        raise PostNotFound()

    return post


async def parse_jwt_data(
    token: str = Depends(OAuth2PasswordBearer(tokenUrl="/auth/token"))
) -> dict:
    try:
        payload = jwt.decode(token, "JWT_SECRET", algorithms=["HS256"])
    except JWTError:
        raise InvalidCredentials()

    return {"user_id": payload["id"]}


async def valid_owned_post(
    post: Mapping = Depends(valid_post_id), 
    token_data: dict = Depends(parse_jwt_data),
) -> Mapping:
    if post["creator_id"] != token_data["user_id"]:
        raise UserNotOwner()

    return post


async def valid_active_creator(
    token_data: dict = Depends(parse_jwt_data),
):
    user = await users_service.get_by_id(token_data["user_id"])
    if not user["is_active"]:
        raise UserIsBanned()
    
    if not user["is_creator"]:
       raise UserNotCreator()
    
    return user
        

# router.py
@router.get("/users/{user_id}/posts/{post_id}", response_model=PostResponse)
async def get_user_post(
    worker: BackgroundTasks,
    post: Mapping = Depends(valid_owned_post),
    user: Mapping = Depends(valid_active_creator),
):
    """Get post that belong the active user."""
    worker.add_task(notifications_service.send_email, user["id"])
    return post

```

## 6. 遵循 REST 规范

!!! note "译者注 - REST API 设计规范"

    参考 [阮一峰](http://www.ruanyifeng.com/){target="_blank"}老师的 - [RESTful API 设计指南](./rest-ful/restapi.md){target="_blank"} 和 [RESTful API 最佳实践](./rest-ful/restapibsetp.md){target="_blank"}

开发 RESTful API 可以更轻松地在如下路由中重用依赖项：

   1. `GET /courses/:course_id`
   2. `GET /courses/:course_id/chapters/:chapter_id/lessons`
   3. `GET /chapters/:chapter_id`

唯一需要注意的是在路径中使用相同的变量名：

- 如果你有两个接口 `GET /profiles/:profile_id` 和 `GET /creators/:creator_id` 两者都验证给定的 `profile_id` 是否存在，但是 `GET /creators/:creator_id` 还检查配置文件是否是创建者，那么最好将 `creator_id` 路径变量重命名为 `profile_id` 并将这两个依赖项链接起来。

```python
# src.profiles.dependencies
async def valid_profile_id(profile_id: UUID4) -> Mapping:
    profile = await service.get_by_id(post_id)
    if not profile:
        raise ProfileNotFound()

    return profile

# src.creators.dependencies
async def valid_creator_id(
    profile: Mapping = Depends(valid_profile_id)
) -> Mapping:
    if not profile["is_creator"]:
       raise ProfileNotCreator()

    return profile

# src.profiles.router.py
@router.get("/profiles/{profile_id}", response_model=ProfileResponse)
async def get_user_profile_by_id(profile: Mapping = Depends(valid_profile_id)):
    """Get profile by id."""
    return profile

# src.creators.router.py
@router.get("/creators/{profile_id}", response_model=ProfileResponse)
async def get_user_profile_by_id(
     creator_profile: Mapping = Depends(valid_creator_id)
):
    """Get creator's profile by id."""
    return creator_profile

```

使用 `/me` 路由定义来返回当前用户资源 (例如. `GET /profiles/me`, `GET /users/me/posts`)

   1. 无需检查用户ID是否存在 - 因为auth校验早已校验了其是否存在。
   2. 无需检查用户ID是否属于请求者

## 7. 如果你的路由只有阻塞的 I/O 操作, 不要让你的路由异步

在底层，FastAPI 可以[有效地处理](https://fastapi.tiangolo.com/async/#path-operation-functions) 异步和同步 I/O 操作。

- FastAPI 在[线程池](https://en.wikipedia.org/wiki/Thread_pool) 中运行`sync`(同步)路由，阻塞 I/O 操作不会阻止[事件循环](<https://docs.python.org/3/library/asyncio-eventloop.html>）执行任务。
- 否则，如果路由被定义为`async`，那么它会通过`await`定期调用，并且 FastAPI 相信您只会执行非阻塞 I/O 操作。

需要注意的是，如果您未能信任并在异步路由中执行阻塞操作，事件循环将无法运行下一个任务，直到该阻塞操作完成。

```python
import asyncio
import time

@router.get("/terrible-ping")
async def terrible_catastrophic_ping():
    time.sleep(10) # I/O阻塞操作10秒
    pong = service.get_pong()  # 从 DB 获取 pong 的 I/O 阻塞操作
    
    return {"pong": pong}

@router.get("/good-ping")
def good_ping():
    time.sleep(10) # I/O 阻塞操作 10 秒，但在另一个线程中
    pong = service.get_pong()  # 从数据库中获取 pong 的 I/O 阻塞操作，但在另一个线程中
    
    return {"pong": pong}

@router.get("/perfect-ping")
async def perfect_ping():
    await asyncio.sleep(10) # 异步阻塞 I/O 操作
    pong = await service.async_get_pong()  # 异步阻塞 I/O 数据库调用

    return {"pong": pong}

```

!!! note  "**当我们调用时会发生什么**"

    === "`GET /terrible-ping`"
    
        1. FastAPI 服务器收到一个请求并开始处理它
        2. 服务器的事件循环和队列中的所有任务将等待直到 `time.sleep()` 完成
            1. 服务器认为 `time.sleep()` 不是一个 I/O 任务, 所以会一直等待并直到它完成。
            2. 在等待期间服务器不会接受任何新的请求。
        3. 然后, 事件循环和所有任务会在队列中一起等待，直到`service.get_pong`执行完毕。
            1. 服务器认为 `service.get_pong()` 不是一个 I/O 任务, 所以他会一直等待，直到它完成。
            2. 服务器在等待期间不会接受任何新的的请求。
        4. 服务器返回响应。
            1. 响应之后, 服务器开始接受新的请求。
    
    ===  "`GET /good-ping`"
    
        1. FastAPI 服务器收到一个请求并开始处理它
        2. FastAPI 将整个路由 `good_ping` 分配到线程池中, 池中有工作线程负责运行该路由绑定的函数。
        3. 在 `good_ping` 执行其间, 事件循环会从队列中选择下一个任务和工作线程给他们， (比如. 接受新请求, 调用数据库)
            - 工作线程独立于主线程 (比如. 我们的 FastAPI 应用程序), 它将等待 `time.sleep` 完成，然后等待 `service.get_pong` 完成。
            - Sync(同步)操作只会阻塞子线程，不会阻塞主线程。
        4. 然后 `good_ping` 完成他的工作, 服务器返回一个响应给客户端。
    
    ===  "`GET /perfect-ping`"
    
        1. FastAPI 服务器收到一个请求并开始处理它
        2. FastAPI 异步等待 `asyncio.sleep(10)`
        3. 事件循环从队列中选择下一个任务并处理它们  (比如. 接受新请求, 调用数据库)
        4. 当 `asyncio.sleep(10)` 完成时, 服务器转到下一行并等待 `service.async_get_pong` 完成。
        5. 事件循环从队列中选择下一个任务并处理它们  (比如. 接受新请求, 调用数据库)
        6. 当 `service.async_get_pong` 完成, 服务器返回一个响应给客户端。

第二个需要强调的是，non-blocking awaitables (非阻塞等待)或者发送到线程池的操作必须是I/O密集型任务(比如: 打开文件、数据库调用、外部API调用等等)。

- 等待CPU密集型任务 (比如. 负责的计算, 数据处理, 视频转码) 是没有价值的，因为CPU必须工作才能完成计算任务, 而I/O操作是外部的，服务器在等待该操作完成时什么都不做，因此它可以去做下一个任务。
- 由于 [GIL](https://realpython.com/python-gil/)，在其他线程中运行 CPU 密集型任务也不是有效的。 简而言之，GIL 一次只允许一个线程工作，这使得它对 CPU密集型任务毫无用处。
- 如果你想优化 CPU 密集型任务，你应该将它们发送给另一个进程中进行工作。

**StackOverflow上困惑用户的相关问题**:

1. <https://stackoverflow.com/questions/62976648/architecture-flask-vs-fastapi/70309597#70309597> - 同样可以看看 [我的回答](https://stackoverflow.com/a/70309597/6927498)
2. <https://stackoverflow.com/questions/65342833/fastapi-uploadfile-is-slow-compared-to-flask>
3. <https://stackoverflow.com/questions/71516140/fastapi-runs-api-calls-in-serial-instead-of-parallel-fashion>

## 8. 从第 0 天开始的自定义基础模型

拥有可控的全局基础模型使我们能够自定义应用程序中的所有模型。

例如，我们可以有一个标准的日期时间格式或为基础模型的所有子类添加一个超级方法。

```python
from datetime import datetime
from zoneinfo import ZoneInfo

import orjson
from fastapi.encoders import jsonable_encoder
from pydantic import BaseModel, root_validator


def orjson_dumps(v, *, default):
    # orjson.dumps 返回字节，为了匹配标准的 json.dumps, 我们需要解码。
    return orjson.dumps(v, default=default).decode()


def convert_datetime_to_gmt(dt: datetime) -> str:
    if not dt.tzinfo:
        dt = dt.replace(tzinfo=ZoneInfo("UTC"))

    return dt.strftime("%Y-%m-%dT%H:%M:%S%z")


class ORJSONModel(BaseModel):
    class Config:
        json_loads = orjson.loads
        json_dumps = orjson_dumps
        json_encoders = {datetime: convert_datetime_to_gmt}  # 日期时间字段的自定义 JSON 编码方法

    @root_validator()
    def set_null_microseconds(cls, data: dict) -> dict:
       """在所有日期时间字段值中删除微秒。"""
        datetime_fields = {
            k: v.replace(microsecond=0)
            for k, v in data.items()
            if isinstance(k, datetime)
        }

        return {**data, **datetime_fields}

    def serializable_dict(self, **kwargs):
       """返回一个只包含可序列化字段的字典。"""
        default_dict = super().dict(**kwargs)

        return jsonable_encoder(default_dict)
```

在上面的示例中，我们决定制作一个全局基础模型：

- 使用 [orjson](https://github.com/ijl/orjson) 用于数据的序列化
- 在所有日期格式中将微秒降为 0
- 将所有日期时间字段序列化为具有显式时区的标准格式

## 9. 文档(Docs)

1. 除非您的 API 是公开的，否则默认情况下隐藏文档。 仅在选定的环境中明确显示它。

    ```python
    from fastapi import FastAPI
    from starlette.config import Config
    
    config = Config(".env")  # 解析 .env 文件中的环境变量
    
    ENVIRONMENT = config("ENVIRONMENT")  # 获取当前环境名称
    SHOW_DOCS_ENVIRONMENT = ("local", "staging")  # 允许显示文档的环境名称列表
    
    app_configs = {"title": "My Cool API"}
    if ENVIRONMENT not in SHOW_DOCS_ENVIRONMENT:
       app_configs["openapi_url"] = None  # 将文档的 url 设置为 null
    
    app = FastAPI(**app_configs)
    ```

2. 帮助FastAPI生成通俗易懂的文档
   1. 设置 `response_model`, `status_code`, `description`, 等字段.
   2. 如果响应模型和状态不同，使用 `responses` 路由属性为不同的响应添加文档

```python
from fastapi import APIRouter, status

router = APIRouter()

@router.post(
    "/endpoints",
    response_model=DefaultResponseModel,  # 默认响应的 pydantic 模型
    status_code=status.HTTP_201_CREATED,  # 默认状态码
    description="文档接口的清晰描述",
    tags=["Endpoint Category"],  # 接口分类
    summary="Summary of the Endpoint",  # 接口概要
    responses={
        status.HTTP_200_OK: {
            "model": OkResponse, # 自定义 pydantic 模型，用于 200 响应
            "description": "Ok Response",
        },
        status.HTTP_201_CREATED: {
            "model": CreatedResponse,  # 自定义 pydantic 模型，用于 201 响应
            "description": "Creates something from user request ",
        },
        status.HTTP_202_ACCEPTED: {
            "model": AcceptedResponse,  # 自定义 pydantic 模型，用于 202 响应
            "description": "Accepts request and handles it later",
        },
    },
)
async def documented_route():
    pass
```

即生成的文档就像这样: ![FastAPI Generated Custom Response Docs](images/custom_responses.png "自定义响应文档")

## 10. 使用 Pydantic 的 BaseSettings 进行配置

Pydantic 提供了一个[强大的工具](https://pydantic-docs.helpmanual.io/usage/settings/) 来解析环境变量并使用其验证器处理它们。

```python
from pydantic import AnyUrl, BaseSettings, PostgresDsn

class AppSettings(BaseSettings):
    class Config:
        env_file = ".env"
        env_file_encoding = "utf-8"
        env_prefix = "app_"

    DATABASE_URL: PostgresDsn
    IS_GOOD_ENV: bool = True
    ALLOWED_CORS_ORIGINS: set[AnyUrl]
```

## 11. SQLAlchemy: 设置数据库键命名约定

根据您的数据库约定明确设置索引的命名优于 `sqlalchemy` 自动命名。

```python
from sqlalchemy import MetaData

POSTGRES_INDEXES_NAMING_CONVENTION = {
    "ix": "%(column_0_label)s_idx",
    "uq": "%(table_name)s_%(column_0_name)s_key",
    "ck": "%(table_name)s_%(constraint_name)s_check",
    "fk": "%(table_name)s_%(column_0_name)s_fkey",
    "pk": "%(table_name)s_pkey",
}
metadata = MetaData(naming_convention=POSTGRES_INDEXES_NAMING_CONVENTION)
```

## 12. 迁移: Alembic

1. 迁移必须是静态的和可恢复的。 如果您的迁移依赖于动态生成的数据，那么请确保唯一动态的是数据本身，而不是其结构。
2. 生成具有描述性名称和 slug 的迁移。 Slug 是必需的，应该解释这些变化。
3. 为新迁移设置人类可读的文件模板。 我们使用 `*date*_*slug*.py` 模式，例如 `2022-08-24_post_content_idx.py`

```ini
# alembic.ini
file_template = %%(year)d-%%(month).2d-%%(day).2d_%%(slug)s
```

## 13. 设置数据库(表/字典)命名约定

命名保持一致很重要。 我们遵循的一些规则：

1. lower_case_snake (小写驼峰命名)
2. 单数形式 (例如. `post`, `post_like`, `user_playlist`)
3. 使用模块前缀对相似表进行分组, 例如. `payment_account`, `payment_bill`, `post`, `post_like`
4. 跨表命名保持一致，但具体的命名是可以的, 例如.
   1. 在所有表中使用 `profile_id`，但如果其中一些只需要作为创建者的配置文件，请使用 `creator_id`
   2. 在所有抽象表，形如 `post_like` 、 `post_view` 中使用 `post_id` ，但在相关模块中使用具体命名，如 `chapters.course_id` 中的 `course_id` 。
5. `_at` 作为 `datetime` 类型的后缀
6. `_date` 作为 `date` 类型的后缀

## 14. 从第0天开始写基于异步的测试

基于DB写集成测试很有可能导致在将来出现基于事件循环的错误。

立即开始基于异步测试客户端的测试， 例如. [async_asgi_testclient](https://github.com/vinissimus/async-asgi-testclient) 或 [httpx](https://github.com/encode/starlette/issues/652)

```python
import pytest
from async_asgi_testclient import TestClient

from src.main import app  # inited FastAPI app


@pytest.fixture
async def client():
    host, port = "127.0.0.1", "5555"
    scope = {"client": (host, port)}

    async with TestClient(
        app, scope=scope, headers={"X-User-Fingerprint": "Test"}
    ) as client:
        yield client


@pytest.mark.asyncio
async def test_create_post(client: TestClient):
    resp = await client.post("/posts")

    assert resp.status_code == 201
```

除非你有同步到数据库连接（打扰了？）或者不打算编写集成测试。

## 15. 后台任务使用 asyncio.create_task

BackgroundTasks 可以 [有效运行](https://github.com/encode/starlette/blob/31164e346b9bd1ce17d968e1301c3bb2c23bb418/starlette/background.py#L25)
所有的阻塞和非阻塞I/O操作, 就像FastAPI 处理阻塞路由一样。 (`sync`(同步)任务在线程池中运行, 而 `async`(异步) 人物则等待中稍后运行。)

- 不要欺骗工作例程以及将阻塞的I/O操作标记为`async`(异步).（这样做的话，会阻塞事件调用循环，导致影响其他的异步任务调用）
- 不要将它用于繁重的CPU密集型任务。

```python
from fastapi import APIRouter, BackgroundTasks
from pydantic import UUID4

from src.notifications import service as notifications_service


router = APIRouter()


@router.post("/users/{user_id}/email")
async def send_user_email(worker: BackgroundTasks, user_id: UUID4):
    """Send email to user"""
    worker.add_task(notifications_service.send_email, user_id)  # send email after responding client
    return {"status": "ok"}
```

## 16. 类型注解很重要

FastAPI, Pydantic, 以及现代的 IDE 鼓励使用类型提示。

**没有类型提示**:

![type_hintsless](./images/type_hintsless.png)

**有类型提示**:

![type_hints](./images/type_hints.png)

## 17. 以chunks(块)的形式保存文件

不要期望您的客户端发送小文件。

```python
import aiofiles
from fastapi import UploadFile

DEFAULT_CHUNK_SIZE = 1024 * 1024 * 50  # 50 megabytes MB(兆字节)

async def save_video(video_file: UploadFile):
   async with aiofiles.open("/file/path/name.mp4", "wb") as f:
     while chunk := await video_file.read(DEFAULT_CHUNK_SIZE):
         await f.write(chunk)
```

## 18. 小心pydantic的动态字段

如果你有一个可以接受联合类型(Union)的 pydantic 字段，请确保验证器明确知道这些类型之间的区别。

```python
from pydantic import BaseModel


class Article(BaseModel):
   text: str | None
   extra: str | None


class Video(BaseModel):
   video_id: int
   text: str | None
   extra: str | None

   
class Post(BaseModel):
   content: Article | Video

   
post = Post(content={"video_id": 1, "text": "text"})
print(type(post.content))
# OUTPUT: Article
# Article 非常包容，所有字段都是可选的，允许任何字典生效
```

**解决方案:**

1. 验证输入只允许有效字段并在提供未知数时引发错误

    ```python
    from pydantic import BaseModel, Extra
    
    class Article(BaseModel):
       text: str | None
       extra: str | None
       
       class Config:
            extra = Extra.forbid
           
    
    class Video(BaseModel):
       video_id: int
       text: str | None
       extra: str | None
       
       class Config:
            extra = Extra.forbid
    
       
    class Post(BaseModel):
       content: Article | Video
    ```

2. 如果字段很简单，请使用 Pydantic 的 Smart Union (>v1.9)

    如果字段很简单，如 `int` 或 `bool`，这是一个很好的解决方案，但它不适用于类等复杂字段。

    没有 Smart Union :

    ```python
    from pydantic import BaseModel
    
    
    class Post(BaseModel):
       field_1: bool | int
       field_2: int | str
       content: Article | Video
    
    p = Post(field_1=1, field_2="1", content={"video_id": 1})
    print(p.field_1)
    # OUTPUT: True
    print(type(p.field_2))
    # OUTPUT: int
    print(type(p.content))
    # OUTPUT: Article
    ```

    有 Smart Union :

    ```python
    class Post(BaseModel):
       field_1: bool | int
       field_2: int | str
       content: Article | Video
    
       class Config:
          smart_union = True
    
    
    p = Post(field_1=1, field_2="1", content={"video_id": 1})
    print(p.field_1)
    # OUTPUT: 1
    print(type(p.field_2))
    # OUTPUT: str
    print(type(p.content))
    # OUTPUT: Article, 因为 smart_union 不适用于像类这样的复杂字段
    ```

3. 快速解决方法

    正确排序字段类型: 从最严格的到宽松的校验。

    ```python
    class Post(BaseModel):
       content: Video | Article
    ```

## 19. SQL-第一, Pydantic-第二

- 通常，数据库处理数据的速度比 CPython 更快、更干净。
- 最好使用 SQL 执行所有复杂的连接和简单的数据操作。
- 最好在数据库中聚合 JSON 以响应嵌套对象。

```python
# src.posts.service
from typing import Mapping

from pydantic import UUID4
from sqlalchemy import desc, func, select, text
from sqlalchemy.sql.functions import coalesce

from src.database import database, posts, profiles, post_review, products

async def get_posts(
    creator_id: UUID4, *, limit: int = 10, offset: int = 0
) -> list[Mapping]: 
    select_query = (
        select(
            (
                posts.c.id,
                posts.c.type,
                posts.c.slug,
                posts.c.title,
                func.json_build_object(
                   text("'id', profiles.id"),
                   text("'first_name', profiles.first_name"),
                   text("'last_name', profiles.last_name"),
                   text("'username', profiles.username"),
                ).label("creator"),
            )
        )
        .select_from(posts.join(profiles, posts.c.owner_id == profiles.c.id))
        .where(posts.c.owner_id == creator_id)
        .limit(limit)
        .offset(offset)
        .group_by(
            posts.c.id,
            posts.c.type,
            posts.c.slug,
            posts.c.title,
            profiles.c.id,
            profiles.c.first_name,
            profiles.c.last_name,
            profiles.c.username,
            profiles.c.avatar,
        )
        .order_by(
            desc(coalesce(posts.c.updated_at, posts.c.published_at, posts.c.created_at))
        )
    )
    
    return await database.fetch_all(select_query)

# src.posts.schemas
import orjson
from enum import Enum

from pydantic import BaseModel, UUID4, validator


class PostType(str, Enum):
    ARTICLE = "ARTICLE"
    COURSE = "COURSE"

   
class Creator(BaseModel):
    id: UUID4
    first_name: str
    last_name: str
    username: str


class Post(BaseModel):
    id: UUID4
    type: PostType
    slug: str
    title: str
    creator: Creator

    @validator("creator", pre=True)  # before default validation
    def parse_json(cls, creator: str | dict | Creator) -> dict | Creator:
       if isinstance(creator, str):  # i.e. json
          return orjson.loads(creator)

       return creator
    
# src.posts.router
from fastapi import APIRouter, Depends

router = APIRouter()


@router.get("/creators/{creator_id}/posts", response_model=list[Post])
async def get_creator_posts(creator: Mapping = Depends(valid_creator_id)):
   posts = await service.get_posts(creator["id"])

   return posts
```

如果聚合数据表单 DB 是一个简单的 JSON，那么看看 Pydantic 的`Json`字段类型，它将首先加载原始 JSON。

```python
from pydantic import BaseModel, Json

class A(BaseModel):
    numbers: Json[list[int]]
    dicts: Json[dict[str, int]]

valid_a = A(numbers="[1, 2, 3]", dicts='{"key": 1000}')  # becomes A(numbers=[1,2,3], dicts={"key": 1000})
invalid_a = A(numbers='["a", "b", "c"]', dicts='{"key": "str instead of int"}')  # raises ValueError
```

## 20. 验证host，如果用户可以发送公开可用的 URL

例如，我们有一个特定的入口：

1. 接受来自用户的媒体文件，
2. 为此文件生成唯一的 url，
3. 返回 url 给用户，
   1. 他们将在其他入口使用它们，例如 `PUT /profiles/me`, `POST /posts`
   2. 这些端点只接受来自白名单主机的文件
4. 使用此名称和匹配的 URL 将文件上传到 AWS。

如果我们不将 URL 主机列入白名单，那么不良用户就有机会上传危险链接。

```python
from pydantic import AnyUrl, BaseModel

ALLOWED_MEDIA_URLS = {"mysite.com", "mysite.org"}

class CompanyMediaUrl(AnyUrl):
    @classmethod
    def validate_host(cls, parts: dict) -> tuple[str, str, str, bool]:
       """将 pydantic 的 AnyUrl 验证扩展到白名单 URL 主机。"""
        host, tld, host_type, rebuild = super().validate_host(parts)
        if host not in ALLOWED_MEDIA_URLS:
            raise ValueError(
                "Forbidden host url. Upload files only to internal services."
            )

        return host, tld, host_type, rebuild


class Profile(BaseModel):
    avatar_url: CompanyMediaUrl  # only whitelisted urls for avatar

```

## 21. 如果schema直接面向客户端，在pydantic的自定义校验中抛出ValueError

它将向用户返回一个很好的详细响应。

```python
# src.profiles.schemas
from pydantic import BaseModel, validator

class ProfileCreate(BaseModel):
    username: str
    
    @validator("username")
    def validate_bad_words(cls, username: str):
        if username  == "me":
            raise ValueError("bad username, choose another")
        
        return username


# src.profiles.routes
from fastapi import APIRouter

router = APIRouter()


@router.post("/profiles")
async def get_creator_posts(profile_data: ProfileCreate):
   pass
```

**Response 例子:**

![custom_bad_response](./images/custom_bad_response.png)

## 22. 不要忘记 FastAPI 将 Response 的 Pydantic 对象转换为 Dict，然后转换为 ResponseModel 的实例，然后转换为 Dict，然后转换为 JSON

```python
from fastapi import FastAPI
from pydantic import BaseModel, root_validator

app = FastAPI()


class ProfileResponse(BaseModel):
    @root_validator
    def debug_usage(cls, data: dict):
        print("created pydantic model")

        return data

    def dict(self, *args, **kwargs):
        print("called dict")
        return super().dict(*args, **kwargs)


@app.get("/", response_model=ProfileResponse)
async def root():
    return ProfileResponse()
```

**日志输出:**

```text
[INFO] [2022-08-28 12:00:00.000000] created pydantic model
[INFO] [2022-08-28 12:00:00.000010] called dict
[INFO] [2022-08-28 12:00:00.000020] created pydantic model
[INFO] [2022-08-28 12:00:00.000030] called dict
```

## 23. 如果你必须使用sync(同步) SDK, 请在线程池中运行

如果您必须使用库与外部服务交互，并且它不支持`async`(异步)，则在外部工作线程中进行 HTTP 调用。

举个简单的例子，我们可以使用来自 starlette 的著名的`run_in_threadpool`。

```python
from fastapi import FastAPI
from fastapi.concurrency import run_in_threadpool
from my_sync_library import SyncAPIClient 

app = FastAPI()


@app.get("/")
async def call_my_sync_library():
    my_data = await service.get_my_data()

    client = SyncAPIClient()
    await run_in_threadpool(client.make_request, data=my_data)
```

## 24. 使用 linters (black, isort, autoflake)

使用 linters, 您可以忘记格式化代码并专注于编写业务逻辑。

Black 是一个毫不妥协的代码格式化程序，它消除了您在开发过程中必须做出的许多小决定。
其他 linter 可帮助您编写更清晰的代码并遵循 PEP8。

使用`pre-commit hook`(预提交钩子)是一种流行的良好做法，但仅使用脚本对我们来说就可以了。

```shell
#!/bin/sh -e
set -x

autoflake --remove-all-unused-imports --recursive --remove-unused-variables --in-place src tests --exclude=__init__.py
isort src tests --profile black
black src tests
```

## 惊喜部分

一些非常善良的人分享了他们自己的经验和最佳实践，绝对值得一读。

在项目的 [issues](https://github.com/zhanymkanov/fastapi-best-practices/issues) 部分查看它们。

例如，[lowercase00](https://github.com/zhanymkanov/fastapi-best-practices/issues/4) 详细描述了他们使用权限和授权、基于类的服务和视图、任务队列的最佳实践， 自定义响应序列化程序，使用 [dynaconf](https://www.dynaconf.com/) 进行配置等。

如果您有任何关于使用 FastAPI 的经验要分享，无论是好是坏，都非常欢迎您创建一个新问题。 阅读它是我们的荣幸。
