# 类型字典

**TypedDict**

=== "中文"

    Python 程序经常使用带有字符串键的字典来表示对象。 `TypedDict` 允许您为表示具有固定模式的对象的字典提供精确的类型，例如 `{'id': 1, 'items': ['x']}`。

    这是一个典型的例子：

    ```python
    movie = {'name': 'Blade Runner', 'year': 1982}
    ```

    仅需要一组固定的字符串键（上面的“name”和“year”），并且每个键都有一个独立的值类型（“str”代表“name”，“int”代表“year” '` 上面）。 我们之前已经看到过“dict[K, V]”类型，它允许您声明统一的字典类型，其中每个值都具有相同的类型，并且支持任意键。 这显然不太适合上面的“电影”。 相反，您可以使用“TypedDict”为“movie”等对象提供精确的类型，其中每个字典值的类型取决于键：

    ```python
    from typing_extensions import TypedDict

    Movie = TypedDict('Movie', {'name': str, 'year': int})

    movie: Movie = {'name': 'Blade Runner', 'year': 1982}
    ```

    `Movie` 是一个 `TypedDict` 类型，包含两项：`'name'`（类型为 `str`）和 `'year'`（类型为 `int`）。 请注意，我们对“movie”变量使用了显式类型注释。 这种类型注释很重要——没有它，mypy 将尝试为“movie”推断出常规的、统一的 [`dict`](https://docs.python.org/3/library/stdtypes.html#dict) 类型 ，这不是我们想要的。

    !!! info "Note"

        如果将 `TypedDict` 对象作为参数传递给函数，通常不需要类型注释，因为 mypy 可以根据声明的参数类型推断所需的类型。 另外，如果之前已经定义了赋值目标，并且它具有 `TypedDict` 类型，则 mypy 会将分配的值视为 `TypedDict`，而不是 [`dict`](https://docs.python.org/3/library/stdtypes.html#dict)。

    现在 mypy 会识别这些是有效的：

    ```python
    name = movie['name']  # Okay; type of name is str
    year = movie['year']  # Okay; type of year is int
    ```

    Mypy 会将无效密钥检测为错误：

    ```python
    director = movie['director']  # Error: 'director' is not a valid key
    ```

    Mypy 还将拒绝运行时计算的表达式作为密钥，因为它无法验证它是否是有效的密钥。 您只能使用字符串文字作为“TypedDict”键。

    `TypedDict` 类型对象也可以充当构造函数。 它在运行时返回一个普通的 [`dict`](https://docs.python.org/3/library/stdtypes.html#dict) 对象——`TypedDict` 不定义新的运行时类型：

    ```python
    toy_story = Movie(name='Toy Story', year=1995)
    ```

    这相当于直接使用 `{ ... }` 或 `dict(key=value, ...)` 构造一个字典。 构造函数形式有时很方便，因为它可以在没有类型注释的情况下使用，并且它还使对象的类型显式化。

    与所有类型一样，“TypedDict”可以用作组件来构建任意复杂的类型。 例如，您可以使用“TypedDict”项定义嵌套的“TypedDict”和容器。 与大多数其他类型不同，mypy 使用 TypedDict 的结构兼容性检查（或结构子类型）。 假设项目类型兼容，具有额外项目的“TypedDict”对象与较窄的“TypedDict”（子类型）兼容（*totality*也会影响子类型，如下所述）。

    `TypedDict` 对象不是常规 `dict[...]` 类型的子类型（反之亦然），因为 [`dict`](https://docs.python.org/3/library/stdtypes.html#dict) 允许添加和删除任意键，与“TypedDict”不同。 然而，任何 `TypedDict` 对象都是 `Mapping[str, object]` 的子类型（即兼容），因为 [`Mapping`](https://docs.python.org/3/library/typing.html#typing.Mapping) 仅提供对字典项的只读访问：

    ```python
    def print_typed_dict(obj: Mapping[str, object]) -> None:
        for key, value in obj.items():
            print(f'{key}: {value}')

    print_typed_dict(Movie(name='Toy Story', year=1995))  # OK
    ```

    !!! info "Note"

        除非您使用的是 Python 3.8 或更高版本（其中标准库 [`typing`](https://docs.python.org/3/library/typing.html#module-typing) 模块中提供了 `TypedDict`），否则您需要使用 pip 安装 `typing_extensions` 以使用 `TypedDict`：

        ```text
        python3 -m pip install --upgrade typing-extensions
        ```

=== "英文"

    Python programs often use dictionaries with string keys to represent objects. `TypedDict` lets you give precise types for dictionaries that represent objects with a fixed schema, such as `{'id': 1, 'items': ['x']}`.

    Here is a typical example:

    ```python
    movie = {'name': 'Blade Runner', 'year': 1982}
    ```

    Only a fixed set of string keys is expected (`'name'` and `'year'` above), and each key has an independent value type (`str` for `'name'` and `int` for `'year'` above). We've previously seen the `dict[K, V]` type, which lets you declare uniform dictionary types, where every value has the same type, and arbitrary keys are supported. This is clearly not a good fit for `movie` above. Instead, you can use a `TypedDict` to give a precise type for objects like `movie`, where the type of each dictionary value depends on the key:

    ```python
    from typing_extensions import TypedDict

    Movie = TypedDict('Movie', {'name': str, 'year': int})

    movie: Movie = {'name': 'Blade Runner', 'year': 1982}
    ```

    `Movie` is a `TypedDict` type with two items: `'name'` (with type `str`) and `'year'` (with type `int`). Note that we used an explicit type annotation for the `movie` variable. This type annotation is important -- without it, mypy will try to infer a regular, uniform [`dict`](https://docs.python.org/3/library/stdtypes.html#dict) type for `movie`, which is not what we want here.

    !!! info "Note"

        If you pass a `TypedDict` object as an argument to a function, no type annotation is usually necessary since mypy can infer the desired type based on the declared argument type. Also, if an assignment target has been previously defined, and it has a `TypedDict` type, mypy will treat the assigned value as a `TypedDict`, not [`dict`](https://docs.python.org/3/library/stdtypes.html#dict).

    Now mypy will recognize these as valid:

    ```python
    name = movie['name']  # Okay; type of name is str
    year = movie['year']  # Okay; type of year is int
    ```

    Mypy will detect an invalid key as an error:

    ```python
    director = movie['director']  # Error: 'director' is not a valid key
    ```

    Mypy will also reject a runtime-computed expression as a key, as it can't verify that it's a valid key. You can only use string literals as `TypedDict` keys.

    The `TypedDict` type object can also act as a constructor. It returns a normal [`dict`](https://docs.python.org/3/library/stdtypes.html#dict) object at runtime -- a `TypedDict` does not define a new runtime type:

    ```python
    toy_story = Movie(name='Toy Story', year=1995)
    ```

    This is equivalent to just constructing a dictionary directly using `{ ... }` or `dict(key=value, ...)`. The constructor form is sometimes convenient, since it can be used without a type annotation, and it also makes the type of the object explicit.

    Like all types, `TypedDict`s can be used as components to build arbitrarily complex types. For example, you can define nested `TypedDict`s and containers with `TypedDict` items. Unlike most other types, mypy uses structural compatibility checking (or structural subtyping) with `TypedDict`s. A `TypedDict` object with extra items is compatible with (a subtype of) a narrower `TypedDict`, assuming item types are compatible (*totality* also affects subtyping, as discussed below).

    A `TypedDict` object is not a subtype of the regular `dict[...]` type (and vice versa), since [`dict`](https://docs.python.org/3/library/stdtypes.html#dict) allows arbitrary keys to be added and removed, unlike `TypedDict`. However, any `TypedDict` object is a subtype of (that is, compatible with) `Mapping[str, object]`, since [`Mapping`](https://docs.python.org/3/library/typing.html#typing.Mapping) only provides read-only access to the dictionary items:

    ```python
    def print_typed_dict(obj: Mapping[str, object]) -> None:
        for key, value in obj.items():
            print(f'{key}: {value}')

    print_typed_dict(Movie(name='Toy Story', year=1995))  # OK
    ```

    !!! info "Note"

        Unless you are on Python 3.8 or newer (where `TypedDict` is available in standard library [`typing`](https://docs.python.org/3/library/typing.html#module-typing) module) you need to install `typing_extensions` using pip to use `TypedDict`:

        ```text
        python3 -m pip install --upgrade typing-extensions
        ```

## 整体性

Totality

=== "中文"

    默认情况下，mypy 确保“TypedDict”对象具有所有指定的键。 这将被标记为错误：

    ```python
    # Error: 'year' missing
    toy_story: Movie = {'name': 'Toy Story'}
    ```

    有时您希望在创建“TypedDict”对象时允许省略键。 您可以向“TypedDict(...)”提供“total=False”参数来实现此目的：

    ```python
    GuiOptions = TypedDict(
        'GuiOptions', {'language': str, 'color': str}, total=False)
    options: GuiOptions = {}  # Okay
    options['language'] = 'en'
    ```

    您可能需要使用 [`dict.get`](https://docs.python.org/3/library/stdtypes.html#dict.get) 来访问部分（非全部）`TypedDict` 的项目， 因为使用 `[]` 进行索引可能会在运行时失败。 然而，mypy 仍然允许使用带有部分 `TypedDict` 的 `[]` ——你只需要小心它，因为它可能会导致 [`KeyError`](https://docs.python.org/3/library/exceptions.html#KeyError)。 到处都需要 [`dict.get`](https://docs.python.org/3/library/stdtypes.html#dict.get) 太麻烦了。 （请注意，您也可以自由使用 [`dict.get`](https://docs.python.org/3/library/stdtypes.html#dict.get) 以及所有 `TypedDict`。）

    不需要的键在错误消息中显示为“？”：

    ```python
    # Revealed type is "TypedDict('GuiOptions', {'language'?: builtins.str,
    #                                            'color'?: builtins.str})"
    reveal_type(options)
    ```

    整体性也会影响结构兼容性。 当需要完整的“TypedDict”时，您不能使用部分“TypedDict”。 此外，当需要部分“TypedDict”时，整个“TypedDict”无效。

=== "英文"

    By default mypy ensures that a `TypedDict` object has all the specified keys. This will be flagged as an error:

    ```python
    # Error: 'year' missing
    toy_story: Movie = {'name': 'Toy Story'}
    ```

    Sometimes you want to allow keys to be left out when creating a `TypedDict` object. You can provide the `total=False` argument to `TypedDict(...)` to achieve this:

    ```python
    GuiOptions = TypedDict(
        'GuiOptions', {'language': str, 'color': str}, total=False)
    options: GuiOptions = {}  # Okay
    options['language'] = 'en'
    ```

    You may need to use [`dict.get`](https://docs.python.org/3/library/stdtypes.html#dict.get) to access items of a partial (non-total) `TypedDict`, since indexing using `[]` could fail at runtime. However, mypy still lets use `[]` with a partial `TypedDict` -- you just need to be careful with it, as it could result in a [`KeyError`](https://docs.python.org/3/library/exceptions.html#KeyError). Requiring [`dict.get`](https://docs.python.org/3/library/stdtypes.html#dict.get) everywhere would be too cumbersome. (Note that you are free to use [`dict.get`](https://docs.python.org/3/library/stdtypes.html#dict.get) with total `TypedDict`s as well.)

    Keys that aren't required are shown with a `?` in error messages:

    ```python
    # Revealed type is "TypedDict('GuiOptions', {'language'?: builtins.str,
    #                                            'color'?: builtins.str})"
    reveal_type(options)
    ```

    Totality also affects structural compatibility. You can't use a partial `TypedDict` when a total one is expected. Also, a total `TypedDict` is not valid when a partial one is expected.

## 支持的操作

Supported operations

=== "中文"

    `TypedDict` 对象支持字典操作和方法的子集。 调用大多数方法时必须使用字符串文字作为键，否则 mypy 将无法检查该键是否有效。 支持的操作列表：

    - [`typing.Mapping`](https://docs.python.org/3/library/typing.html#typing.Mapping)中包含的任何内容:

      - `d[key]`
      - `key in d`
      - `len(d)`
      - `for key in d` (iteration)
      - [`d.get(key[, default])`](https://docs.python.org/3/library/stdtypes.html#dict.get)
      - [`d.keys()`](https://docs.python.org/3/library/stdtypes.html#dict.keys)
      - [`d.values()`](https://docs.python.org/3/library/stdtypes.html#dict.values)
      - [`d.items()`](https://docs.python.org/3/library/stdtypes.html#dict.items)

    - [`d.copy()`](https://docs.python.org/3/library/stdtypes.html#dict.copy)
    - [`d.setdefault(key, default)`](https://docs.python.org/3/library/stdtypes.html#dict.setdefault)
    - [`d1.update(d2)`](https://docs.python.org/3/library/stdtypes.html#dict.update)
    - [`d.pop(key[, default])`](https://docs.python.org/3/library/stdtypes.html#dict.pop) (partial `TypedDict`s only)
    - `del d[key]` (partial `TypedDict`s only)

    !!! info "Note"

        [`dict.clear`](https://docs.python.org/3/library/stdtypes.html#dict.clear) 和 [`dict.popitem`](https://docs.python.org/3/library/stdtypes.html#dict.popitem) 不支持，因为它们不安全——它们可以删除由于结构子类型而对 mypy 不可见的必需的“TypedDict”项。

=== "英文"

    `TypedDict` objects support a subset of dictionary operations and methods. You must use string literals as keys when calling most of the methods, as otherwise mypy won't be able to check that the key is valid. List of supported operations:

    - Anything included in [`typing.Mapping`](https://docs.python.org/3/library/typing.html#typing.Mapping):

      - `d[key]`
      - `key in d`
      - `len(d)`
      - `for key in d` (iteration)
      - [`d.get(key[, default])`](https://docs.python.org/3/library/stdtypes.html#dict.get)
      - [`d.keys()`](https://docs.python.org/3/library/stdtypes.html#dict.keys)
      - [`d.values()`](https://docs.python.org/3/library/stdtypes.html#dict.values)
      - [`d.items()`](https://docs.python.org/3/library/stdtypes.html#dict.items)

    - [`d.copy()`](https://docs.python.org/3/library/stdtypes.html#dict.copy)
    - [`d.setdefault(key, default)`](https://docs.python.org/3/library/stdtypes.html#dict.setdefault)
    - [`d1.update(d2)`](https://docs.python.org/3/library/stdtypes.html#dict.update)
    - [`d.pop(key[, default])`](https://docs.python.org/3/library/stdtypes.html#dict.pop) (partial `TypedDict`s only)
    - `del d[key]` (partial `TypedDict`s only)

    !!! info "Note"

        [`dict.clear`](https://docs.python.org/3/library/stdtypes.html#dict.clear) and [`dict.popitem`](https://docs.python.org/3/library/stdtypes.html#dict.popitem) are not supported since they are unsafe -- they could delete required `TypedDict` items that are not visible to mypy because of structural subtyping.

## 基于类的语法

Class-based syntax

=== "中文"

    Python 3.6 及更高版本支持另一种基于类的语法来定义“TypedDict”：

    ```python
    from typing_extensions import TypedDict

    class Movie(TypedDict):
        name: str
        year: int
    ```

    上面的定义相当于原来的“Movie”定义。 它实际上并没有定义真正的类。 此语法还支持一种继承形式——子类可以定义附加项。 然而，这主要是一种符号快捷方式。 由于 mypy 使用与 TypedDict 的结构兼容性，因此不需要继承来实现兼容性。 下面是一个继承的例子：

    ```python
    class Movie(TypedDict):
        name: str
        year: int

    class BookBasedMovie(Movie):
        based_on: str
    ```

    现在“BookBasedMovie”有键“name”、“year”和“based_on”。

=== "英文"

    An alternative, class-based syntax to define a `TypedDict` is supported in Python 3.6 and later:

    ```python
    from typing_extensions import TypedDict

    class Movie(TypedDict):
        name: str
        year: int
    ```

    The above definition is equivalent to the original `Movie` definition. It doesn't actually define a real class. This syntax also supports a form of inheritance -- subclasses can define additional items. However, this is primarily a notational shortcut. Since mypy uses structural compatibility with `TypedDict`s, inheritance is not required for compatibility. Here is an example of inheritance:

    ```python
    class Movie(TypedDict):
        name: str
        year: int

    class BookBasedMovie(Movie):
        based_on: str
    ```

    Now `BookBasedMovie` has keys `name`, `year` and `based_on`.

## 混合必需和非必需的项目

Mixing required and non-required items

=== "中文"

    除了允许跨“TypedDict”类型重用之外，继承还允许您在单个“TypedDict”中混合必需和非必需（使用“total=False”）项。 例子：

    ```python
    class MovieBase(TypedDict):
        name: str
        year: int

    class Movie(MovieBase, total=False):
        based_on: str
    ```

    现在“Movie”需要键“name”和“year”，而“based_on”在构造对象时可以省略。 混合了必需和非必需键的“TypedDict”（例如上面的“Movie”）仅当其他“TypedDict”中的所有必需键都是第一个“TypedDict”中的必需键时才与另一个“TypedDict”兼容 ，并且其他“TypedDict”的所有非必需键也是第一个“TypedDict”中的非必需键。

=== "英文"

    In addition to allowing reuse across `TypedDict` types, inheritance also allows you to mix required and non-required (using `total=False`) items in a single `TypedDict`. Example:

    ```python
    class MovieBase(TypedDict):
        name: str
        year: int

    class Movie(MovieBase, total=False):
        based_on: str
    ```

    Now `Movie` has required keys `name` and `year`, while `based_on` can be left out when constructing an object. A `TypedDict` with a mix of required and non-required keys, such as `Movie` above, will only be compatible with another `TypedDict` if all required keys in the other `TypedDict` are required keys in the first `TypedDict`, and all non-required keys of the other `TypedDict` are also non-required keys in the first `TypedDict`.

## TypedDict 的联合

Unions of TypedDicts

=== "中文"

    由于 TypedDict 在运行时实际上只是常规字典，因此不可能使用“isinstance”检查来区分 TypedDict Union 的不同变体，就像处理常规对象一样。

    相反，您可以使用[`标记联合模式`](https://mypy.readthedocs.io/en/stable/literal_types.html#tagged-unions)。 文档的引用部分有完整的描述和示例，但简而言之，您需要为每个 TypedDict 提供相同的键，其中每个值都有唯一的 [`Literal type`](https://mypy.readthedocs.io/en/stable/literal_types.html#literal-types)。 然后，检查该键以区分您的 TypedDict。

=== "英文"

    Since TypedDicts are really just regular dicts at runtime, it is not possible to use `isinstance` checks to distinguish between different variants of a Union of TypedDict in the same way you can with regular objects.

    Instead, you can use the [`tagged union pattern`](https://mypy.readthedocs.io/en/stable/literal_types.html#tagged-unions). The referenced section of the docs has a full description with an example, but in short, you will need to give each TypedDict the same key where each value has a unique [`Literal type`](https://mypy.readthedocs.io/en/stable/literal_types.html#literal-types). Then, check that key to distinguish between your TypedDicts.
