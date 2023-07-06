# 内置类型

=== "中文"

    本章介绍一些常用的内置类型。 稍后我们将介绍许多其他类型。

=== "英文"

    **Built-in types**
    
    This chapter introduces some commonly used built-in types. We will cover many other kinds of types later.

## 简单类型

=== "中文"

    以下是一些常见内置类型的示例：
    
    | **类型** | **描述** |
    | --------| -------- |
    | `int` | 整数 |
    | `float` | 浮点数 |
    | `bool` | 布尔值（`int`的子类） |
    | `str` | 文本，unicode 代码点序列  |
    | `bytes` | 8 位字符串，字节值序列  |
    | `object` | 任意对象（object是公共基类） |

    所有内置类都可以用作类型。

=== "英文"

    **Simple types**
    
    Here are examples of some common built-in types:
    
    | **Type** | **Description** |
    | --------| -------- |
    | `int` | integer |
    | `float` | floating point number |
    | `bool` | boolean value (subclass of `int`)  |
    | `str` | text, sequence of unicode codepoints  |
    | `bytes` | 8-bit string, sequence of byte values  |
    | `object` | an arbitrary object (object is the common base class) |

    All built-in classes can be used as types.

| Type | Description |
| --------| -------- |
| aaa | bbb  |
| aaa | bbb  |
| aaa | bbb  |
| aaa | bbb  |
| aaa | bbb  |

## Any type

