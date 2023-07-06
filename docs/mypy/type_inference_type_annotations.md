# 类型推断和类型注解

**Type inference and type annotations**

## 类型推断

=== "中文"

    对于大多数变量，如果您没有显式指定其类型，mypy 将根据最初分配给变量的类型推断出正确的类型。

    ```python

    # 尽管没有注释，Mypy 仍会推断这些变量的类型
    i = 1
    reveal_type(i)  # 显示的类型是 “builtins.int”
    l = [1, 2]
    reveal_type(l)  # 显示类型为 “builtins.list[builtins.int]”
    ```

    !!! info "笔记"
    
        请注意，mypy 不会在动态类型函数（没有函数类型注释的函数）中使用类型推断 - 在此类函数中，每个局部变量类型默认为“Any”。 有关更多详细信息，请参阅[`dynamic-typing`](https://mypy.readthedocs.io/en/stable/dynamic_typing.html#dynamic-typing)。

        ```python
        def untyped_function():
            i = 1
            reveal_type(i) # 揭示的类型是 "Any"
                           # “reveal_type”在未经检查的函数中始终输出“Any”
        ```

=== "英文"

    **Type inference**

    For most variables, if you do not explicitly specify its type, mypy will infer the correct type based on what is initially assigned to the variable.

    ```python

    # Mypy will infer the type of these variables, despite no annotations
    i = 1
    reveal_type(i)  # Revealed type is "builtins.int"
    l = [1, 2]
    reveal_type(l)  # Revealed type is "builtins.list[builtins.int]"
    ```

    !!! info "Note"
    
        Note that mypy will not use type inference in dynamically typed functions (those without a function type annotation) — every local variable type defaults to `Any` in such functions. For more details, see [`dynamic-typing`](https://mypy.readthedocs.io/en/stable/dynamic_typing.html#dynamic-typing).

        ```python
        def untyped_function():
            i = 1
            reveal_type(i) # Revealed type is "Any"
                           # 'reveal_type' always outputs 'Any' in unchecked functions
        ```

## 变量的显式类型

=== "中文"

    您可以使用变量类型注释覆盖变量的推断类型：

    ```python
    from typing import Union

    x: Union[int, str] = 1
    ```

    如果没有类型注释，“x” 的类型将只是 “int” 。 我们使用注释给它一个更通用的类型 “Union[int, str]”（该类型意味着该值可以是 “int” 或 “str” ）。
    
    考虑这个问题的最佳方法是类型注释设置变量的类型，而不是表达式的类型。 例如，mypy 会抱怨以下代码：

    ```python
    x: Union[int, str] = 1.1  # error: 赋值中的类型不兼容（表达式的类型为“float”，变量的类型为“Union[int, str]”）
    ```

    !!! info "Note"

        要显式覆盖表达式的类型，您可以使用 [`cast(\<type\>, \<expression\>) <typing.cast>`](https://docs.python.org/3/library/typing.html#typing.cast）。 有关详细信息，请参阅 [`casts`](https://mypy.readthedocs.io/en/stable/type_narrowing.html#casts)。

    请注意，您可以显式声明变量的类型而不为其指定初始值：

    ```python
        # 我们只解压两个值，因此 mypy 没有右侧值来推断“cs”的类型：
        a, b, *cs = 1, 2  # error: 需要“cs”的类型注解
        
        rs: list[int]  # no assignment!
        p, q, *rs = 1, 2  # OK
    ```

=== "英文"

    **Explicit types for variables**

    You can override the inferred type of a variable by using a variable type annotation:

    ```python
    from typing import Union

    x: Union[int, str] = 1
    ```

    Without the type annotation, the type of ``x`` would be just ``int``. We use an annotation to give it a more general type ``Union[int, str]`` (this type means that the value can be either an ``int`` or a ``str``).
    
    The best way to think about this is that the type annotation sets the type of the variable, not the type of the expression. For instance, mypy will complain about the following code:

    ```python
    x: Union[int, str] = 1.1  # error: Incompatible types in assignment
                              # (expression has type "float", variable has type "Union[int, str]")
    ```

    !!! info "Note"

        To explicitly override the type of an expression you can use [`cast(\<type\>, \<expression\>) <typing.cast>`](https://docs.python.org/3/library/typing.html#typing.cast). See [`casts`](https://mypy.readthedocs.io/en/stable/type_narrowing.html#casts) for details.

    Note that you can explicitly declare the type of a variable without giving it an initial value:

    ```python
        # We only unpack two values, so there's no right-hand side value
        # for mypy to infer the type of "cs" from:
        a, b, *cs = 1, 2  # error: Need type annotation for "cs"
        
        rs: list[int]  # no assignment!
        p, q, *rs = 1, 2  # OK
    ```

## 集合的显式类型

=== "中文"

    类型检查器不能总是推断出列表或字典的类型。 当创建空列表或字典并将其分配给没有显式变量类型的新变量时，通常会出现这种情况。 这是一个示例，其中 mypy 在没有帮助的情况下无法推断类型：

    ```python
    l = []  # Error: 需要变量 "l" 的类型注解
    ```

    在这些情况下，您可以使用类型注释显式指定类型：

    ```python
    l: list[int] = []       # 创建 int 的空列表
    d: dict[str, int] = {}  # 创建空字典（str -> int）
    ```

    !!! info "Note"

        在内置集合上使用类型参数（例如“list[int]”），例如 [`list`](https://docs.python.org/3/library/stdtypes.html#list)、[`dict`](https ://docs.python.org/3/library/stdtypes.html#dict)、[`tuple`](https://docs.python.org/3/library/stdtypes.html#tuple) 和 [` set`](https://docs.python.org/3/library/stdtypes.html#set) 仅适用于 Python 3.9 及更高版本。 对于Python 3.8及更早版本，您必须使用[`List`](https://docs.python.org/3/library/typing.html#typing.List)（例如`List[int]`），[`Dict`](https://docs.python.org/3/library/typing.html#typing.Dict)，等等。

=== "英文"

    **Explicit types for collections**

    The type checker cannot always infer the type of a list or a dictionary. This often arises when creating an empty list or dictionary and assigning it to a new variable that doesn't have an explicit variable type. Here is an example where mypy can't infer the type without some help:

    ```python
    l = []  # Error: Need type annotation for "l"
    ```

    In these cases you can give the type explicitly using a type annotation:

    ```python
    l: list[int] = []       # Create empty list of int
    d: dict[str, int] = {}  # Create empty dictionary (str -> int)
    ```

    !!! info "Note"

        Using type arguments (e.g. `list[int]`) on builtin collections like [`list`](https://docs.python.org/3/library/stdtypes.html#list),  [`dict`](https://docs.python.org/3/library/stdtypes.html#dict), [`tuple`](https://docs.python.org/3/library/stdtypes.html#tuple), and  [`set`](https://docs.python.org/3/library/stdtypes.html#set) only works in Python 3.9 and later. For Python 3.8 and earlier, you must use [`List`](https://docs.python.org/3/library/typing.html#typing.List) (e.g. `List[int]`), [`Dict`](https://docs.python.org/3/library/typing.html#typing.Dict), and so on.

## 容器类型的兼容性

=== "中文"

    快速说明：容器类型有时可能不直观。 我们将在[`variance`](https://mypy.readthedocs.io/en/stable/common_issues.html#variance)中详细讨论这一点。 例如，以下程序会生成 mypy 错误，因为 mypy 将 ``list[int]`` 视为与 ``list[object]`` 不兼容：

    ```python
    def f(l: list[object], k: list[int]) -> None:
        l = k  # error: 赋值中的类型不兼容
    ```

    不允许上述赋值的原因是允许赋值可能会导致非 int 值存储在 “int” 列表中：

    ```python
    def f(l: list[object], k: list[int]) -> None:
       l = k
       l.append('x')
       print(k[-1])  # 哎哟; list[int] 中的字符串
    ```

    其他容器类型，如 [`dict`](https://docs.python.org/3/library/stdtypes.html#dict) 和 [`set`](https://docs.python.org/3/library) /stdtypes.html#set) 的行为类似。

    您仍然可以运行上面的程序； 它打印 “x”。 这说明静态类型不会影响程序的运行时行为。 您可以运行类型检查失败的程序，这在执行大型重构时通常非常方便。 因此，您始终可以“解决”类型系统，并且它并不会真正限制您在程序中可以执行的操作。

=== "英文"

    **Compatibility of container types**

    A quick note: container types can sometimes be unintuitive. We'll discuss this more in [`variance`](https://mypy.readthedocs.io/en/stable/common_issues.html#variance). For example, the following program generates a mypy error, because mypy treats ``list[int]`` as incompatible with ``list[object]``:

    ```python
    def f(l: list[object], k: list[int]) -> None:
        l = k  # error: Incompatible types in assignment
    ```

    The reason why the above assignment is disallowed is that allowing the assignment could result in non-int values stored in a list of ``int``:

    ```python
    def f(l: list[object], k: list[int]) -> None:
       l = k
       l.append('x')
       print(k[-1])  # Ouch; a string in list[int]
    ```

    Other container types like [`dict`](https://docs.python.org/3/library/stdtypes.html#dict) and [`set`](https://docs.python.org/3/library/stdtypes.html#set) behave similarly.

    You can still run the above program; it prints ``x``. This illustrates the fact that static types do not affect the runtime behavior of programs. You can run programs with type check failures, which is often very handy when performing a large refactoring. Thus you can always 'work around' the type system, and it doesn't really limit what you can do in your program.

## 类型推断中的上下文

=== "中文"

    类型推断是*双向的*并考虑上下文。
    
    Mypy 在推断右侧表达式的类型时会考虑赋值左侧变量的类型。 例如，以下内容将类型检查：
    
    ```python
    def f(l: list[object]) -> None:
        l = [1, 2]  # 推断 [1, 2] 的类型为 list[object]，而不是 list[int]
    ```
    
    值表达式 “[1, 2]” 使用附加上下文进行类型检查，该上下文被分配给 “list[object]” 类型的变量。 这用于将*表达式*的类型推断为 “list[object]” 。
    
    声明的参数类型也用于类型上下文。 在这个程序中，mypy 知道空列表 `[]` 应该具有类型 `list[int]`，基于 `foo` 中 `arg` 的声明类型：

    ```python
    def foo(arg: list[int]) -> None:
        print('Items:', ''.join(str(a) for a in arg))
    
    foo([])  # OK
    ```
    
    但是，上下文仅在单个语句中起作用。 这里 mypy 需要空列表的注释，因为上下文仅在以下语句中可用：

    ```python
    def foo(arg: list[int]) -> None:
        print('Items:', ', '.join(arg))
    
    a = []  # Error: 需要“a”的类型注解
    foo(a)
    ```
    
    通过添加类型注解可以轻松解决该问题：

    ```Python
    ...
    a: list[int] = []  # OK
    foo(a)
    ```

=== "英文"

    **Context in type inference**

    Type inference is *bidirectional* and takes context into account.
    
    Mypy will take into account the type of the variable on the left-hand side of an assignment when inferring the type of the expression on the right-hand side. For example, the following will type check:
    
    ```python
    def f(l: list[object]) -> None:
        l = [1, 2]  # Infer type list[object] for [1, 2], not list[int]
    ```
    
    The value expression `[1, 2]` is type checked with the additional context that it is being assigned to a variable of type `list[object]`. This is used to infer the type of the *expression* as `list[object]`.
    
    Declared argument types are also used for type context. In this program mypy knows that the empty list `[]` should have type `list[int]` based on the declared type of `arg` in `foo`:

    ```python
    def foo(arg: list[int]) -> None:
        print('Items:', ''.join(str(a) for a in arg))
    
    foo([])  # OK
    ```
    
    However, context only works within a single statement. Here mypy requires an annotation for the empty list, since the context would only be available in the following statement:

    ```python
    def foo(arg: list[int]) -> None:
        print('Items:', ', '.join(arg))
    
    a = []  # Error: Need type annotation for "a"
    foo(a)
    ```
    
    Working around the issue is easy by adding a type annotation:

    ```Python
    ...
    a: list[int] = []  # OK
    foo(a)
    ```

## 消除类型错误

=== "中文"

    您可能希望禁用特定行或代码库中特定文件内的类型检查。 为此，您可以使用 `# type:ignore` 注释。
    
    例如，在最新更新中，您使用的 Web 框架现在可以将整数参数传递给“run()”，从而在该端口上的本地主机上启动它。 就像这样：

    ```python
    # 启动应用程序 http://localhost:8000
    app.run(8000)
    ```
    
    然而，开发人员忘记更新“run”的类型注释，因此 mypy 仍然认为“run”只需要“str”类型。 这会给你带来以下错误：

    ```text
    error: Argument 1 to "run" of "A" has incompatible type "int"; expected "str"
    ```
    
    如果您自己无法直接修复 Web 框架，您可以通过添加 `# type:ignore` 来临时禁用该行的类型检查：

    ```python
    # 启动应用程序 http://localhost:8000
    app.run(8000)  # type: ignore
    ```
    
    这将抑制该特定行上出现的任何 mypy 错误。
    
    您可能应该在`# type:ignore`注释上添加更多信息，以解释为什么首先添加忽略。 这可能是指向负责类型存根的存储库中的问题的链接，也可能是对该错误的简短解释。 为此，请使用以下格式：
    
    ```python
    # 启动应用程序 http://localhost:8000
    app.run(8000)  # type: ignore  # `run()` in v2.0 accepts an `int`, as a port
    ```

=== "英文"

    **Silencing type errors**

    You might want to disable type checking on specific lines, or within specific files in your codebase. To do that, you can use a `# type: ignore` comment.
    
    For example, say in its latest update, the web framework you use can now take an integer argument to `run()`, which starts it on localhost on that port. Like so:

    ```python
    # Starting app on http://localhost:8000
    app.run(8000)
    ```
    
    However, the devs forgot to update their type annotations for `run`, so mypy still thinks `run` only expects `str` types. This would give you the following error:

    ```text
    error: Argument 1 to "run" of "A" has incompatible type "int"; expected "str"
    ```
    
    If you cannot directly fix the web framework yourself, you can temporarily disable type checking on that line, by adding a `# type: ignore`:

    ```python
    # Starting app on http://localhost:8000
    app.run(8000)  # type: ignore
    ```
    
    This will suppress any mypy errors that would have raised on that specific line.
    
    You should probably add some more information on the `# type: ignore` comment, to explain why the ignore was added in the first place. This could be a link to an issue on the repository responsible for the type stubs, or it could be a short explanation of the bug. To do that, use this format:
    
    ```python
    # Starting app on http://localhost:8000
    app.run(8000)  # type: ignore  # `run()` in v2.0 accepts an `int`, as a port
    ```

### 输入忽略错误代码

=== "中文"

    默认情况下，mypy 显示每个错误的错误代码：
    
    ```text
    error: "str" has no attribute "trim"  [attr-defined]
    ```
    
    可以在忽略注释中添加特定的错误代码（例如 `# type:ignore[attr-defined]`）以阐明正在沉默的内容。 您可以在[此处`](https://mypy.readthedocs.io/en/stable/error_codes.html#silence-error-codes)找到有关错误代码的更多信息。

=== "英文"

    **Type ignore error codes**

    By default, mypy displays an error code for each error:
    
    ```text
    error: "str" has no attribute "trim"  [attr-defined]
    ```
    
    It is possible to add a specific error-code in your ignore comment (e.g. `# type: ignore[attr-defined]`) to clarify what's being silenced. You can find more information about error codes [`here`](https://mypy.readthedocs.io/en/stable/error_codes.html#silence-error-codes).

### 其他消除错误的方法

=== "中文"

    您可以通过使用“Any”动态键入特定变量来让 mypy 消除有关特定变量的错误。 有关更多信息，请参阅[`dynamic-typing`](https://mypy.readthedocs.io/en/stable/dynamic_typing.html#dynamic-typing)。
    
    ```python
    from typing import Any
    
    def f(x: Any, y: str) -> None:
        x = 'hello'
        x += 1  # OK
    ```
    
    您可以通过在文件顶部添加 `# mypy:ignore-errors` 来忽略文件中的所有 mypy 错误：
    
    ```python
    # mypy: ignore-errors
    # 这是一个测试文件，跳过其中的类型检查。
    import unittest
    ...
    ```
    
    您还可以在 [`mypy 配置文件`](https://mypy.readthedocs.io/en/stable/config_file.html#config-file) 中指定每个模块的配置选项。 例如：
    
    ```ini
    # Don't report errors in the 'package_to_fix_later' package
    [mypy-package_to_fix_later.*]
    ignore_errors = True
    
    # Disable specific error codes in the 'tests' package
    # Also don't require type annotations
    [mypy-tests.*]
    disable_error_code = var-annotated, has-type
    allow_untyped_defs = True
    
    # Silence import errors from the 'library_missing_types' package
    [mypy-library_missing_types.*]
    ignore_missing_imports = True
    ```
    
    Finally, adding a `@typing.no_type_check` decorator to a class, method or
    function causes mypy to avoid type checking that class, method or function
    and to treat it as not having any type annotations.
    
    ```python
    @typing.no_type_check
    def foo() -> str:
       return 12345  # No error!
    ```

=== "英文"

    **Other ways to silence errors**

    You can get mypy to silence errors about a specific variable by dynamically typing it with `Any`. See [`dynamic-typing`](https://mypy.readthedocs.io/en/stable/dynamic_typing.html#dynamic-typing) for more information.
    
    ```python
    from typing import Any
    
    def f(x: Any, y: str) -> None:
        x = 'hello'
        x += 1  # OK
    ```
    
    You can ignore all mypy errors in a file by adding a `# mypy: ignore-errors` at the top of the file:
    
    ```python
    # mypy: ignore-errors
    # This is a test file, skipping type checking in it.
    import unittest
    ...
    ```
    
    You can also specify per-module configuration options in your [`The mypy configuration file`](https://mypy.readthedocs.io/en/stable/config_file.html#config-file). For example:
    
    ```ini
    # Don't report errors in the 'package_to_fix_later' package
    [mypy-package_to_fix_later.*]
    ignore_errors = True
    
    # Disable specific error codes in the 'tests' package
    # Also don't require type annotations
    [mypy-tests.*]
    disable_error_code = var-annotated, has-type
    allow_untyped_defs = True
    
    # Silence import errors from the 'library_missing_types' package
    [mypy-library_missing_types.*]
    ignore_missing_imports = True
    ```
    
    Finally, adding a `@typing.no_type_check` decorator to a class, method or
    function causes mypy to avoid type checking that class, method or function
    and to treat it as not having any type annotations.
    
    ```python
    @typing.no_type_check
    def foo() -> str:
       return 12345  # No error!
    ```
