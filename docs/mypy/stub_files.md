# 存根文件

=== "中文"

    *存根文件* 是一个包含 Python 模块公共接口框架的文件，包括类、变量、函数——最重要的是它们的类型。
    
    Mypy 使用存储在 [typeshed](https://github.com/python/typeshed) 存储库中的存根文件来确定标准库和第三方库函数、类和其他定义的类型。 您还可以创建自己的存根，用于对代码进行类型检查。

=== "英文"

    **Stub files**
    
    A *stub file* is a file containing a skeleton of the public interface of that Python module, including classes, variables, functions -- and most importantly, their types.
    
    Mypy uses stub files stored in the [typeshed](https://github.com/python/typeshed) repository to determine the types of standard library and third-party library functions, classes, and other definitions. You can also create your own stubs that will be used to type check your code.

## 创建一个存根

Creating a stub

=== "中文"

    以下是如何创建存根文件的概述：

    - 为库（或任意模块）编写存根文件，并将其作为`.pyi`文件存储在与库模块相同的目录中。

    - 或者，将存根（`.pyi` 文件）放在为存根保留的目录中（例如，`myproject/stubs`）。 在这种情况下，您必须设置环境变量`MYPYPATH`来引用该目录。 例如：

        ```shell
        export MYPYPATH=~/work/myproject/stubs
        ```

    对模块使用正常的 Python 文件名约定，例如 模块`csv`的`csv.pyi`。 使用包含`__init__.pyi`的子目录作为包。 请注意，必须安装 [`PEP 561`](https://peps.python.org/pep-0561/) 仅存根软件包，并且不能通过 `MYPYPATH` 指向(请参阅 [`PEP 561 支持 `](https://mypy.readthedocs.io/en/latest/installed_packages.html#installed-packages))。

    如果目录包含同一模块的`.py`和`.pyi`文件，则`.pyi`文件优先。 这样，即使您不想修改源代码，也可以轻松地为模块添加注释。 例如，如果您在程序中使用第 3 方开源库（并且 typeshed 中还没有存根），这可能很有用。

    就是这样！

    现在您可以在 mypy 程序中访问该模块并输入使用该库的检查代码。 如果您为库模块编写存根，请考虑将其贡献给 typeshed 存储库，以供使用 mypy 的其他程序员使用。

    Mypy 还附带了两个工具，可以更轻松地创建和维护存根：[`自动存根生成 (stubgen)`](https://mypy.readthedocs.io/en/latest/stubgen.html#stubgen) 和 [` 自动存根测试（stubtest）`]（https://mypy.readthedocs.io/en/latest/stubtest.html#stubtest）。

    以下部分介绍了可以在程序和存根文件中使用的类型注释类型。

    !!! info "Note"

        您可能想将`MYPYPATH`指向标准库或安装第 3 方软件包的 `site-packages`目录。 这几乎总是一个坏主意——您可能会收到大量关于您没有编写的代码的错误消息，并且 mypy 还无法很好地分析，并且在最坏的情况下，mypy 可能会由于某些它没想到的第 3 方包构构建而崩溃, 

=== "英文"

    Here is an overview of how to create a stub file:

    - Write a stub file for the library (or an arbitrary module) and store it as a `.pyi` file in the same directory as the library module.

    - Alternatively, put your stubs (`.pyi` files) in a directory reserved for stubs (e.g., {file}`myproject/stubs`). In this case you have to set the environment variable `MYPYPATH` to refer to the directory.  For example:

    ```shell
    export MYPYPATH=~/work/myproject/stubs
    ```

    Use the normal Python file name conventions for modules, e.g. {file}`csv.pyi` for module `csv`. Use a subdirectory with `__init__.pyi` for packages. Note that [`PEP 561`](https://peps.python.org/pep-0561/) stub-only packages must be installed, and may not be pointed at through the `MYPYPATH` (see [`PEP 561 support`](https://mypy.readthedocs.io/en/latest/installed_packages.html#installed-packages)).

    If a directory contains both a `.py` and a `.pyi` file for the same module, the `.pyi` file takes precedence. This way you can easily add annotations for a module even if you don't want to modify the source code. This can be useful, for example, if you use 3rd party open source libraries in your program (and there are no stubs in typeshed yet).

    That's it!

    Now you can access the module in mypy programs and type check code that uses the library. If you write a stub for a library module, consider making it available for other programmers that use mypy by contributing it back to the typeshed repo.

    Mypy also ships with two tools for making it easier to create and maintain stubs: [`Automatic stub generation (stubgen)`](https://mypy.readthedocs.io/en/latest/stubgen.html#stubgen) and [`Automatic stub testing (stubtest)`](https://mypy.readthedocs.io/en/latest/stubtest.html#stubtest).

    The following sections explain the kinds of type annotations you can use in your programs and stub files.

    !!! info "Note"

        You may be tempted to point `MYPYPATH` to the standard library or to the {file}`site-packages` directory where your 3rd party packages are installed. This is almost always a bad idea -- you will likely get tons of error messages about code you didn't write and that mypy can't analyze all that well yet, and in the worst case scenario mypy may crash due to some construct in a 3rd party package that it didn't expect.

## 存根文件语法

=== "中文"

    存根文件是用普通的 Python 语法编写的，但通常会省略运行时逻辑，例如变量初始值设定项、函数体和默认参数。

    如果不可能完全省略某些运行时逻辑，建议的约定是用省略号表达式（`...`）替换或删除它们。 下面的每个省略号实际上都以三个点的形式写在存根文件中：

    ```python
    # 带注释的变量不需要赋值。
    # 因此按照惯例，我们在存根文件中省略它们。
    x: int

    # 函数体无法完全去除。 按照惯例，
    # 我们将它们替换为“...”而不是“pass”语句。
    def func_1(code: str) -> int: ...

    # 我们可以对默认参数执行相同的操作。
    def func_2(a: int, b: int = ...) -> int: ...
    ```

    !!! info "Note"

        省略号`...`在[`可调用类型`](https://mypy.readthedocs.io/en/latest/kinds_of_types.html#callable-types)和[`元组类型`](https://mypy.readthedocs.io/en/latest/kinds_of_types.html#tuple-types)中也有不同的含义。

=== "英文"

    Stub files are written in normal Python syntax, but generally leaving out runtime logic like variable initializers, function bodies, and default arguments.

    If it is not possible to completely leave out some piece of runtime logic, the recommended convention is to replace or elide them with ellipsis expressions (`...`). Each ellipsis below is literally written in the stub file as three dots:

    ```python
    # Variables with annotations do not need to be assigned a value.
    # So by convention, we omit them in the stub file.
    x: int

    # Function bodies cannot be completely removed. By convention,
    # we replace them with `...` instead of the `pass` statement.
    def func_1(code: str) -> int: ...

    # We can do the same with default arguments.
    def func_2(a: int, b: int = ...) -> int: ...
    ```

    !!! info "Note"

        The ellipsis `...` is also used with a different meaning in [`callable types`](https://mypy.readthedocs.io/en/latest/kinds_of_types.html#callable-types) and [`tuple types`](https://mypy.readthedocs.io/en/latest/kinds_of_types.html#tuple-types).

## 在运行时使用存根文件语法

Using stub file syntax at runtime

=== "中文"

    您有时可能还需要省略常规 Python 代码中的实际逻辑——例如，在使用 [`函数重载`](./more_types.md#函数重载) 或 [`自定义协议`](./protocol_and_struct_subtyping.md#简单的自定义协议)编写方法时.

    推荐的风格是使用省略号来执行此操作，就像在存根文件中一样。 在代码的用户可能意外调用没有实际功能的函数的情况下，抛出 `NotImplementedError` 也被认为是可以接受的。 逻辑。

    只要函数体不包含运行时逻辑，您也可以省略默认参数：函数体仅包含单个省略号、pass 语句或`raise NotImplementedError()`。 函数体包含文档字符串也是可以接受的。 例如：

    ```python
    from typing_extensions import Protocol

    class Resource(Protocol):
        def ok_1(self, foo: list[str] = ...) -> None: ...

        def ok_2(self, foo: list[str] = ...) -> None:
            raise NotImplementedError()

        def ok_3(self, foo: list[str] = ...) -> None:
            """Some docstring"""
            pass

        # Error: Incompatible default for argument "foo" (default has type "ellipsis", argument has type "list[str]")
        def not_ok(self, foo: list[str] = ...) -> None:
            print(foo)
    ```

=== "英文"

    You may also occasionally need to elide actual logic in regular Python code -- for example, when writing methods in [`overload variants`](./more_types.md#函数重载) or [`custom protocols`](./protocol_and_struct_subtyping.md#简单的自定义协议).

    The recommended style is to use ellipses to do so, just like in stub files. It is also considered stylistically acceptable to throw a [`NotImplementedError`](https://docs.python.org/3/library/exceptions.html#NotImplementedError) in cases where the user of the code may accidentally call functions with no actual logic.

    You can also elide default arguments as long as the function body also contains no runtime logic: the function body only contains a single ellipsis, the pass statement, or a `raise NotImplementedError()`. It is also acceptable for the function body to contain a docstring. For example:

    ```python
    from typing_extensions import Protocol

    class Resource(Protocol):
        def ok_1(self, foo: list[str] = ...) -> None: ...

        def ok_2(self, foo: list[str] = ...) -> None:
            raise NotImplementedError()

        def ok_3(self, foo: list[str] = ...) -> None:
            """Some docstring"""
            pass

        # Error: Incompatible default for argument "foo" (default has
        # type "ellipsis", argument has type "list[str]")
        def not_ok(self, foo: list[str] = ...) -> None:
            print(foo)
    ```