=== "中文"

    如果你找不到某个值的合适类型，你可以随时回退到 Any：


    | Type | Description |
    | --------| -------- |
    | `Any` | 任意类型的动态类型值  |

    Any 类型在 [typing](https://docs.python.org/3/library/typing.html#module-typing) 模块中定义。 有关更多详细信息，请参阅[动态类型代码](https://mypy.readthedocs.io/en/stable/dynamic_typing.html#dynamic-typing)。

=== "英文"

    **Any type**

    If you can’t find a good type for some value, you can always fall back to Any:


    | Type | Description |
    | --------| -------- |
    | `Any` | dynamically typed value with an arbitrary type  |

    The type Any is defined in the [typing](https://docs.python.org/3/library/typing.html#module-typing) module. See [Dynamically typed code](https://mypy.readthedocs.io/en/stable/dynamic_typing.html#dynamic-typing) for more details.

## 范型类型

=== "中文"

    在Python 3.9及更高版本中，内置集合类型对象支持索引：

    | 类型 | 描述 |
    | --------| -------- |
    | `list[str]` | `str` 对象列表  |
    | `tuple[int, int]` | 两个 `int` 对象的元组（`tuple[()]` 是空元组）  |
    | `tuple[int, ...]` | 任意数量的 `int` 对象的元组  |
    | `dict[str, int]` | 字典从 str 键到 `int` 值  |
    | `Iterable[int]` | 包含整数的可迭代对象  |
    | `Sequence[bool]` | 布尔值序列（只读）  |
    | `Mapping[str, int]` | 从 str 键到 int 值的映射（只读）  |
    | `type[C]` | `C` 的类型对象（`C` 是类/类型变量/类型联合）  |
       
    类型 `dict` 是一个泛型类，由 `[...]` 中的类型参数表示。 例如，`dict[int, str]` 是从整数到字符串的字典，`dict[Any, Any]` 是动态类型（任意）值和键的字典。 `list` 是另一个泛型类。

    `Iterable`、`Sequence` 和 `Mapping` 是与 Python 协议相对应的泛型类型。 例如，当需要`Iterable[str]`或`Sequence[str]`时，`str`对象或`list[str]`对象是有效的。 您可以从 [`collections.abc`](https://docs.python.org/3/library/collections.abc.html#module-collections.abc) 导入，而不是从 Python 3.9 中的 [`typing`](https://docs.python.org/3/library/typing.html#module-typing)导入它们。
    
    有关更多详细信息，请参阅 [`generic-builtins`](https://mypy.readthedocs.io/en/stable/runtime_troubles.html#generic-builtins)，包括如何在 Python 3.7 和 3.8 中的注释中使用它们。
    
    如果您需要支持 Python 3.8 及更早版本，则需要 [`typing`](https://docs.python.org/3/library/typing.html#module-typing) 中定义的这些旧类型：

    | 类型 | 描述 |
    | --------| -------- |
    |`List[str]`          | `str` 对象列表  |
    |`Tuple[int, int]`    | 两个 `int` 对象的元组（`tuple[()]` 是空元组）  |
    |`Tuple[int, ...]`    | 任意数量的 `int` 对象的元组  |
    |`Dict[str, int]`     | 字典从 str 键到 `int` 值  |
    |`Iterable[int]`      | 包含整数的可迭代对象  |
    |`Sequence[bool]`     | 布尔值序列（只读）  |
    |`Mapping[str, int]`  | 从 str 键到 int 值的映射（只读）  |
    |`Type[C]`            | `C` 的类型对象（`C` 是类/类型变量/类型联合）  |
    
    `List` 是支持索引的内置类型 `list` 的别名 (`dict`/`Dict` 和 `tuple`/`Tuple` 也都是类似的).
    
    请注意，尽管 `Iterable`、`Sequence` 和 `Mapping` 看起来与 [`collections.abc`](https://docs.python.org/3/library/collections.abc.html#module-collections.abc)（以前的 `collections`）中定义的抽象基类类似，但它们并不相同，因为后者不支持索引 Python 3.9 之前的版本。

=== "英文"

    **Generic types**

    In Python 3.9 and later, built-in collection type objects support indexing:

    | Type | Description |
    | --------| -------- |
    | `list[str]` | list of `str` objects  |
    | `tuple[int, int]` | tuple of two int objects (`tuple[()]` is the empty tuple)  |
    | `tuple[int, ...]` | tuple of an arbitrary number of `int` objects  |
    | `dict[str, int]` | dictionary from str keys to `int` values  |
    | `Iterable[int]` | iterable object containing ints  |
    | `Sequence[bool]` | sequence of booleans (read-only)  |
    | `Mapping[str, int]` | mapping from str keys to int values (read-only)  |
    | `type[C]` | type object of `C` (`C` is a class/type variable/union of types)  |
       
    The type `dict` is a generic class, signified by type arguments within `[...]`. For example, `dict[int, str]` is a dictionary from integers to strings and `dict[Any, Any]` is a dictionary of dynamically typed (arbitrary) values and keys. `list` is another generic class.

    `Iterable`, `Sequence`, and `Mapping` are generic types that correspond to Python protocols. For example, a `str` object or a `list[str]` object is valid when `Iterable[str]` or `Sequence[str]` is expected. You can import them from [`collections.abc`](https://docs.python.org/3/library/collections.abc.html#module-collections.abc) instead of importing from [`typing`](https://docs.python.org/3/library/typing.html#module-typing) in Python 3.9.
    
    See [`generic-builtins`](https://mypy.readthedocs.io/en/stable/runtime_troubles.html#generic-builtins) for more details, including how you can use these in annotations also in Python 3.7 and 3.8.
    
    These legacy types defined in [`typing`](https://docs.python.org/3/library/typing.html#module-typing) are needed if you need to support Python 3.8 and earlier:

    | Type | Description |
    | --------| -------- |
    |`List[str]`          | list of `str` objects |
    |`Tuple[int, int]`    | tuple of two `int` objects (`Tuple[()]` is the empty tuple) |
    |`Tuple[int, ...]`    | tuple of an arbitrary number of `int` objects |
    |`Dict[str, int]`     | dictionary from `str` keys to `int` values |
    |`Iterable[int]`      | iterable object containing ints |
    |`Sequence[bool]`     | sequence of booleans (read-only) |
    |`Mapping[str, int]`  | mapping from `str` keys to `int` values (read-only) |
    |`Type[C]`            | type object of `C` (`C` is a class/type variable/union of types) |
    
    `List` is an alias for the built-in type `list` that supports indexing (and similarly for `dict`/`Dict` and `tuple`/`Tuple`).
    
    Note that even though `Iterable`, `Sequence` and `Mapping` look similar to abstract base classes defined in [`collections.abc`](https://docs.python.org/3/library/collections.abc.html#module-collections.abc) (formerly `collections`), they are not identical, since the latter don't support indexing prior to Python 3.9.
