# 运行时的注解问题

=== "中文"

    类型注释的惯用用法有时可能会与给定版本的 Python 认为的合法代码相冲突。 本节描述这些场景并解释如何让代码再次运行。 一般来说，我们可以使用三种工具：
    
    - 使用 `from __future__ import annotations` ([`PEP 563`](https://peps.python.org/pep-0563/)) （此行为最终可能会在未来的 Python 版本中成为默认行为）
    - 使用字符串文字类型或类型注释
    - 使用 `typing.TYPE_CHECKING`
    
    在讨论您可能遇到的具体问题之前，我们会先提供这些内容的描述。

=== "英文"

    **Annotation issues at runtime**

    Idiomatic use of type annotations can sometimes run up against what a given version of Python considers legal code. This section describes these scenarios and explains how to get your code running again. Generally speaking, we have three tools at our disposal:
    
    - Use of `from __future__ import annotations` ([`PEP 563`](https://peps.python.org/pep-0563/))  (this behaviour may eventually be made the default in a future Python version)
    - Use of string literal types or type comments
    - Use of `typing.TYPE_CHECKING`
    
    We provide a description of these before moving onto discussion of specific problems you may encounter.

## 字符串文字类型和类型注释

=== "中文"

    Mypy 允许您使用以 `# type:` 形式添加类型注释。 例如：

    ```python
    a = 1  # type: int

    def f(x):  # type: (int) -> int
        return x + 1

    # 具有多个参数的函数的替代类型注释语法
    def send_email(
        address,     # type: Union[str, List[str]]
        sender,      # type: str
        cc,          # type: Optional[List[str]]
        subject='',
        body=None    # type: List[str]
    ):
        # type: (...) -> bool
    ```

    类型注释不会导致运行时错误，因为 Python 不会评估注释。

    以类似的方式，使用字符串文字类型可以回避可能导致运行时错误的注释问题。

    任何类型都可以作为字符串文字输入，并且您可以自由地将字符串文字类型与非字符串文字类型组合：

    ```python
    def f(a: list['A']) -> None: ...  # OK, 防止 NameError 因为 A 是稍后定义的
    def g(n: 'int') -> None: ...      # 同样 OK, 虽然没用

    class A: pass
    ```

    `# type:` 注释和 [`存根文件`](./stub_files.md) 中永远不需要字符串文字类型。

    字符串文字类型必须稍后在*同一模块*中定义（或导入）。 它们不能用于留下未解决的跨模块引用。(有关处理导入周期，请参阅 [`import-cycles`](#循环导入).)

=== "英文"

    **String literal types and type comments**

    Mypy allows you to add type annotations using `# type:` type comments. For example:

    ```python
    a = 1  # type: int

    def f(x):  # type: (int) -> int
        return x + 1

    # Alternative type comment syntax for functions with many arguments
    def send_email(
        address,     # type: Union[str, List[str]]
        sender,      # type: str
        cc,          # type: Optional[List[str]]
        subject='',
        body=None    # type: List[str]
    ):
        # type: (...) -> bool
    ```

    Type comments can't cause runtime errors because comments are not evaluated by Python.

    In a similar way, using string literal types sidesteps the problem of annotations tha would cause runtime errors.

    Any type can be entered as a string literal, and you can combine string-literal types with non-string-literal types freely:

    ```python
    def f(a: list['A']) -> None: ...  # OK, prevents NameError since A is defined later
    def g(n: 'int') -> None: ...      # Also OK, though not useful

    class A: pass
    ```

    String literal types are never needed in `# type:` comments and [`stub files`](./stub_files.md).

    String literal types must be defined (or imported) later *in the same module*. They cannot be used to leave cross-module references unresolved.  (For dealing with import cycles, see [`import-cycles`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#import-cycles).)

    (future-annotations)=

## Future 模块注解导入 (PEP 563)

=== "中文"

    这里描述的许多问题都是由 Python 尝试评估注释引起的。 未来的 Python 版本（可能是 Python 3.12）将默认不再尝试计算函数和变量注释。 Python 3.7 及更高版本中通过使用 `from __future__ import annotation`提供了此行为。

    这可以被认为是所有函数和变量注释的自动字符串文字化。 请注意，函数和变量注释仍然需要是有效的 Python 语法。 有关更多详细信息，请参阅[`PEP 563`](https://peps.python.org/pep-0563/).

    !!! note "Note"

        即使使用 `__future__` 导入，在某些情况下仍然可能需要字符串文字或导致错误，通常涉及在以下情况中使用前置引用或泛型：

        - [`type aliases`](./kind_of_types.md#类型别名);
        - [`type narrowing`](./type_narrowing.md);
        - 类型定义 (参考 [`typing.TypeVar`](https://docs.python.org/3/library/typing.html#typing.TypeVar), [`typing.NewType`](https://docs.python.org/3/library/typing.html#typing.NewType), [`typing.NamedTuple`](https://docs.python.org/3/library/typing.html#typing.NamedTuple));
        - 基类.

        ```python
        # 基类示例
        from __future__ import annotations
        class A(tuple['B', 'C']): ... # 这里需要字符串文字类型
        class B: ...
        class C: ...
        ```

    !!! warning "Warning"
        
        某些库可能具有动态评估注释的用例，例如通过使用`typing.get_type_hints`或`eval`。 如果您的注释在评估时会引发错误（例如使用 Python 3.9 的 [`PEP 604`](https://peps.python.org/pep-0604/) 语法），则在使用此类时可能需要小心这样的库。

=== "英文"

    **Future annotations import (PEP 563)**

    Many of the issues described here are caused by Python trying to evaluate annotations. Future Python versions (potentially Python 3.12) will by default no longer attempt to evaluate function and variable annotations. This behaviour is made available in Python 3.7 and later through the use of `from __future__ import annotations`.

    This can be thought of as automatic string literal-ification of all function and variable annotations. Note that function and variable annotations are still required to be valid Python syntax. For more details, see [`PEP 563`](https://peps.python.org/pep-0563/).

    !!! note "Note"

        Even with the `__future__` import, there are some scenarios that could still require string literals or result in errors, typically involving use of forward references or generics in:

        - [`type aliases`](./kind_of_types.md#类型别名);
        - [`type narrowing`](./type_narrowing.md);
        - type definitions (see [`typing.TypeVar`](https://docs.python.org/3/library/typing.html#typing.TypeVar), [`typing.NewType`](https://docs.python.org/3/library/typing.html#typing.NewType), [`typing.NamedTuple`](https://docs.python.org/3/library/typing.html#typing.NamedTuple));
        - base classes.

        ```python
        # base class example
        from __future__ import annotations
        class A(tuple['B', 'C']): ... # String literal types needed here
        class B: ...
        class C: ...
        ```

    !!! warning "Warning"
        
        Some libraries may have use cases for dynamic evaluation of annotations, for instance, through use of `typing.get_type_hints` or `eval`. If your annotation would raise an error when evaluated (say by using [`PEP 604`](https://peps.python.org/pep-0604/) syntax with Python 3.9), you may need to be careful when using such libraries.

## typing.TYPE_CHECKING 变量

=== "中文"

    [`typing`](https://docs.python.org/3/library/typing.html#module-typing) 模块定义了一个 [`typing.TYPE_CHECKING`](https://docs.python.org/3/library/typing.html#typing.TYPE_CHECKING) 常量在运行时为 `False`，但在类型检查时被视为 `True`。

    在 `if TYPE_CHECKING` 代码的内部: 代码不会在运行时执行，因此它提供了一种方便的方法来告诉 mypy 某些内容，而无需在运行时评估代码。 这对于解决[循环导入](#循环导入)最有用。

=== "英文"

    **typing.TYPE_CHECKING**

    The [`typing`](https://docs.python.org/3/library/typing.html#module-typing) module defines a [`typing.TYPE_CHECKING`](https://docs.python.org/3/library/typing.html#typing.TYPE_CHECKING) constant that is `False` at runtime but treated as `True` while type checking.

    Since code inside `if TYPE_CHECKING:` is not executed at runtime, it provides a convenient way to tell mypy something without the code being evaluated at runtime. This is most useful for resolving [`import cycles`](#循环导入).

## 类名前置引用

=== "中文"

    Python does not allow references to a class object before the class is
    defined (aka forward reference). Thus this code does not work as expected:

    ```python
    def f(x: A) -> None: ...  # NameError: name "A" is not defined
    class A: ...
    ```

    Starting from Python 3.7, you can add `from __future__ import annotations` to
    resolve this, as discussed earlier:

    ```python
    from __future__ import annotations

    def f(x: A) -> None: ...  # OK
    class A: ...
    ```

    For Python 3.6 and below, you can enter the type as a string literal or type comment:

    ```python
    def f(x: 'A') -> None: ...  # OK

    # Also OK
    def g(x):  # type: (A) -> None
        ...

    class A: ...
    ```

    Of course, instead of using future annotations import or string literal types,
    you could move the function definition after the class definition. This is not
    always desirable or even possible, though.

=== "英文"

    **Class name forward references**

    Python does not allow references to a class object before the class is
    defined (aka forward reference). Thus this code does not work as expected:

    ```python
    def f(x: A) -> None: ...  # NameError: name "A" is not defined
    class A: ...
    ```

    Starting from Python 3.7, you can add `from __future__ import annotations` to
    resolve this, as discussed earlier:

    ```python
    from __future__ import annotations

    def f(x: A) -> None: ...  # OK
    class A: ...
    ```

    For Python 3.6 and below, you can enter the type as a string literal or type comment:

    ```python
    def f(x: 'A') -> None: ...  # OK

    # Also OK
    def g(x):  # type: (A) -> None
        ...

    class A: ...
    ```

    Of course, instead of using future annotations import or string literal types,
    you could move the function definition after the class definition. This is not
    always desirable or even possible, though.

## 循环导入

=== "中文"

    An import cycle occurs where module A imports module B and module B imports module A (perhaps indirectly, e.g. `A -> B -> C -> A`). Sometimes in order to add type annotations you have to add extra imports to a module and those imports cause cycles that didn't exist before. This can lead to errors at runtime like:

    ```text
    ImportError: cannot import name 'b' from partially initialized module 'A' (most likely due to a circular import)
    ```

    If those cycles do become a problem when running your program, there's a trick: if the import is only needed for type annotations and you're using a) the [`future annotations import`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#future-annotations), or b) string literals or type comments for the relevant annotations, you can write the imports inside `if TYPE_CHECKING:` so that they are not executed at runtime. Example:

    File `foo.py`:

    ```python
    from typing import TYPE_CHECKING

    if TYPE_CHECKING:
        import bar

    def listify(arg: 'bar.BarClass') -> 'list[bar.BarClass]':
        return [arg]
    ```

    File `bar.py`:

    ```python
    from foo import listify

    class BarClass:
        def listifyme(self) -> 'list[BarClass]':
            return listify(self)
    ```

=== "英文"

    **Import cycles**

    An import cycle occurs where module A imports module B and module B imports module A (perhaps indirectly, e.g. `A -> B -> C -> A`). Sometimes in order to add type annotations you have to add extra imports to a module and those imports cause cycles that didn't exist before. This can lead to errors at runtime like:

    ```text
    ImportError: cannot import name 'b' from partially initialized module 'A' (most likely due to a circular import)
    ```

    If those cycles do become a problem when running your program, there's a trick: if the import is only needed for type annotations and you're using a) the [`future annotations import`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#future-annotations), or b) string literals or type comments for the relevant annotations, you can write the imports inside `if TYPE_CHECKING:` so that they are not executed at runtime. Example:

    File `foo.py`:

    ```python
    from typing import TYPE_CHECKING

    if TYPE_CHECKING:
        import bar

    def listify(arg: 'bar.BarClass') -> 'list[bar.BarClass]':
        return [arg]
    ```

    File `bar.py`:

    ```python
    from foo import listify

    class BarClass:
        def listifyme(self) -> 'list[BarClass]':
            return listify(self)
    ```

## 使用存根中的泛型类，但运行时不用

=== "中文"

    Some classes are declared as [`generic`](https://mypy.readthedocs.io/en/latest/generics.html#generic-classes) in stubs, but not at runtime.

    In Python 3.8 and earlier, there are several examples within the standard library, for instance, [`os.PathLike`](https://docs.python.org/3/library/os.html#os.PathLike) and [`queue.Queue`](https://docs.python.org/3/library/queue.html#queue.Queue). Subscripting
    such a class will result in a runtime error:

    ```python
    from queue import Queue

    class Tasks(Queue[str]):  # TypeError: 'type' object is not subscriptable
        ...

    results: Queue[int] = Queue()  # TypeError: 'type' object is not subscriptable
    ```

    To avoid errors from use of these generics in annotations, just use the [`future annotations import`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#future-annotations) (or string literals or type
    comments for Python 3.6 and below).

    To avoid errors when inheriting from these classes, things are a little more
    complicated and you need to use [`typing.TYPE_CHECKING`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#typing-type-checking):

    ```python
    from typing import TYPE_CHECKING
    from queue import Queue

    if TYPE_CHECKING:
        BaseQueue = Queue[str]  # this is only processed by mypy
    else:
        BaseQueue = Queue  # this is not seen by mypy but will be executed at runtime

    class Tasks(BaseQueue):  # OK
        ...

    task_queue: Tasks
    reveal_type(task_queue.get())  # Reveals str
    ```

    If your subclass is also generic, you can use the following:

    ```python
    from typing import TYPE_CHECKING, TypeVar, Generic
    from queue import Queue

    _T = TypeVar("_T")
    if TYPE_CHECKING:
        class _MyQueueBase(Queue[_T]): pass
    else:
        class _MyQueueBase(Generic[_T], Queue): pass

    class MyQueue(_MyQueueBase[_T]): pass

    task_queue: MyQueue[str]
    reveal_type(task_queue.get())  # Reveals str
    ```

    In Python 3.9, we can just inherit directly from `Queue[str]` or `Queue[T]` since its [`queue.Queue`](https://docs.python.org/3/library/queue.html#queue.Queue implements {py:meth}`__class_getitem__`, so the class object can be subscripted at runtime without issue.

=== "英文"

    **Using classes that are generic in stubs but not at runtime**

    Some classes are declared as [`generic`](https://mypy.readthedocs.io/en/latest/generics.html#generic-classes) in stubs, but not at runtime.

    In Python 3.8 and earlier, there are several examples within the standard library, for instance, [`os.PathLike`](https://docs.python.org/3/library/os.html#os.PathLike) and [`queue.Queue`](https://docs.python.org/3/library/queue.html#queue.Queue). Subscripting
    such a class will result in a runtime error:

    ```python
    from queue import Queue

    class Tasks(Queue[str]):  # TypeError: 'type' object is not subscriptable
        ...

    results: Queue[int] = Queue()  # TypeError: 'type' object is not subscriptable
    ```

    To avoid errors from use of these generics in annotations, just use the [`future annotations import`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#future-annotations) (or string literals or type
    comments for Python 3.6 and below).

    To avoid errors when inheriting from these classes, things are a little more
    complicated and you need to use [`typing.TYPE_CHECKING`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#typing-type-checking):

    ```python
    from typing import TYPE_CHECKING
    from queue import Queue

    if TYPE_CHECKING:
        BaseQueue = Queue[str]  # this is only processed by mypy
    else:
        BaseQueue = Queue  # this is not seen by mypy but will be executed at runtime

    class Tasks(BaseQueue):  # OK
        ...

    task_queue: Tasks
    reveal_type(task_queue.get())  # Reveals str
    ```

    If your subclass is also generic, you can use the following:

    ```python
    from typing import TYPE_CHECKING, TypeVar, Generic
    from queue import Queue

    _T = TypeVar("_T")
    if TYPE_CHECKING:
        class _MyQueueBase(Queue[_T]): pass
    else:
        class _MyQueueBase(Generic[_T], Queue): pass

    class MyQueue(_MyQueueBase[_T]): pass

    task_queue: MyQueue[str]
    reveal_type(task_queue.get())  # Reveals str
    ```

    In Python 3.9, we can just inherit directly from `Queue[str]` or `Queue[T]` since its [`queue.Queue`](https://docs.python.org/3/library/queue.html#queue.Queue implements {py:meth}`__class_getitem__`, so the class object can be subscripted at runtime without issue.

## 使用存根中定义的类型，但不在运行时使用

=== "中文"

    Sometimes stubs that you're using may define types you wish to re-use that do not exist at runtime. Importing these types naively will cause your code to fail at runtime with `ImportError` or `ModuleNotFoundError`. Similar to previous sections, these can be dealt with by using [`typing.TYPE_CHECKING`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#typing-type-checking):

    ```python
    from __future__ import annotations
    from typing import TYPE_CHECKING
    if TYPE_CHECKING:
        from _typeshed import SupportsRichComparison

    def f(x: SupportsRichComparison) -> None
    ```

    The `from __future__ import annotations` is required to avoid a `NameError` when using the imported symbol. For more information and caveats, see the section on [`future annotations`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#future-annotations).

=== "英文"

    **Using types defined in stubs but not at runtime**

    Sometimes stubs that you're using may define types you wish to re-use that do not exist at runtime. Importing these types naively will cause your code to fail at runtime with `ImportError` or `ModuleNotFoundError`. Similar to previous sections, these can be dealt with by using [`typing.TYPE_CHECKING`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#typing-type-checking):

    ```python
    from __future__ import annotations
    from typing import TYPE_CHECKING
    if TYPE_CHECKING:
        from _typeshed import SupportsRichComparison

    def f(x: SupportsRichComparison) -> None
    ```

    The `from __future__ import annotations` is required to avoid a `NameError` when using the imported symbol. For more information and caveats, see the section on [`future annotations`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#future-annotations).

## 使用内置的泛型

=== "中文"

    Starting with Python 3.9 ([`585`](https://peps.python.org/pep-0585/)), the type objects of many collections in the standard library support subscription at runtime. This means that you no longer have to import the equivalents from [`typing`](https://docs.python.org/3/library/typing.html#module-typing); you can simply use the built-in collections or those from [`collections.abc`](https://docs.python.org/3/library/collections.abc.html#module-collections.abc):

    ```python
    from collections.abc import Sequence
    x: list[str]
    y: dict[int, str]
    z: Sequence[str] = x
    ```

    There is limited support for using this syntax in Python 3.7 and later as well: if you use `from __future__ import annotations`, mypy will understand this syntax in annotations. However, since this will not be supported by the Python interpreter at runtime, make sure you're aware of the caveats mentioned in the notes at {ref} [`future annotations import`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#future-annotations).

=== "英文"

    **Using generic builtins**

    Starting with Python 3.9 ([`585`](https://peps.python.org/pep-0585/)), the type objects of many collections in the standard library support subscription at runtime. This means that you no longer have to import the equivalents from [`typing`](https://docs.python.org/3/library/typing.html#module-typing); you can simply use the built-in collections or those from [`collections.abc`](https://docs.python.org/3/library/collections.abc.html#module-collections.abc):

    ```python
    from collections.abc import Sequence
    x: list[str]
    y: dict[int, str]
    z: Sequence[str] = x
    ```

    There is limited support for using this syntax in Python 3.7 and later as well: if you use `from __future__ import annotations`, mypy will understand this syntax in annotations. However, since this will not be supported by the Python interpreter at runtime, make sure you're aware of the caveats mentioned in the notes at {ref} [`future annotations import`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#future-annotations).

## 使用Union的 X | Y 语法

=== "中文"

    Starting with Python 3.10 [`PEP 604`](https://peps.python.org/pep-0604/), you can spell union types as `x: int | str`, instead of `x: typing.Union[int, str]`.

    There is limited support for using this syntax in Python 3.7 and later as well: if you use `from __future__ import annotations`, mypy will understand this syntax in annotations, string literal types, type comments and stub files. However, since this will not be supported by the Python interpreter at runtime (if evaluated, `int | str` will raise `TypeError: unsupported operand type(s) for |: 'type' and 'type'`), make sure you're aware of the caveats mentioned in the notes at [`future annotations import<future-annotations>`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#future-annotations).

=== "英文"

    **Using X | Y syntax for Unions**

    Starting with Python 3.10 [`PEP 604`](https://peps.python.org/pep-0604/), you can spell union types as `x: int | str`, instead of `x: typing.Union[int, str]`.

    There is limited support for using this syntax in Python 3.7 and later as well: if you use `from __future__ import annotations`, mypy will understand this syntax in annotations, string literal types, type comments and stub files. However, since this will not be supported by the Python interpreter at runtime (if evaluated, `int | str` will raise `TypeError: unsupported operand type(s) for |: 'type' and 'type'`), make sure you're aware of the caveats mentioned in the notes at [`future annotations import<future-annotations>`](https://mypy.readthedocs.io/en/latest/runtime_troubles.html#future-annotations).

## 使用typing模块的新功能

=== "中文"

    You may find yourself wanting to use features added to the {py:mod}`typing` module in earlier versions of Python than the addition, for example, using any of `Literal`, `Protocol`, `TypedDict` with Python 3.6.

    The easiest way to do this is to install and use the `typing_extensions` package from PyPI for the relevant imports, for example:

    ```python
    from typing_extensions import Literal
    x: Literal["open", "close"]
    ```

    If you don't want to rely on `typing_extensions` being installed on newer Pythons, you could alternatively use:

    ```python
    import sys
    if sys.version_info >= (3, 8):
        from typing import Literal
    else:
        from typing_extensions import Literal

    x: Literal["open", "close"]
    ```

    This plays nicely well with following [`PEP 508`](https://peps.python.org/pep-0508/) dependency specification: `typing_extensions; python_version<"3.8"`

=== "英文"

    **Using new additions to the typing module**

    You may find yourself wanting to use features added to the {py:mod}`typing` module in earlier versions of Python than the addition, for example, using any of `Literal`, `Protocol`, `TypedDict` with Python 3.6.

    The easiest way to do this is to install and use the `typing_extensions` package from PyPI for the relevant imports, for example:

    ```python
    from typing_extensions import Literal
    x: Literal["open", "close"]
    ```

    If you don't want to rely on `typing_extensions` being installed on newer Pythons, you could alternatively use:

    ```python
    import sys
    if sys.version_info >= (3, 8):
        from typing import Literal
    else:
        from typing_extensions import Literal

    x: Literal["open", "close"]
    ```

    This plays nicely well with following [`PEP 508`](https://peps.python.org/pep-0508/) dependency specification: `typing_extensions; python_version<"3.8"`
