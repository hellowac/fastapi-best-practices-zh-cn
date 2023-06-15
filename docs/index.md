# FastAPI 最佳实践

我们在创业时使用的并且自以为是的最佳实践和惯例列表。

在过去 1.5 年的生产中，
我们一直在做出好的和坏的决定，这些决定极大地影响了我们的开发人员体验。
其中一些值得分享。

## 1. 项目结构。 一致且可预测

There are many ways to structure the project, but the best structure is a structure that is consistent, straightforward, and has no surprises.

- If looking at the project structure doesn't give you an idea of what the project is about, then the structure might be unclear.
- If you have to open packages to understand what modules are located in them, then your structure is unclear.
- If the frequency and location of the files feels random, then your project structure is bad.
- If looking at the module's location and its name doesn't give you an idea of what's inside it, then your structure is very bad.

Although the project structure, where we separate files by their type (e.g. api, crud, models, schemas)
presented by [@tiangolo](https://github.com/tiangolo) is good for microservices or projects with fewer scopes,
we couldn't fit it into our monolith with a lot of domains and modules.
Structure that I found more scalable and evolvable is inspired by Netflix's [Dispatch](https://github.com/Netflix/dispatch) with some little modifications.

```text
fastapi-project
├── alembic/
├── src
│   ├── auth
│   │   ├── router.py
│   │   ├── schemas.py  # pydantic models
│   │   ├── models.py  # db models
│   │   ├── dependencies.py
│   │   ├── config.py  # local configs
│   │   ├── constants.py
│   │   ├── exceptions.py
│   │   ├── service.py
│   │   └── utils.py
│   ├── aws
│   │   ├── client.py  # client model for external service communication
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
│   ├── config.py  # global configs
│   ├── models.py  # global models
│   ├── exceptions.py  # global exceptions
│   ├── pagination.py  # global module e.g. pagination
│   ├── database.py  # db connection related stuff
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

1. Store all domain directories inside `src` folder
   1. `src/` - highest level of an app, contains common models, configs, and constants, etc.
   2. `src/main.py` - root of the project, which inits the FastAPI app
2. Each package has its own router, schemas, models, etc.
   1. `router.py` - is a core of each module with all the endpoints
   2. `schemas.py` - for pydantic models
   3. `models.py` - for db models
   4. `service.py` - module specific business logic  
   5. `dependencies.py` - router dependencies
   6. `constants.py` - module specific constants and error codes
   7. `config.py` - e.g. env vars
   8. `utils.py` - non-business logic functions, e.g. response normalization, data enrichment, etc.
   9. `exceptions` - module specific exceptions, e.g. `PostNotFound`, `InvalidUserData`
3. When package requires services or dependencies or constants from other packages - import them with an explicit module name

```python
from src.auth import constants as auth_constants
from src.notifications import service as notification_service
from src.posts.constants import ErrorCode as PostsErrorCode  # in case we have Standard ErrorCode in constants module of each package
```

## 2. 过度使用 Pydantic 进行数据验证

Pydantic has a rich set of features to validate and transform data.

In addition to regular features like required & non-required fields with default values,
Pydantic has built-in comprehensive data processing tools like regex, enums for limited allowed options, length validation, email validation, etc.

```python3
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

## 3. 使用依赖项进行数据验证与数据库

Pydantic can only validate the values from client input.
Use dependencies to validate data against database constraints like email already exists, user not found, etc.

```python3
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

If we didn't put data validation to dependency, we would have to add post_id validation
for every endpoint and write the same tests for each of them.

## 4. 依赖(Dependency)链

Dependencies can use other dependencies and avoid code repetition for similar logic.

```python3
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

Dependencies can be reused multiple times, and they won't be recalculated - FastAPI caches dependency's result within a request's scope by default,
i.e. if we have a dependency that calls service `get_post_by_id`, we won't be visiting DB each time we call this dependency - only the first function call.

Knowing this, we can easily decouple dependencies onto multiple smaller functions that operate on a smaller domain and are easier to reuse in other routes.
For example, in the code below we are using `parse_jwt_data` three times:

1. `valid_owned_post`
2. `valid_active_creator`
3. `get_user_post`,

but `parse_jwt_data` is called only once, in the very first call.

```python3
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

Developing RESTful API makes it easier to reuse dependencies in routes like these:

   1. `GET /courses/:course_id`
   2. `GET /courses/:course_id/chapters/:chapter_id/lessons`
   3. `GET /chapters/:chapter_id`

The only caveat is to use the same variable names in the path:

- If you have two endpoints `GET /profiles/:profile_id` and `GET /creators/:creator_id`
that both validate whether the given `profile_id` exists,  but `GET /creators/:creator_id`
also checks if the profile is creator, then it's better to rename `creator_id` path variable to `profile_id` and chain those two dependencies.

```python3
# src.profiles.dependencies
async def valid_profile_id(profile_id: UUID4) -> Mapping:
    profile = await service.get_by_id(post_id)
    if not profile:
        raise ProfileNotFound()

    return profile

# src.creators.dependencies
async def valid_creator_id(profile: Mapping = Depends(valid_profile_id)) -> Mapping:
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

Use /me endpoints for users resources (e.g. `GET /profiles/me`, `GET /users/me/posts`)

   1. No need to validate that user id exists - it's already checked via auth method
   2. No need to check whether the user id belongs to the requester

## 7. 不要让你的路由异步，如果你只有阻塞 I/O 操作

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
    await asyncio.sleep(10) # 非阻塞 I/O 操作
    pong = await service.async_get_pong()  # 非阻塞 I/O 数据库调用

    return {"pong": pong}

```

**当我们调用时会发生什么:**

1. `GET /terrible-ping`
   1. FastAPI server receives a request and starts handling it
   2. Server's event loop and all the tasks in the queue will be waiting until `time.sleep()` is finished
      1. Server thinks `time.sleep()` is not an I/O task, so it waits until it is finished
      2. Server won't accept any new requests while waiting
   3. Then, event loop and all the tasks in the queue will be waiting until `service.get_pong` is finished
      1. Server thinks `service.get_pong()` is not an I/O task, so it waits until it is finished
      2. Server won't accept any new requests while waiting
   4. Server returns the response.
      1. After a response, server starts accepting new requests
2. `GET /good-ping`
   1. FastAPI server receives a request and starts handling it
   2. FastAPI sends the whole route `good_ping` to the threadpool, where a worker thread will run the function
   3. While `good_ping` is being executed, event loop selects next tasks from the queue and works on them (e.g. accept new request, call db)
      - Independently of main thread (i.e. our FastAPI app),
        worker thread will be waiting for `time.sleep` to finish and then for `service.get_pong` to finish
      - Sync operation blocks only the side thread, not the main one.
   4. When `good_ping` finishes its work, server returns a response to the client
3. `GET /perfect-ping`
   1. FastAPI server receives a request and starts handling it
   2. FastAPI awaits `asyncio.sleep(10)`
   3. Event loop selects next tasks from the queue and works on them (e.g. accept new request, call db)
   4. When `asyncio.sleep(10)` is done, servers goes to the next lines and awaits `service.async_get_pong`
   5. Event loop selects next tasks from the queue and works on them (e.g. accept new request, call db)
   6. When `service.async_get_pong` is done, server returns a response to the client

The second caveat is that operations that are non-blocking awaitables or are sent to the thread pool must be I/O intensive tasks (e.g. open file, db call, external API call).

- Awaiting CPU-intensive tasks (e.g. heavy calculations, data processing, video transcoding) is worthless since the CPU has to work to finish the tasks,
while I/O operations are external and server does nothing while waiting for that operations to finish, thus it can go to the next tasks.
- Running CPU-intensive tasks in other threads also isn't effective, because of [GIL](https://realpython.com/python-gil/).
In short, GIL allows only one thread to work at a time, which makes it useless for CPU tasks.
- If you want to optimize CPU intensive tasks you should send them to workers in another process.

**Related StackOverflow questions of confused users**

1. <https://stackoverflow.com/questions/62976648/architecture-flask-vs-fastapi/70309597#70309597>
   - Here you can also check [my answer](https://stackoverflow.com/a/70309597/6927498)
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
        json_encoders = {datetime: convert_datetime_to_gmt}  # method for customer JSON encoding of datetime fields

    @root_validator()
    def set_null_microseconds(cls, data: dict) -> dict:
       """Drops microseconds in all the datetime field values."""
        datetime_fields = {
            k: v.replace(microsecond=0)
            for k, v in data.items()
            if isinstance(k, datetime)
        }

        return {**data, **datetime_fields}

    def serializable_dict(self, **kwargs):
       """Return a dict which contains only serializable fields."""
        default_dict = super().dict(**kwargs)

        return jsonable_encoder(default_dict)
```

In the example above we have decided to make a global base model which:

- uses [orjson](https://github.com/ijl/orjson) to serialize data
- drops microseconds to 0 in all date formats
- serializes all datetime fields to standard format with explicit timezone

## 9. 文档(Docs)

1. Unless your API is public, hide docs by default. Show it explicitly on the selected envs only.

```python
from fastapi import FastAPI
from starlette.config import Config

config = Config(".env")  # parse .env file for env variables

ENVIRONMENT = config("ENVIRONMENT")  # get current env name
SHOW_DOCS_ENVIRONMENT = ("local", "staging")  # explicit list of allowed envs

app_configs = {"title": "My Cool API"}
if ENVIRONMENT not in SHOW_DOCS_ENVIRONMENT:
   app_configs["openapi_url"] = None  # set url for docs as null

app = FastAPI(**app_configs)
```

2. Help FastAPI to generate an easy-to-understand docs
   1. Set `response_model`, `status_code`, `description`, etc.
   2. If models and statuses vary, use `responses` route attribute to add docs for different responses

```python
from fastapi import APIRouter, status

router = APIRouter()

@router.post(
    "/endpoints",
    response_model=DefaultResponseModel,  # default response pydantic model 
    status_code=status.HTTP_201_CREATED,  # default status code
    description="Description of the well documented endpoint",
    tags=["Endpoint Category"],
    summary="Summary of the Endpoint",
    responses={
        status.HTTP_200_OK: {
            "model": OkResponse, # custom pydantic model for 200 response
            "description": "Ok Response",
        },
        status.HTTP_201_CREATED: {
            "model": CreatedResponse,  # custom pydantic model for 201 response
            "description": "Creates something from user request ",
        },
        status.HTTP_202_ACCEPTED: {
            "model": AcceptedResponse,  # custom pydantic model for 202 response
            "description": "Accepts request and handles it later",
        },
    },
)
async def documented_route():
    pass
```

Will generate docs like this:
![FastAPI Generated Custom Response Docs](images/custom_responses.png "Custom Response Docs")

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

Explicitly setting the indexes' namings according to your database's convention is preferable over sqlalchemy's.

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

BackgroundTasks can [effectively run](https://github.com/encode/starlette/blob/31164e346b9bd1ce17d968e1301c3bb2c23bb418/starlette/background.py#L25)
both blocking and non-blocking I/O operations the same way FastAPI handles blocking routes (`sync` tasks are run in a threadpool, while `async` tasks are awaited later)

- Don't lie to the worker and don't mark blocking I/O operations as `async`
- Don't use it for heavy CPU intensive tasks.

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

```
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

With linters, you can forget about formatting the code and focus on writing the business logic.

Black is the uncompromising code formatter that eliminates so many small decisions you have to make during development.
Other linters help you write cleaner code and follow the PEP8.

It's a popular good practice to use pre-commit hooks, but just using the script was ok for us.

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

例如，[lowercase00](https://github.com/zhanymkanov/fastapi-best-practices/issues/4) 详细描述了他们使用权限和授权、基于类的服务和视图、任务队列的最佳实践， 自定义响应序列化程序，使用 dynaconf 进行配置等。

如果您有任何关于使用 FastAPI 的经验要分享，无论是好是坏，都非常欢迎您创建一个新问题。 阅读它是我们的荣幸。
