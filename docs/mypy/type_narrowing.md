# 类型收缩

=== "中文"

    本节专门介绍 mypy 支持的几种类型收缩技术。
    
    类型缩小是指您让类型检查器相信更广泛的类型实际上更具体，例如，`Shape` 类型的对象实际上是更窄的 `Square` 类型。

=== "英文"

    **Type narrowing**

    This section is dedicated to  several type narrowing techniques which are supported by mypy.
    
    Type narrowing is when you convince a type checker that a broader type is actually more specific, for instance, that an object of type `Shape` is actually of the narrower type `Square`.

## 类型收缩表达式

Type narrowing expressions

=== "中文"

    缩小类型范围的最简单方法是使用受支持的表达式之一：

    - [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) 就像 `isinstance(obj, float)` 会将 `obj` 缩小为 `float` 类型
    - [`issubclass`](https://docs.python.org/3/library/functions.html#issubclass) 就像 `issubclass(cls, MyClass)` 会将`cls` 缩小为 `Type[MyClass]` 类型
    - [`type`](https://docs.python.org/3/library/functions.html#type) 就像 `type(obj) is int` 会将 `obj` 缩小为 `int` 类型
    - [`callable`](https://docs.python.org/3/library/functions.html#callable) 就像 `callable(obj)` 将对象缩小为可调用类型

    类型缩小是根据上下文而定的。 例如，根据条件，mypy 将仅在`if`分支内缩小表达式：

    ```python
    def function(arg: object):
        if isinstance(arg, int):
            # 类型仅在“if”分支内缩小
            reveal_type(arg)  # 显示类型：“builtins.int”
        elif isinstance(arg, str) or isinstance(arg, bool):
            # 在这个 elif 分支中，类型以不同的方式缩小：
            reveal_type(arg)  # 显示类型：“builtins.str |builtins.bool”

            # 后续的缩小操作将进一步缩小类型
            if isinstance(arg, bool):
                reveal_type(arg)  # 显示类型: "builtins.bool"

        # 回到“if”语句之外，类型没有缩小：
        reveal_type(arg)  # 显示类型: "builtins.object"
    ```

    Mypy 理解`return`或异常引发对对象类型的影响：

    ```python
    def function(arg: int | str):
        if isinstance(arg, int):
            return

        # 此时 `arg` 不能是 `int`：
        reveal_type(arg)  # 显示类型: "builtins.str"
    ```

    我们还可以使用`assert`来缩小相同上下文中的类型：

    ```python
    def function(arg: Any):
        assert isinstance(arg, int)
        reveal_type(arg)  # 显示类型: "builtins.int"
    ```

    !!! info "Note"

        使用 [`--warn-unreachable`](https://mypy.readthedocs.io/en/latest/command_line.html#cmdoption-mypy-warn-unreachable) 将类型缩小到某些不可能的状态将被视为错误。

        ```python
        def function(arg: int):
            # error: “int”和“str”的子类不能存在：
            # 会有不兼容的方法签名
            assert isinstance(arg, str)

            # error: 声明无法访问(Statement is unreachable)
            print("so mypy concludes the assert will always trigger")
        ```

        如果没有`--warn-unreachable`，mypy 将不会检查它认为无法访问的代码。 有关更多信息，请参阅[`无法访问的代码`](https://mypy.readthedocs.io/en/latest/common_issues.html#unreachable)。

        ```python
        x: int = 1
        assert isinstance(x, str)
        reveal_type(x)  # 显示类型 "builtins.int"
        print(x + '!')  # 使用`mypy`进行类型检查，但在运行时失败。
        ```

=== "英文"

    The simplest way to narrow a type is to use one of the supported expressions:

    - [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) like in `isinstance(obj, float)` will narrow `obj` to have `float` type
    - [`issubclass`](https://docs.python.org/3/library/functions.html#issubclass) like in `issubclass(cls, MyClass)` will narrow `cls` to be `Type[MyClass]`
    - [`type`](https://docs.python.org/3/library/functions.html#type) like in `type(obj) is int` will narrow `obj` to have `int` type
    - [`callable`](https://docs.python.org/3/library/functions.html#callable) like in `callable(obj)` will narrow object to callable type

    Type narrowing is contextual. For example, based on the condition, mypy will narrow an expression only within an `if` branch:

    ```python
    def function(arg: object):
        if isinstance(arg, int):
            # Type is narrowed within the ``if`` branch only
            reveal_type(arg)  # Revealed type: "builtins.int"
        elif isinstance(arg, str) or isinstance(arg, bool):
            # Type is narrowed differently within this ``elif`` branch:
            reveal_type(arg)  # Revealed type: "builtins.str | builtins.bool"

            # Subsequent narrowing operations will narrow the type further
            if isinstance(arg, bool):
                reveal_type(arg)  # Revealed type: "builtins.bool"

        # Back outside of the ``if`` statement, the type isn't narrowed:
        reveal_type(arg)  # Revealed type: "builtins.object"
    ```

    Mypy understands the implications `return` or exception raising can have
    for what type an object could be:

    ```python
    def function(arg: int | str):
        if isinstance(arg, int):
            return

        # `arg` can't be `int` at this point:
        reveal_type(arg)  # Revealed type: "builtins.str"
    ```

    We can also use `assert` to narrow types in the same context:

    ```python
    def function(arg: Any):
        assert isinstance(arg, int)
        reveal_type(arg)  # Revealed type: "builtins.int"
    ```

    !!! info "Note"

        With [`--warn-unreachable`](https://mypy.readthedocs.io/en/latest/command_line.html#cmdoption-mypy-warn-unreachable) narrowing types to some impossible state will be treated as an error.

        ```python
        def function(arg: int):
            # error: Subclass of "int" and "str" cannot exist:
            # would have incompatible method signatures
            assert isinstance(arg, str)

            # error: Statement is unreachable
            print("so mypy concludes the assert will always trigger")
        ```

        Without `--warn-unreachable` mypy will simply not check code it deems to be unreachable. See [`unreachable code`](https://mypy.readthedocs.io/en/latest/common_issues.html#unreachable) for more information.

        ```python
        x: int = 1
        assert isinstance(x, str)
        reveal_type(x)  # Revealed type is "builtins.int"
        print(x + '!')  # Typechecks with `mypy`, but fails in runtime.
        ```

### 是否为子类

issubclass

=== "中文"

    Mypy 还可以使用 [`issubclass`](https://docs.python.org/3/library/functions.html#issubclass) 在处理类型和元类时进行更好的类型推断：

    ```python
    class MyCalcMeta(type):
        @classmethod
        def calc(cls) -> int:
            ...

    def f(o: object) -> None:
        t = type(o)  # 我们必须在这里使用一个变量
        reveal_type(t)  # 揭示的类型是 "builtins.type"

        if issubclass(t, MyCalcMeta):  # `issubclass(type(o), MyCalcMeta)` 不起作用
            reveal_type(t)  # 揭示的类型是 "Type[MyCalcMeta]"
            t.calc()  # Okay
    ```

=== "英文"

    Mypy can also use [`issubclass`](https://docs.python.org/3/library/functions.html#issubclass) for better type inference when working with types and metaclasses:

    ```python
    class MyCalcMeta(type):
        @classmethod
        def calc(cls) -> int:
            ...

    def f(o: object) -> None:
        t = type(o)  # We must use a variable here
        reveal_type(t)  # Revealed type is "builtins.type"

        if issubclass(t, MyCalcMeta):  # `issubclass(type(o), MyCalcMeta)` won't work
            reveal_type(t)  # Revealed type is "Type[MyCalcMeta]"
            t.calc()  # Okay
    ```

### 是否可调用

callable

=== "中文"

    Mypy 在类型检查期间知道哪些类型可调用，哪些类型不可调用。 所以，我们知道 `callable()` 会返回什么。 例如：

    ```python
    from typing import Callable

    x: Callable[[], int]

    if callable(x):
        reveal_type(x)  # N: 揭示的类型是 "def () -> builtins.int"
    else:
        ...  # 永远不会被执行并且会引发错误 `--warn-unreachable`
    ```

    [`callable`](https://docs.python.org/3/library/functions.html#callable) 函数甚至可以将 `Union` 类型拆分为可调用部分和不可调用部分：

    ```python
    from typing import Callable, Union

    x: Union[int, Callable[[], int]]

    if callable(x):
        reveal_type(x)  # N: 揭示的类型是 "def () -> builtins.int"
    else:
        reveal_type(x)  # N: 揭示的类型是 "builtins.int"
    ```

=== "英文"

    Mypy knows what types are callable and which ones are not during type checking. So, we know what `callable()` will return. For example:

    ```python
    from typing import Callable

    x: Callable[[], int]

    if callable(x):
        reveal_type(x)  # N: Revealed type is "def () -> builtins.int"
    else:
        ...  # Will never be executed and will raise error with `--warn-unreachable`
    ```

    [`callable`](https://docs.python.org/3/library/functions.html#callable) function can even split `Union` type for callable and non-callable parts:

    ```python
    from typing import Callable, Union

    x: Union[int, Callable[[], int]]

    if callable(x):
        reveal_type(x)  # N: Revealed type is "def () -> builtins.int"
    else:
        reveal_type(x)  # N: Revealed type is "builtins.int"
    ```

## 转换

Casts

=== "中文"

    Mypy 支持类型转换，通常用于将静态类型值强制转换为子类型。 然而，与 Java 或 C# 等语言不同，mypy 强制转换仅用作类型检查器的提示，并且不执行运行时类型检查。 使用函数 [`typing.cast`](https://docs.python.org/3/library/typing.html#typing.cast) 执行转换：

    ```python
    from typing import cast

    o: object = [1]
    x = cast(list[int], o)  # OK
    y = cast(list[str], o)  # OK (强制转换不执行实际的运行时检查)
    ```

    为了支持上述类型转换的运行时检查，我们必须检查所有列表项的类型，这对于大型列表来说效率非常低。 强制转换用于消除虚假的类型检查器警告，并在类型检查器无法完全理解正在发生的情况时为类型检查器提供一些帮助。

    !!! info "Note"

        You can use an assertion if you want to perform an actual runtime check:

        ```python
        def foo(o: object) -> None:
            print(o + 5)  # Error: can't add 'object' and 'int'
            assert isinstance(o, int)
            print(o + 5)  # OK: type of 'o' is 'int' here
        ```


    正如前面所解释的，您不需要对`Any`类型的表达式进行强制转换，或者在分配给类型为`Any`的变量时进行强制转换。 您还可以使用`Any`作为转换目标类型——这允许您对结果执行任何操作。 例如：

    ```python
    from typing import cast, Any

    x = 1
    x.whatever()  # Type check error
    y = cast(Any, x)
    y.whatever()  # Type check OK (runtime error)
    ```

=== "英文"

    Mypy supports type casts that are usually used to coerce a statically typed value to a subtype. Unlike languages such as Java or C#, however, mypy casts are only used as hints for the type checker, and they don't perform a runtime type check. Use the function [`typing.cast`](https://docs.python.org/3/library/typing.html#typing.cast) to perform a cast:

    ```python
    from typing import cast

    o: object = [1]
    x = cast(list[int], o)  # OK
    y = cast(list[str], o)  # OK (cast performs no actual runtime check)
    ```

    To support runtime checking of casts such as the above, we'd have to check the types of all list items, which would be very inefficient for large lists. Casts are used to silence spurious type checker warnings and give the type checker a little help when it can't quite understand what is going on.

    !!! info "Note"

        You can use an assertion if you want to perform an actual runtime check:

        ```python
        def foo(o: object) -> None:
            print(o + 5)  # Error: can't add 'object' and 'int'
            assert isinstance(o, int)
            print(o + 5)  # OK: type of 'o' is 'int' here
        ```


    You don't need a cast for expressions with type `Any`, or when assigning to a variable with type `Any`, as was explained earlier. You can also use `Any` as the cast target type -- this lets you perform any operations on the result. For example:

    ```python
    from typing import cast, Any

    x = 1
    x.whatever()  # Type check error
    y = cast(Any, x)
    y.whatever()  # Type check OK (runtime error)
    ```

## 用户定义的类型保护

User-Defined Type Guards

=== "中文"

    Mypy 支持用户定义的类型防护 ([`PEP 647`](https://peps.python.org/pep-0647/)).

    类型保护是程序影响类型检查器基于运行时检查所采用的条件类型缩小的一种方式。

    基本上，`TypeGuard`是`bool`类型的“智能(smart)”别名。 让我们看一下常规的`bool`示例：

    ```python
    def is_str_list(val: list[object]) -> bool:
    """判断列表中的所有对象是否都是字符串"""
    return all(isinstance(x, str) for x in val)

    def func1(val: list[object]) -> None:
        if is_str_list(val):
            reveal_type(val)  # Reveals list[object]
            print(" ".join(val)) # Error: incompatible type
    ```

    与`TypeGuard`相同的示例:

    ```python
    from typing import TypeGuard  # use `typing_extensions` for Python 3.9 and below

    def is_str_list(val: list[object]) -> TypeGuard[list[str]]:
        """判断列表中的所有对象是否都是字符串"""
        return all(isinstance(x, str) for x in val)

    def func1(val: list[object]) -> None:
        if is_str_list(val):
            reveal_type(val)  # list[str]
            print(" ".join(val)) # ok
    ```

    它是如何工作的？ `TypeGuard` 将第一个函数参数 (`val`) 缩小为指定为第一个类型参数 (`list[str]`) 的类型。

    !!! info "Note"

        缩小范围[不严格(not strict)](https://www.python.org/dev/peps/pep-0647/#enforcing-strict-narrowing)。 例如，您可以将`str`缩小为`int`：

        ```python
        def f(value: str) -> TypeGuard[int]:
            return True
        ```

        注意: 由于不强制执行严格的缩小，因此很容易破坏类型安全。

        然而，坚定或不知情的开发人员可以通过多种方式破坏类型安全——最常见的是使用强制转换或 Any。 如果 Python 开发人员花时间在代码中学习和实现用户定义的类型保护，则可以安全地假设他们对类型安全感兴趣，并且不会以破坏类型安全或破坏类型安全的方式编写类型保护函数。 产生无意义的结果。

=== "英文"

    Mypy supports User-Defined Type Guards ([`PEP 647`](https://peps.python.org/pep-0647/)).

    A type guard is a way for programs to influence conditional type narrowing employed by a type checker based on runtime checks.

    Basically, a `TypeGuard` is a "smart" alias for a `bool` type. Let's have a look at the regular `bool` example:

    ```python
    def is_str_list(val: list[object]) -> bool:
    """Determines whether all objects in the list are strings"""
    return all(isinstance(x, str) for x in val)

    def func1(val: list[object]) -> None:
        if is_str_list(val):
            reveal_type(val)  # Reveals list[object]
            print(" ".join(val)) # Error: incompatible type
    ```

    The same example with `TypeGuard`:

    ```python
    from typing import TypeGuard  # use `typing_extensions` for Python 3.9 and below

    def is_str_list(val: list[object]) -> TypeGuard[list[str]]:
        """Determines whether all objects in the list are strings"""
        return all(isinstance(x, str) for x in val)

    def func1(val: list[object]) -> None:
        if is_str_list(val):
            reveal_type(val)  # list[str]
            print(" ".join(val)) # ok
    ```

    How does it work? `TypeGuard` narrows the first function argument (`val`) to the type specified as the first type parameter (`list[str]`).

    !!! info "Note"

        Narrowing is [not strict](https://www.python.org/dev/peps/pep-0647/#enforcing-strict-narrowing). For example, you can narrow `str` to `int`:

        ```python
        def f(value: str) -> TypeGuard[int]:
            return True
        ```

        Note: since strict narrowing is not enforced, it's easy to break type safety.

        However, there are many ways a determined or uninformed developer can subvert type safety -- most commonly by using cast or Any. If a Python developer takes the time to learn about and implement user-defined type guards within their code, it is safe to assume that they are interested in type safety and will not write their type guard functions in a way that will undermine type safety or produce nonsensical results.

### 泛型类型保护

=== "中文"

    `TypeGuard` 也可以使用泛型类型：

    ```python
    from typing import TypeVar
    from typing import TypeGuard  # use `typing_extensions` for `python<3.10`

    _T = TypeVar("_T")

    def is_two_element_tuple(val: tuple[_T, ...]) -> TypeGuard[tuple[_T, _T]]:
        return len(val) == 2

    def func(names: tuple[str, ...]):
        if is_two_element_tuple(names):
            reveal_type(names)  # tuple[str, str]
        else:
            reveal_type(names)  # tuple[str, ...]
    ```

=== "英文"

    `TypeGuard` can also work with generic types:

    ```python
    from typing import TypeVar
    from typing import TypeGuard  # use `typing_extensions` for `python<3.10`

    _T = TypeVar("_T")

    def is_two_element_tuple(val: tuple[_T, ...]) -> TypeGuard[tuple[_T, _T]]:
        return len(val) == 2

    def func(names: tuple[str, ...]):
        if is_two_element_tuple(names):
            reveal_type(names)  # tuple[str, str]
        else:
            reveal_type(names)  # tuple[str, ...]
    ```

### 带参数的类型保护

Typeguards with parameters

=== "中文"

    类型保护函数可以接受额外的参数：

    ```python
    from typing import Type, TypeVar
    from typing import TypeGuard  # use `typing_extensions` for `python<3.10`

    _T = TypeVar("_T")

    def is_set_of(val: set[Any], type: Type[_T]) -> TypeGuard[set[_T]]:
        return all(isinstance(x, type) for x in val)

    items: set[Any]
    if is_set_of(items, str):
        reveal_type(items)  # set[str]
    ```

=== "英文"

    Type guard functions can accept extra arguments:

    ```python
    from typing import Type, TypeVar
    from typing import TypeGuard  # use `typing_extensions` for `python<3.10`

    _T = TypeVar("_T")

    def is_set_of(val: set[Any], type: Type[_T]) -> TypeGuard[set[_T]]:
        return all(isinstance(x, type) for x in val)

    items: set[Any]
    if is_set_of(items, str):
        reveal_type(items)  # set[str]
    ```

### 作为方法的类型保护

TypeGuards as methods

=== "中文"

    > 方法也可以充当`TypeGuard`：

    ```python
    class StrValidator:
        def is_valid(self, instance: object) -> TypeGuard[str]:
            return isinstance(instance, str)

    def func(to_validate: object) -> None:
        if StrValidator().is_valid(to_validate):
            reveal_type(to_validate)  # Revealed type is "builtins.str"
    ```

    !!! info "Note"

        请注意，`TypeGuard`[不会缩小](https://www.python.org/dev/peps/pep-0647/#narrowing-of-implicit-self-and-cls-parameters)`self`的类型 或`cls`隐式参数。

        如果需要缩小`self`或`cls`，则可以将该值作为显式参数传递给类型保护函数：

        ```python
        class Parent:
            def method(self) -> None:
                reveal_type(self)  # Revealed type is "Parent"
                if is_child(self):
                    reveal_type(self)  # Revealed type is "Child"

        class Child(Parent):
            ...

        def is_child(instance: Parent) -> TypeGuard[Child]:
            return isinstance(instance, Child)
        ```

=== "英文"

    > A method can also serve as the `TypeGuard`:

    ```python
    class StrValidator:
        def is_valid(self, instance: object) -> TypeGuard[str]:
            return isinstance(instance, str)

    def func(to_validate: object) -> None:
        if StrValidator().is_valid(to_validate):
            reveal_type(to_validate)  # Revealed type is "builtins.str"
    ```

    !!! info "Note"

        Note, that `TypeGuard` [does not narrow](https://www.python.org/dev/peps/pep-0647/#narrowing-of-implicit-self-and-cls-parameters) types of `self` or `cls` implicit arguments.

        If narrowing of `self` or `cls` is required, the value can be passed as an explicit argument to a type guard function:

        ```python
        class Parent:
            def method(self) -> None:
                reveal_type(self)  # Revealed type is "Parent"
                if is_child(self):
                    reveal_type(self)  # Revealed type is "Child"

        class Child(Parent):
            ...

        def is_child(instance: Parent) -> TypeGuard[Child]:
            return isinstance(instance, Child)
        ```

### 作为类型保护的赋值表达式

Assignment expressions as TypeGuards

=== "中文"

    有时您可能需要创建一个新变量并同时将其缩小为某种特定类型。 这可以通过将`TypeGuard`与 [:= 运算符](https://docs.python.org/3/whatsnew/3.8.html#assignment-expressions) 一起使用来实现。

    ```python
    from typing import TypeGuard  # use `typing_extensions` for `python<3.10`

    def is_float(a: object) -> TypeGuard[float]:
        return isinstance(a, float)

    def main(a: object) -> None:
        if is_float(x := a):
            reveal_type(x)  # N: Revealed type is 'builtins.float'
            reveal_type(a)  # N: Revealed type is 'builtins.object'
        reveal_type(x)  # N: Revealed type is 'builtins.object'
        reveal_type(a)  # N: Revealed type is 'builtins.object'
    ```

    这里会发生什么？

    1. 我们创建一个新变量`x`并为其分配值`a`
    2. 我们在`x`上运行`is_float()`类型保护
    3. 它将`x`缩小为`if`上下文中的`float`，并且不涉及`a`

    !!! info "Note"

        这同样适用于`isinstance(x := a, float)`。

=== "英文"

    Sometimes you might need to create a new variable and narrow it to some specific type at the same time. This can be achieved by using `TypeGuard` together with [:= operator](https://docs.python.org/3/whatsnew/3.8.html#assignment-expressions).

    ```python
    from typing import TypeGuard  # use `typing_extensions` for `python<3.10`

    def is_float(a: object) -> TypeGuard[float]:
        return isinstance(a, float)

    def main(a: object) -> None:
        if is_float(x := a):
            reveal_type(x)  # N: Revealed type is 'builtins.float'
            reveal_type(a)  # N: Revealed type is 'builtins.object'
        reveal_type(x)  # N: Revealed type is 'builtins.object'
        reveal_type(a)  # N: Revealed type is 'builtins.object'
    ```

    What happens here?

    1. We create a new variable `x` and assign a value of `a` to it
    2. We run `is_float()` type guard on `x`
    3. It narrows `x` to be `float` in the `if` context and does not touch `a`

    !!! info "Note"

        The same will work with `isinstance(x := a, float)` as well.
