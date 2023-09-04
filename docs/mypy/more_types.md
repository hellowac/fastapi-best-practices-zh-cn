# 更多类型

=== "中文"

    本节介绍了一些其他类型，包括 [`typing.NoReturn`](https://docs.python.org/3/library/typing.html#typing.NoReturn)、[`typing.NewType`]( https://docs.python.org/3/library/typing.html#typing.NewType），以及异步代码的类型。 它还讨论了如何使用重载为函数提供更精确的类型。 所有这些仅在特定情况下有用，因此请随意跳过本节，当您需要其中一些时再回来。
    
    以下是本文所涵盖内容的快速摘要：
    
    - [`typing.NoReturn`](https://docs.python.org/3/library/typing.html#typing.NoReturn) 让您告诉 mypy 函数永远不会正常返回。
    - [`typing.NewType`](https://docs.python.org/3/library/typing.html#typing.NewType) 允许您定义类型的协变，该协变被 mypy 视为单独的类型，但在运行时与原始类型相同。 例如，您可以将 `UserId` 作为 `int` 的协变，它在运行时只是一个`int`。
    - [`typing.overload`](https://docs.python.org/3/library/typing.html#typing.overload) 让您定义一个可以接受多个不同签名的函数。 如果您需要对参数和返回类型之间的关系进行编码，而这种关系很难正常表达，那么这非常有用。
    - 异步类型允许您使用 `async` 和 `await` 的同时支持检查编码类型。

=== "英文"

    **More types**

    This section introduces a few additional kinds of types, including [`typing.NoReturn`](https://docs.python.org/3/library/typing.html#typing.NoReturn), [`typing.NewType`](https://docs.python.org/3/library/typing.html#typing.NewType), and types for async code. It also discusses how to give functions more precise types using overloads. All of these are only situationally useful, so feel free to skip this section and come back when you have a need for some of them.
    
    Here's a quick summary of what's covered here:
    
    - [`typing.NoReturn`](https://docs.python.org/3/library/typing.html#typing.NoReturn) lets you tell mypy that a function never returns normally.
    - [`typing.NewType`](https://docs.python.org/3/library/typing.html#typing.NewType) lets you define a variant of a type that is treated as a separate type by mypy but is identical to the original type at runtime. For example, you can have `UserId` as a variant of `int` that is just an `int` at runtime.
    - [`typing.overload`](https://docs.python.org/3/library/typing.html#typing.overload) lets you define a function that can accept multiple distinct signatures. This is useful if you need to encode a relationship between the arguments and the return type that would be difficult to express normally.
    - Async types let you type check programs using `async` and `await`.

## NoReturn类型

The NoReturn type

=== "中文"

    Mypy 提供对永不返回的函数的支持。 例如，无条件引发异常的函数：

    ```python
    from typing import NoReturn

    def stop() -> NoReturn:
        raise Exception('no way')
    ```

    Mypy will ensure that functions annotated as returning [`NoReturn`](https://docs.python.org/3/library/typing.html#typing.NoReturn) truly never return, either implicitly or explicitly. Mypy will also recognize that the code after calls to such functions is unreachable and will behave accordingly:

    ```python
    def f(x: int) -> int:
        if x == 0:
            return x
        stop()
        return 'whatever works'  # No error in an unreachable block
    ```

    Mypy 将确保注释为返回 [`NoReturn`](https://docs.python.org/3/library/typing.html#typing.NoReturn) 的函数真正永远不会返回，无论是隐式还是显式。 Mypy 还会识别出调用此类函数后的代码是无法访问的，并会做出相应的行为：

    ```text
    python3 -m pip install --upgrade typing-extensions
    ```

=== "英文"

    Mypy provides support for functions that never return. For example, a function that unconditionally raises an exception:

    ```python
    from typing import NoReturn

    def stop() -> NoReturn:
        raise Exception('no way')
    ```

    Mypy will ensure that functions annotated as returning [`NoReturn`](https://docs.python.org/3/library/typing.html#typing.NoReturn) truly never return, either implicitly or explicitly. Mypy will also recognize that the code after calls to such functions is unreachable and will behave accordingly:

    ```python
    def f(x: int) -> int:
        if x == 0:
            return x
        stop()
        return 'whatever works'  # No error in an unreachable block
    ```

    In earlier Python versions you need to install `typing_extensions` using pip to use [`NoReturn`](https://docs.python.org/3/library/typing.html#typing.NoReturn) in your code. Python 3 command line:

    ```text
    python3 -m pip install --upgrade typing-extensions
    ```

## NewType类型

NewTypes

=== "中文"

    在某些情况下，您可能希望通过创建仅用于区分某些值与基类实例的简单派生类来避免编程错误。 例子：

    ```python
    class UserId(int):
        pass

    def get_by_user_id(user_id: UserId):
        ...
    ```

    然而，这种方法引入了一些运行时开销。 为了避免这种情况，类型模块提供了一个辅助对象 “NewType”，它创建简单的唯一类型，运行时开销几乎为零。 
    Mypy 会将语句 `Derived = NewType('Derived', Base)` 视为大致相当于以下定义：

    ```python
    class Derived(Base):
        def __init__(self, _x: Base) -> None:
            ...
    ```

    但是，在运行时， `NewType('Derived', Base)` 将返回一个虚拟可调用对象，该可调用对象仅返回其参数：

    ```python
    def Derived(_x):
        return _x
    ```

    Mypy 将需要从需要 “UserId” 的 “int” 显式转换，而需要“int”的地方从“UserId”隐式转换。 例子：

    ```python
    from typing import NewType

    UserId = NewType('UserId', int)

    def name_by_id(user_id: UserId) -> str:
        ...

    UserId('user')          # Fails type check

    name_by_id(42)          # Fails type check
    name_by_id(UserId(42))  # OK

    num: int = UserId(5) + 1
    ```

    `NewType` 只接受两个参数。 第一个参数必须是包含新类型名称的字符串文字，并且必须等于分配新类型的变量的名称。 第二个参数必须是一个正确的可子类，即不是像 [`Union`](https://docs.python.org/3/library/typing.html#typing.Union) 等类型构造。

    `NewType` 返回的可调用对象仅接受一个参数； 这相当于只支持一个接受基类实例的构造函数（见上文）。 例子：

    ```python
    from typing import NewType

    class PacketId:
        def __init__(self, major: int, minor: int) -> None:
            self._major = major
            self._minor = minor

    TcpPacketId = NewType('TcpPacketId', PacketId)

    packet = PacketId(100, 100)
    tcp_packet = TcpPacketId(packet)  # OK

    tcp_packet = TcpPacketId(127, 0)  # Fails in type checker and at runtime
    ```

    您不能使用 [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) 或 [`issubclass`](https://docs.python.org/3/library/function.html#issubclass) 在 `NewType` 返回的对象上，也不能对 `NewType` 返回的对象进行子类化。

    !!! info "Note"

        与类型别名不同，“NewType” 在使用时将创建一个全新且唯一的类型。 `NewType` 的预期目的是帮助您检测意外地将旧基本类型和新派生类型混合在一起的情况。

        例如，使用类型别名时，以下内容将成功进行类型检查：

        ```python
        UserId = int

        def name_by_id(user_id: UserId) -> str:
            ...

        name_by_id(3)  # int 和 UserId 是同义词
        ```

        但使用 `NewType` 的类似示例不会进行类型检查：

        ```python
        from typing import NewType

        UserId = NewType('UserId', int)

        def name_by_id(user_id: UserId) -> str:
            ...

        name_by_id(3)  # int 与 UserId 不同
        ```

=== "英文"

    There are situations where you may want to avoid programming errors by creating simple derived classes that are only used to distinguish certain values from base class instances. Example:

    ```python
    class UserId(int):
        pass

    def get_by_user_id(user_id: UserId):
        ...
    ```

    However, this approach introduces some runtime overhead. To avoid this, the typing module provides a helper object `NewType ` that creates simple unique types with almost zero runtime overhead. Mypy will treat the statement `Derived = NewType('Derived', Base)` as being roughly equivalent to the following definition:

    ```python
    class Derived(Base):
        def __init__(self, _x: Base) -> None:
            ...
    ```

    However, at runtime, `NewType('Derived', Base)` will return a dummy callable that simply returns its argument:

    ```python
    def Derived(_x):
        return _x
    ```

    Mypy will require explicit casts from `int` where `UserId` is expected, while implicitly casting from `UserId` where `int` is expected. Examples:

    ```python
    from typing import NewType

    UserId = NewType('UserId', int)

    def name_by_id(user_id: UserId) -> str:
        ...

    UserId('user')          # Fails type check

    name_by_id(42)          # Fails type check
    name_by_id(UserId(42))  # OK

    num: int = UserId(5) + 1
    ```

    `NewType` accepts exactly two arguments. The first argument must be a string literal containing the name of the new type and must equal the name of the variable to which the new type is assigned. The second argument must be a properly subclassable class, i.e., not a type construct like [`Union`](https://docs.python.org/3/library/typing.html#typing.Union), etc.

    The callable returned by `NewType` accepts only one argument; this is equivalent to supporting only one constructor accepting an instance of the base class (see above). Example:

    ```python
    from typing import NewType

    class PacketId:
        def __init__(self, major: int, minor: int) -> None:
            self._major = major
            self._minor = minor

    TcpPacketId = NewType('TcpPacketId', PacketId)

    packet = PacketId(100, 100)
    tcp_packet = TcpPacketId(packet)  # OK

    tcp_packet = TcpPacketId(127, 0)  # Fails in type checker and at runtime
    ```

    You cannot use [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) or [`issubclass`](https://docs.python.org/3/library/functions.html#issubclass) on the object returned by `NewType`, nor can you subclass an object returned by `NewType`.

    !!! info "Note"

        Unlike type aliases, `NewType ` will create an entirely new and unique type when used. The intended purpose of `NewType ` is to help you detect cases where you accidentally mixed together the old base type and the new derived type.

        For example, the following will successfully typecheck when using type aliases:

        ```python
        UserId = int

        def name_by_id(user_id: UserId) -> str:
            ...

        name_by_id(3)  # ints and UserId are synonymous
        ```

        But a similar example using `NewType ` will not typecheck:

        ```python
        from typing import NewType

        UserId = NewType('UserId', int)

        def name_by_id(user_id: UserId) -> str:
            ...

        name_by_id(3)  # int is not the same as UserId
        ```

## 函数重载

Function overloading

=== "中文"

    有时，函数中的参数和类型彼此依赖，而无法使用 [`Union`](https://docs.python.org/3/library/typing.html#typing.Union) 捕获 。 例如，假设我们要编写一个可以接受 x-y 坐标的函数。 如果我们只传入一个 x-y 坐标，我们将返回一个 “ClickEvent” 对象。 但是，如果我们传入两个 x-y 坐标，我们将返回一个 “DragEvent” 对象。

    我们第一次尝试编写这个函数可能如下所示：

    ```python
    from typing import Union, Optional

    def mouse_event(x1: int,
                    y1: int,
                    x2: Optional[int] = None,
                    y2: Optional[int] = None) -> Union[ClickEvent, DragEvent]:
        if x2 is None and y2 is None:
            return ClickEvent(x1, y1)
        elif x2 is not None and y2 is not None:
            return DragEvent(x1, y1, x2, y2)
        else:
            raise TypeError("Bad arguments")
    ```

    虽然这个函数签名有效，但它太松散了：它意味着无论我们传入的参数数量如何，“mouse_event” 都可以返回任一对象。它也不禁止调用者传入错误数量的整数：mypy 会像这样对待调用 例如，“mouse_event(1, 2, 20)” 是有效的。

    我们可以通过使用 [`overloading`](https://peps.python.org/pep-0484/#function-method-overloading) 做得更好，它可以让我们为同一个函数提供多个类型注释（签名）以更准确地描述函数的行为：

    ```python
    from typing import Union, overload

    # 重载“mouse_event”的*协变*。
    # 这些协变为类型检查器提供了额外的信息。
    # 它们在运行时被忽略。

    @overload
    def mouse_event(x1: int, y1: int) -> ClickEvent: ...
    @overload
    def mouse_event(x1: int, y1: int, x2: int, y2: int) -> DragEvent: ...

    # “mouse_event”的实际*实现*。
    # 实现包含实际的运行时逻辑。
    #
    # 它可能有也可能没有类型提示。 如果是，mypy 将根据类型提示检查实现的主体。
    #
    # Mypy 还将检查并确保签名与提供的协变一致。

    def mouse_event(x1: int,
                    y1: int,
                    x2: Optional[int] = None,
                    y2: Optional[int] = None) -> Union[ClickEvent, DragEvent]:
        if x2 is None and y2 is None:
            return ClickEvent(x1, y1)
        elif x2 is not None and y2 is not None:
            return DragEvent(x1, y1, x2, y2)
        else:
            raise TypeError("Bad arguments")
    ```

    这使得 mypy 能够更准确地理解对 “mouse_event” 的调用。 例如，mypy 将理解 “mouse_event(5, 25)” 将始终具有 “ClickEvent” 的返回类型，并将报告 “mouse_event(5, 25, 2)” 等调用的错误。

    另一个例子，假设我们要编写一个自定义容器类来实现 [`__getitem__`](https://docs.python.org/3/reference/datamodel.html#object.__getitem__) 方法 (`[]` 括号索引）。 如果此方法接收到一个整数，我们将返回一个项目。 如果它收到一个“切片”，我们将返回一个项目的[“序列”](https://docs.python.org/3/library/typing.html#typing.Sequence)。

    我们可以通过使用重载来精确编码参数和返回类型之间的关系，如下所示：

    ```python
    from typing import Sequence, TypeVar, Union, overload

    T = TypeVar('T')

    class MyList(Sequence[T]):
        @overload
        def __getitem__(self, index: int) -> T: ...

        @overload
        def __getitem__(self, index: slice) -> Sequence[T]: ...

        def __getitem__(self, index: Union[int, slice]) -> Union[T, Sequence[T]]:
            if isinstance(index, int):
                # Return a T here
            elif isinstance(index, slice):
                # Return a sequence of Ts here
            else:
                raise TypeError(...)
    ```

    !!! info "Note"

        如果您只需要将类型变量限制为某些类型或子类型，则可以使用[`值限制`](https://mypy.readthedocs.io/en/stable/generics.html#type-variable-value-restriction)。

    函数参数的默认值不会影响其签名——只有默认值的缺失或存在才会影响。 因此，为了减少冗余，可以用“...”作为占位符来替换重载定义中的默认值：

    ```python
    from typing import overload

    class M: ...

    @overload
    def get_model(model_or_pk: M, flag: bool = ...) -> M: ...
    @overload
    def get_model(model_or_pk: int, flag: bool = ...) -> M | None: ...

    def get_model(model_or_pk: int | M, flag: bool = True) -> M | None:
        ...
    ```

=== "英文"

    Sometimes the arguments and types in a function depend on each other in ways that can't be captured with a [`Union`](https://docs.python.org/3/library/typing.html#typing.Union). For example, suppose we want to write a function that can accept x-y coordinates. If we pass in just a single x-y coordinate, we return a `ClickEvent` object. However, if we pass in two x-y coordinates, we return a `DragEvent` object.

    Our first attempt at writing this function might look like this:

    ```python
    from typing import Union, Optional

    def mouse_event(x1: int,
                    y1: int,
                    x2: Optional[int] = None,
                    y2: Optional[int] = None) -> Union[ClickEvent, DragEvent]:
        if x2 is None and y2 is None:
            return ClickEvent(x1, y1)
        elif x2 is not None and y2 is not None:
            return DragEvent(x1, y1, x2, y2)
        else:
            raise TypeError("Bad arguments")
    ```

    While this function signature works, it's too loose: it implies `mouse_event` could return either object regardless of the number of arguments we pass in. It also does not prohibit a caller from passing in the wrong number of ints: mypy would treat calls like `mouse_event(1, 2, 20)` as being valid, for example.

    We can do better by using [`overloading`](https://peps.python.org/pep-0484/#function-method-overloading) which lets us give the same function multiple type annotations (signatures) to more accurately describe the function's behavior:

    ```python
    from typing import Union, overload

    # Overload *variants* for 'mouse_event'.
    # These variants give extra information to the type checker.
    # They are ignored at runtime.

    @overload
    def mouse_event(x1: int, y1: int) -> ClickEvent: ...
    @overload
    def mouse_event(x1: int, y1: int, x2: int, y2: int) -> DragEvent: ...

    # The actual *implementation* of 'mouse_event'.
    # The implementation contains the actual runtime logic.
    #
    # It may or may not have type hints. If it does, mypy
    # will check the body of the implementation against the
    # type hints.
    #
    # Mypy will also check and make sure the signature is
    # consistent with the provided variants.

    def mouse_event(x1: int,
                    y1: int,
                    x2: Optional[int] = None,
                    y2: Optional[int] = None) -> Union[ClickEvent, DragEvent]:
        if x2 is None and y2 is None:
            return ClickEvent(x1, y1)
        elif x2 is not None and y2 is not None:
            return DragEvent(x1, y1, x2, y2)
        else:
            raise TypeError("Bad arguments")
    ```

    This allows mypy to understand calls to `mouse_event` much more precisely. For example, mypy will understand that `mouse_event(5, 25)` will always have a return type of `ClickEvent` and will report errors for calls like `mouse_event(5, 25, 2)`.

    As another example, suppose we want to write a custom container class that implements the [`__getitem__`](https://docs.python.org/3/reference/datamodel.html#object.__getitem__) method (`[]` bracket indexing). If this method receives an integer we return a single item. If it receives a `slice`, we return a [`Sequence`](https://docs.python.org/3/library/typing.html#typing.Sequence) of items.

    We can precisely encode this relationship between the argument and the return type by using overloads like so:

    ```python
    from typing import Sequence, TypeVar, Union, overload

    T = TypeVar('T')

    class MyList(Sequence[T]):
        @overload
        def __getitem__(self, index: int) -> T: ...

        @overload
        def __getitem__(self, index: slice) -> Sequence[T]: ...

        def __getitem__(self, index: Union[int, slice]) -> Union[T, Sequence[T]]:
            if isinstance(index, int):
                # Return a T here
            elif isinstance(index, slice):
                # Return a sequence of Ts here
            else:
                raise TypeError(...)
    ```

    !!! info "Note"

        If you just need to constrain a type variable to certain types or subtypes, you can use a [`value restriction`](https://mypy.readthedocs.io/en/stable/generics.html#type-variable-value-restriction).

    The default values of a function's arguments don't affect its signature -- only the absence or presence of a default value does. So in order to reduce redundancy, it's possible to replace default values in overload definitions with `...` as a placeholder:

    ```python
    from typing import overload

    class M: ...

    @overload
    def get_model(model_or_pk: M, flag: bool = ...) -> M: ...
    @overload
    def get_model(model_or_pk: int, flag: bool = ...) -> M | None: ...

    def get_model(model_or_pk: int | M, flag: bool = True) -> M | None:
        ...
    ```

### 运行时行为

Runtime behavior

=== "中文"

    重载函数必须由两个或多个重载*协变*组成，后跟一个*实现*。 协变和实现在代码中必须相邻：将它们视为一个不可分割的单元。

    协变主体必须全部为空； 仅允许实现包含代码。 这是因为在运行时，协变被完全忽略：它们被最终的实现函数覆盖。

    这意味着重载函数仍然是普通的Python函数！ 没有自动调度处理，您必须手动处理实现中的不同类型（例如，通过使用 `if` 语句和 [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) 检查)。

    如果要在存根文件中添加重载，则应省略实现函数：存根不包含运行时逻辑。

    !!! info "Note"

        虽然我们可以使用 “pass” 关键字将协变主体留空，但更常见的约定是使用省略号（“...”）文字。

=== "英文"

    An overloaded function must consist of two or more overload *variants* followed by an *implementation*. The variants and the implementations must be adjacent in the code: think of them as one indivisible unit.

    The variant bodies must all be empty; only the implementation is allowed to contain code. This is because at runtime, the variants are completely ignored: they're overridden by the final implementation function.

    This means that an overloaded function is still an ordinary Python function! There is no automatic dispatch handling and you must manually handle the different types in the implementation (e.g. by using `if` statements and [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) checks).

    If you are adding an overload within a stub file, the implementation function should be omitted: stubs do not contain runtime logic.

    !!! info "Note"

        While we can leave the variant body empty using the `pass` keyword, the more common convention is to instead use the ellipsis (`...`) literal.

### 重载的类型进行调用检查

Type checking calls to overloads

=== "中文"

    当您调用重载函数时，mypy 将在考虑参数类型和数量后，通过选择最佳匹配协变来推断正确的返回类型。 然而，调用永远不会根据实现进行类型检查。 这就是为什么 mypy 会报告像 “mouse_event(5, 25, 3)” 这样的调用无效，即使它与实现签名匹配。

    如果有多个同样好的匹配协变，mypy 将选择最先定义的协变。 例如，考虑以下程序：

    ```python
    # For Python 3.8 and below you must use `typing.List` instead of `list`. e.g.
    # from typing import List
    from typing import overload

    @overload
    def summarize(data: list[int]) -> float: ...

    @overload
    def summarize(data: list[str]) -> str: ...

    def summarize(data):
        if not data:
            return 0.0
        elif isinstance(data[0], int):
            # Do int specific code
        else:
            # Do str-specific code

    # What is the type of 'output'? float or str?
    output = summarize([])
    ```

    `summarize([])` 调用匹配两种协变：空列表可以是 `list[int]` 或 `list[str]`。 在这种情况下，mypy 将通过选择第一个匹配的协变来打破平局：“output” 将具有 “float” 的推断类型。 实现者负责确保 “summarize” 在运行时以相同的方式打破联系。

    但是，“选择第一个匹配项” 规则有两个例外。 首先，如果由于参数类型为“Any”而导致多个协变匹配，则 mypy 将使推断类型也为“Any”：

    ```python
    dynamic_var: Any = some_dynamic_function()

    # output2 is of type 'Any'
    output2 = summarize(dynamic_var)
    ```

    其次，如果由于一个或多个参数是联合而导致多个协变匹配，则 mypy 将使推断类型成为匹配协变返回的并集：

    ```python
    some_list: Union[list[int], list[str]]

    # output3 is of type 'Union[float, str]'
    output3 = summarize(some_list)
    ```

    !!! info "Note"

        由于“选择第一个匹配”规则，更改重载协变的顺序可以更改 mypy 类型检查程序的方式。

        为了尽量减少潜在问题，我们建议您：

        1. 确保您的重载协变以与实现中运行时检查（例如 [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) 检查）相同的顺序列出。
        2. 按照从最具体到最不具体的顺序对协变和运行时检查进行排序。（有关示例，请参阅以下部分）。

=== "英文"

    When you call an overloaded function, mypy will infer the correct return type by picking the best matching variant, after taking into consideration both the argument types and arity. However, a call is never type checked against the implementation. This is why mypy will report calls like `mouse_event(5, 25, 3)` as being invalid even though it matches the implementation signature.

    If there are multiple equally good matching variants, mypy will select the variant that was defined first. For example, consider the following program:

    ```python
    # For Python 3.8 and below you must use `typing.List` instead of `list`. e.g.
    # from typing import List
    from typing import overload

    @overload
    def summarize(data: list[int]) -> float: ...

    @overload
    def summarize(data: list[str]) -> str: ...

    def summarize(data):
        if not data:
            return 0.0
        elif isinstance(data[0], int):
            # Do int specific code
        else:
            # Do str-specific code

    # What is the type of 'output'? float or str?
    output = summarize([])
    ```

    The `summarize([])` call matches both variants: an empty list could be either a `list[int]` or a `list[str]`. In this case, mypy will break the tie by picking the first matching variant: `output` will have an inferred type of `float`. The implementor is responsible for making sure `summarize` breaks ties in the same way at runtime.

    However, there are two exceptions to the "pick the first match" rule. First, if multiple variants match due to an argument being of type `Any`, mypy will make the inferred type also be `Any`:

    ```python
    dynamic_var: Any = some_dynamic_function()

    # output2 is of type 'Any'
    output2 = summarize(dynamic_var)
    ```

    Second, if multiple variants match due to one or more of the arguments being a union, mypy will make the inferred type be the union of the matching variant returns:

    ```python
    some_list: Union[list[int], list[str]]

    # output3 is of type 'Union[float, str]'
    output3 = summarize(some_list)
    ```

    !!! info "Note"

        Due to the "pick the first match" rule, changing the order of your overload variants can change how mypy type checks your program.

        To minimize potential issues, we recommend that you:

        1. Make sure your overload variants are listed in the same order as
        the runtime checks (e.g. [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) checks) in your implementation.
        2. Order your variants and runtime checks from most to least specific.
        (See the following section for an example).

### 协变的类型检查

Type checking the variants

=== "中文"

    Mypy 将对您的重载协变定义执行多次检查，以确保它们的行为符合预期。 首先，mypy 将检查并确保没有重载协变遮盖后续协变。 例如，考虑以下函数，它将两个“Expression”对象加在一起，并包含一个特殊情况来处理接收两个“Literal”类型：

    ```python
    from typing import overload, Union

    class Expression:
        # ...snip...

    class Literal(Expression):
        # ...snip...

    # Warning -- the first overload variant shadows the second!

    @overload
    def add(left: Expression, right: Expression) -> Expression: ...

    @overload
    def add(left: Literal, right: Literal) -> Literal: ...

    def add(left: Expression, right: Expression) -> Expression:
        # ...snip...
    ```

    虽然此代码片段在技术上是类型安全的，但它确实包含反模式：永远不会选择第二个协变！ 如果我们尝试调用“add(Literal(3), Literal(4))”，mypy 将始终选择第一个协变并将函数调用评估为“Expression”类型，而不是“Literal”。 这是因为 `Literal` 是 `Expression` 的子类型，这意味着“选择第一个匹配”规则在考虑第一次重载后将始终停止。

    因为拥有永远无法匹配的重载协变几乎肯定是一个错误，所以 mypy 将报告错误。 要修复该错误，我们可以 1) 删除第二个重载或 2) 交换重载的顺序：

    ```python
    # Everything is ok now -- the variants are correctly ordered
    # from most to least specific.

    @overload
    def add(left: Literal, right: Literal) -> Literal: ...

    @overload
    def add(left: Expression, right: Expression) -> Expression: ...

    def add(left: Expression, right: Expression) -> Expression:
        # ...snip...
    ```

    Mypy 还将对不同的协变进行类型检查，并标记任何具有本质上不安全的重叠协变的重载。 例如，考虑以下不安全重载定义：

    ```python
    from typing import overload, Union

    @overload
    def unsafe_func(x: int) -> int: ...

    @overload
    def unsafe_func(x: object) -> str: ...

    def unsafe_func(x: object) -> Union[int, str]:
        if isinstance(x, int):
            return 42
        else:
            return "some string"
    ```

    从表面上看，这个函数定义似乎没问题。 但是，当我们尝试像这样使用它时，会导致推断类型与实际运行时类型之间存在差异：

    ```python
    some_obj: object = 42
    unsafe_func(some_obj) + " danger danger"  # Type checks, yet crashes at runtime!
    ```

    由于 `some_obj` 的类型为 [`object`](https://docs.python.org/3/library/functions.html#object)，mypy 将决定 `unsafe_func` 必须返回 `str` 类型的内容，并且 总结以上内容将进行类型检查。 但实际上，`unsafe_func`会返回一个int，导致代码在运行时崩溃！

    为了防止此类问题，mypy 将尽最大努力检测并禁止本质上不安全的重叠重载。 当以下两个条件都成立时，两个变体被视为不安全重叠：

    1. 第一个变体的所有参数都与第二个变体兼容。
    2. 第一个变体的返回类型与第二个变体“不”兼容（例如不是其子类型）。

    因此，在此示例中，第一个变体中的 “int” 参数是第二个变体中 “object” 参数的子类型，但 “int” 返回类型不是 “str” 的子类型。 这两个条件都成立，因此 mypy 会正确地将 `unsafe_func` 标记为不安全。

    但是，mypy 不会检测 *所有* 不安全的重载使用。 例如，假设我们修改上面的代码片段，使其调用 “summarize” 而不是 “unsafe_func” ：

    ```python
    some_list: list[str] = []
    summarize(some_list) + "danger danger"  # Type safe, yet crashes at runtime!
    ```

    我们在这里遇到了类似的问题。 该程序类型检查我们是否只查看重载上的注释。 但由于“summarize(...)”被设计为在接收到空列表时偏向于返回浮点数，因此该程序实际上会在运行时崩溃。

    mypy 没有将像“summarize”这样的定义标记为潜在不安全的原因是，如果这样做，编写安全重载将非常困难。 例如，假设我们定义一个具有两个分别接受类型“A”和“B”的变体的重载。 即使这两种类型完全不相关，用户仍然可能通过传入继承自“A”和“B”的第三种类型“C”的值来触发与上述类似的运行时错误。

    值得庆幸的是，此类情况相对较少。 然而，这确实意味着，在设计或使用重载函数时应该小心谨慎，因为该重载函数可能会接收两个看似不相关类型的实例值。

=== "英文"

    Mypy will perform several checks on your overload variant definitions to ensure they behave as expected. First, mypy will check and make sure that no overload variant is shadowing a subsequent one. For example, consider the following function which adds together two `Expression` objects, and contains a special-case to handle receiving two `Literal` types:

    ```python
    from typing import overload, Union

    class Expression:
        # ...snip...

    class Literal(Expression):
        # ...snip...

    # Warning -- the first overload variant shadows the second!

    @overload
    def add(left: Expression, right: Expression) -> Expression: ...

    @overload
    def add(left: Literal, right: Literal) -> Literal: ...

    def add(left: Expression, right: Expression) -> Expression:
        # ...snip...
    ```

    While this code snippet is technically type-safe, it does contain an anti-pattern: the second variant will never be selected! If we try calling `add(Literal(3), Literal(4))`, mypy will always pick the first variant and evaluate the function call to be of type `Expression`, not `Literal`. This is because `Literal` is a subtype of `Expression`, which means the "pick the first match" rule will always halt after considering the first overload.

    Because having an overload variant that can never be matched is almost certainly a mistake, mypy will report an error. To fix the error, we can either 1) delete the second overload or 2) swap the order of the overloads:

    ```python
    # Everything is ok now -- the variants are correctly ordered
    # from most to least specific.

    @overload
    def add(left: Literal, right: Literal) -> Literal: ...

    @overload
    def add(left: Expression, right: Expression) -> Expression: ...

    def add(left: Expression, right: Expression) -> Expression:
        # ...snip...
    ```

    Mypy will also type check the different variants and flag any overloads that have inherently unsafely overlapping variants. For example, consider the following unsafe overload definition:

    ```python
    from typing import overload, Union

    @overload
    def unsafe_func(x: int) -> int: ...

    @overload
    def unsafe_func(x: object) -> str: ...

    def unsafe_func(x: object) -> Union[int, str]:
        if isinstance(x, int):
            return 42
        else:
            return "some string"
    ```

    On the surface, this function definition appears to be fine. However, it will result in a discrepancy between the inferred type and the actual runtime type when we try using it like so:

    ```python
    some_obj: object = 42
    unsafe_func(some_obj) + " danger danger"  # Type checks, yet crashes at runtime!
    ```

    Since `some_obj` is of type [`object`](https://docs.python.org/3/library/functions.html#object), mypy will decide that `unsafe_func` must return something of type `str` and concludes the above will type check. But in reality, `unsafe_func` will return an int, causing the code to crash at runtime!

    To prevent these kinds of issues, mypy will detect and prohibit inherently unsafely overlapping overloads on a best-effort basis. Two variants are considered unsafely overlapping when both of the following are true:

    1. All of the arguments of the first variant are compatible with the second.
    2. The return type of the first variant is *not* compatible with (e.g. is not a subtype of) the second.

    So in this example, the `int` argument in the first variant is a subtype of the `object` argument in the second, yet the `int` return type is not a subtype of `str`. Both conditions are true, so mypy will correctly flag `unsafe_func` as being unsafe.

    However, mypy will not detect *all* unsafe uses of overloads. For example, suppose we modify the above snippet so it calls `summarize` instead of `unsafe_func`:

    ```python
    some_list: list[str] = []
    summarize(some_list) + "danger danger"  # Type safe, yet crashes at runtime!
    ```

    We run into a similar issue here. This program type checks if we look just at the annotations on the overloads. But since `summarize(...)` is designed to be biased towards returning a float when it receives an empty list, this program will actually crash during runtime.

    The reason mypy does not flag definitions like `summarize` as being potentially unsafe is because if it did, it would be extremely difficult to write a safe overload. For example, suppose we define an overload with two variants that accept types `A` and `B` respectively. Even if those two types were completely unrelated, the user could still potentially trigger a runtime error similar to the ones above by passing in a value of some third type `C` that inherits from both `A` and `B`.

    Thankfully, these types of situations are relatively rare. What this does mean, however, is that you should exercise caution when designing or using an overloaded function that can potentially receive values that are an instance of two seemingly unrelated types.

### 类型检查的实现

Type checking the implementation

=== "中文"

    实现的主体根据实现上提供的类型提示进行类型检查。 例如，在上面的 “MyList” 示例中，主体中的代码使用参数列表 “index: Union[int, slice]” 和返回类型 “Union[T, Sequence[T]]” 进行检查。 如果实现上没有注释，则不会对主体进行类型检查。 如果您想强制 mypy 检查主体，请使用 [`--check-untyped-defs`](https://mypy.readthedocs.io/en/stable/command_line.html#cmdoption-mypy-check-untyped-defs) 标志（[`更多详细信息请参见此处`](https://mypy.readthedocs.io/en/stable/command_line.html#untyped-definitions-and-calls)）。

    这些变体还必须与实现类型提示兼容。 在 “MyList” 示例中，mypy 将检查参数类型“int”和返回类型“T”是否与第一个变体的“Union[int, slice]”和“Union[T, Sequence]”兼容。 对于第二个变体，它验证参数类型“slice”和返回类型“Sequence[T]”与“Union[int, slice]”和“Union[T, Sequence]”兼容。

    !!! info "Note"

        上面记录的重载语义是从 mypy 0.620 开始的新语义。

        以前，mypy 用于对所有重载变体执行类型擦除。 例如，上一节中的 “summarize” 示例过去是非法的，因为“list[str]”和“list[int]”都被删除为“list[Any]”。 mypy 0.620 中删除了此限制。

        Mypy 之前还使用不同的算法来选择最佳匹配变体。 如果此算法未能找到匹配项，则默认返回“Any”。 新算法使用“选择第一个匹配”规则，并且仅当输入参数也包含“Any”时才会回退到返回“Any”。

=== "英文"

    The body of an implementation is type-checked against the type hints provided on the implementation. For example, in the `MyList` example up above, the code in the body is checked with argument list `index: Union[int, slice]` and a return type of `Union[T, Sequence[T]]`. If there are no annotations on the implementation, then the body is not type checked. If you want to force mypy to check the body anyways, use the [`--check-untyped-defs`](https://mypy.readthedocs.io/en/stable/command_line.html#cmdoption-mypy-check-untyped-defs) flag ([`more details here`](https://mypy.readthedocs.io/en/stable/command_line.html#untyped-definitions-and-calls)).

    The variants must also also be compatible with the implementation type hints. In the `MyList` example, mypy will check that the parameter type `int` and the return type `T` are compatible with `Union[int, slice]` and `Union[T, Sequence]` for the first variant. For the second variant it verifies the parameter type `slice` and the return type `Sequence[T]` are compatible with `Union[int, slice]` and `Union[T, Sequence]`.

    !!! info "Note"

        The overload semantics documented above are new as of mypy 0.620.

        Previously, mypy used to perform type erasure on all overload variants. For example, the `summarize` example from the previous section used to be illegal because `list[str]` and `list[int]` both erased to just `list[Any]`. This restriction was removed in mypy 0.620.

        Mypy also previously used to select the best matching variant using a different algorithm. If this algorithm failed to find a match, it would default to returning `Any`. The new algorithm uses the "pick the first match" rule and will fall back to returning `Any` only if the input arguments also contain `Any`.

### 条件重载

Conditional overloads

=== "中文"

    有时有条件地定义重载很有用。 常见用例包括运行时不可用的类型或仅存在于特定 Python 版本中的类型。 所有现有的超载规则仍然适用。 例如，必须至少有两个重载。

    !!! info "Note"

        Mypy 只能推断有限数量的条件。 目前支持的包括 [`TYPE_CHECKING`](https://docs.python.org/3/library/typing.html#typing.TYPE_CHECKING), `MYPY`, [`version_and_platform_checks`](https://mypy.readthedocs.io/en/stable/common_issues.html#version-and-platform-checks), [`--always-true`](https://mypy.readthedocs.io/en/stable/command_line.html#cmdoption-mypy-always-true), 和 [`--always-false`](https://mypy.readthedocs.io/en/stable/command_line.html#cmdoption-mypy-always-false) 等.

    ```python
    from typing import TYPE_CHECKING, Any, overload

    if TYPE_CHECKING:
        class A: ...
        class B: ...


    if TYPE_CHECKING:
        @overload
        def func(var: A) -> A: ...

        @overload
        def func(var: B) -> B: ...

    def func(var: Any) -> Any:
        return var

    reveal_type(func(A()))  # Revealed type is "A"
    ```

    ```python
    # flags: --python-version 3.10
    import sys
    from typing import Any, overload

    class A: ...
    class B: ...
    class C: ...
    class D: ...


    if sys.version_info < (3, 7):
        @overload
        def func(var: A) -> A: ...

    elif sys.version_info >= (3, 10):
        @overload
        def func(var: B) -> B: ...

    else:
        @overload
        def func(var: C) -> C: ...

    @overload
    def func(var: D) -> D: ...

    def func(var: Any) -> Any:
        return var


    reveal_type(func(B()))  # Revealed type is "B"
    reveal_type(func(C()))  # No overload variant of "func" matches argument type "C"
        # Possible overload variants:
        #     def func(var: B) -> B
        #     def func(var: D) -> D
        # Revealed type is "Any"
    ```

    !!! info "Note"
        
        在最后一个示例中，mypy 使用 [`--python-version 3.10`](https://mypy.readthedocs.io/en/stable/command_line.html#cmdoption-mypy-python-version) 执行.

        因此，条件“sys.version_info >= (3, 10)”将匹配，并且将添加“B”的重载。
        
        `A` 和 `C` 的重载被忽略！
        
        “D”的重载没有条件定义，因此也被添加。

    当 mypy 无法推断条件始终为“True”或始终为“False”时，会发出错误。

    ```python
    from typing import Any, overload

    class A: ...
    class B: ...


    def g(bool_var: bool) -> None:
        if bool_var:  # Condition can't be inferred, unable to merge overloads
            @overload
            def func(var: A) -> A: ...

            @overload
            def func(var: B) -> B: ...

        def func(var: Any) -> Any: ...

        reveal_type(func(A()))  # Revealed type is "Any"
    ```

=== "英文"

    Sometimes it is useful to define overloads conditionally. Common use cases include types that are unavailable at runtime or that only exist in a certain Python version. All existing overload rules still apply. For example, there must be at least two overloads.

    !!! info "Note"

        Mypy can only infer a limited number of conditions. Supported ones currently include [`TYPE_CHECKING`](https://docs.python.org/3/library/typing.html#typing.TYPE_CHECKING), `MYPY`, [`version_and_platform_checks`](https://mypy.readthedocs.io/en/stable/common_issues.html#version-and-platform-checks), [`--always-true`](https://mypy.readthedocs.io/en/stable/command_line.html#cmdoption-mypy-always-true), and [`--always-false`](https://mypy.readthedocs.io/en/stable/command_line.html#cmdoption-mypy-always-false) values.

    ```python
    from typing import TYPE_CHECKING, Any, overload

    if TYPE_CHECKING:
        class A: ...
        class B: ...


    if TYPE_CHECKING:
        @overload
        def func(var: A) -> A: ...

        @overload
        def func(var: B) -> B: ...

    def func(var: Any) -> Any:
        return var

    reveal_type(func(A()))  # Revealed type is "A"
    ```

    ```python
    # flags: --python-version 3.10
    import sys
    from typing import Any, overload

    class A: ...
    class B: ...
    class C: ...
    class D: ...


    if sys.version_info < (3, 7):
        @overload
        def func(var: A) -> A: ...

    elif sys.version_info >= (3, 10):
        @overload
        def func(var: B) -> B: ...

    else:
        @overload
        def func(var: C) -> C: ...

    @overload
    def func(var: D) -> D: ...

    def func(var: Any) -> Any:
        return var


    reveal_type(func(B()))  # Revealed type is "B"
    reveal_type(func(C()))  # No overload variant of "func" matches argument type "C"
        # Possible overload variants:
        #     def func(var: B) -> B
        #     def func(var: D) -> D
        # Revealed type is "Any"
    ```

    !!! info "Note"
        In the last example, mypy is executed with [`--python-version 3.10`](https://mypy.readthedocs.io/en/stable/command_line.html#cmdoption-mypy-python-version).

        Therefore, the condition `sys.version_info >= (3, 10)` will match and the overload for `B` will be added.
        
        The overloads for `A` and `C` are ignored!
        
        The overload for `D` is not defined conditionally and thus is also added.

    When mypy cannot infer a condition to be always `True` or always `False`, an error is emitted.

    ```python
    from typing import Any, overload

    class A: ...
    class B: ...


    def g(bool_var: bool) -> None:
        if bool_var:  # Condition can't be inferred, unable to merge overloads
            @overload
            def func(var: A) -> A: ...

            @overload
            def func(var: B) -> B: ...

        def func(var: Any) -> Any: ...

        reveal_type(func(A()))  # Revealed type is "Any"
    ```

## self类型的高级用法

Advanced uses of self-types

=== "中文"

    通常，mypy 不需要为实例和类方法的第一个参数添加注释。 然而，对于某些编程模式，它们可能需要具有更精确的静态类型。

=== "英文"

    Normally, mypy doesn't require annotations for the first arguments of instance and class methods. However, they may be needed to have more precise static typing for certain programming patterns.

### 泛型类中的受限方法

Restricted methods in generic classes

=== "中文"

    在泛型类中，某些方法可能只允许为类型参数的某些值调用：

    ```python
    T = TypeVar('T')

    class Tag(Generic[T]):
        item: T
        def uppercase_item(self: Tag[str]) -> str:
            return self.item.upper()

    def label(ti: Tag[int], ts: Tag[str]) -> None:
        ti.uppercase_item()  # E: Invalid self argument "Tag[int]" to attribute function
                            # "uppercase_item" with type "Callable[[Tag[str]], str]"
        ts.uppercase_item()  # This is OK
    ```

    在类型参数本身是泛型的情况下，此模式还允许匹配嵌套类型：

    ```python
    T = TypeVar('T', covariant=True)
    S = TypeVar('S')

    class Storage(Generic[T]):
        def __init__(self, content: T) -> None:
            self.content = content
        def first_chunk(self: Storage[Sequence[S]]) -> S:
            return self.content[0]

    page: Storage[list[str]]
    page.first_chunk()  # OK, type is "str"

    Storage(0).first_chunk()  # Error: Invalid self argument "Storage[int]" to attribute function
                            # "first_chunk" with type "Callable[[Storage[Sequence[S]]], S]"
    ```

    最后，可以使用自身类型的重载来表达一些棘手方法的精确类型：

    ```python
    T = TypeVar('T')

    class Tag(Generic[T]):
        @overload
        def export(self: Tag[str]) -> str: ...
        @overload
        def export(self, converter: Callable[[T], str]) -> str: ...

        def export(self, converter=None):
            if isinstance(self.item, str):
                return self.item
            return converter(self.item)
    ```

    特别是，在自类型上重载的 [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) 方法可能有助于注释类型参数依赖的泛型类构造函数 以不平凡的方式了解构造函数参数，请参见例如 [`Popen`](https://docs.python.org/3/library/subprocess.html#subprocess.Popen)。

=== "英文"

    In generic classes some methods may be allowed to be called only for certain values of type arguments:

    ```python
    T = TypeVar('T')

    class Tag(Generic[T]):
        item: T
        def uppercase_item(self: Tag[str]) -> str:
            return self.item.upper()

    def label(ti: Tag[int], ts: Tag[str]) -> None:
        ti.uppercase_item()  # E: Invalid self argument "Tag[int]" to attribute function
                            # "uppercase_item" with type "Callable[[Tag[str]], str]"
        ts.uppercase_item()  # This is OK
    ```

    This pattern also allows matching on nested types in situations where the type argument is itself generic:

    ```python
    T = TypeVar('T', covariant=True)
    S = TypeVar('S')

    class Storage(Generic[T]):
        def __init__(self, content: T) -> None:
            self.content = content
        def first_chunk(self: Storage[Sequence[S]]) -> S:
            return self.content[0]

    page: Storage[list[str]]
    page.first_chunk()  # OK, type is "str"

    Storage(0).first_chunk()  # Error: Invalid self argument "Storage[int]" to attribute function
                            # "first_chunk" with type "Callable[[Storage[Sequence[S]]], S]"
    ```

    Finally, one can use overloads on self-type to express precise types of some tricky methods:

    ```python
    T = TypeVar('T')

    class Tag(Generic[T]):
        @overload
        def export(self: Tag[str]) -> str: ...
        @overload
        def export(self, converter: Callable[[T], str]) -> str: ...

        def export(self, converter=None):
            if isinstance(self.item, str):
                return self.item
            return converter(self.item)
    ```

    In particular, an [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) method overloaded on self-type may be useful to annotate generic class constructors where type arguments depend on constructor parameters in a non-trivial way, see e.g. [`Popen`](https://docs.python.org/3/library/subprocess.html#subprocess.Popen).

### Mixin 类

Mixin classes

=== "中文"

    在 mixin 方法中使用主机类协议作为自类型可以为 mixin 类的静态类型提供更多的代码可重用性。 例如，可以定义一个协议来定义主机类的通用功能，而不是向每个 mixin 添加所需的抽象方法：

    ```python
    class Lockable(Protocol):
        @property
        def lock(self) -> Lock: ...

    class AtomicCloseMixin:
        def atomic_close(self: Lockable) -> int:
            with self.lock:
                # perform actions

    class AtomicOpenMixin:
        def atomic_open(self: Lockable) -> int:
            with self.lock:
                # perform actions

    class File(AtomicCloseMixin, AtomicOpenMixin):
        def __init__(self) -> None:
            self.lock = Lock()

    class Bad(AtomicCloseMixin):
        pass

    f = File()
    b: Bad
    f.atomic_close()  # OK
    b.atomic_close()  # Error: Invalid self type for "atomic_close"
    ```

    请注意，只要显式自类型不是当前类的超类型，就*必须*成为协议。 在这种情况下，mypy 将仅在调用站点检查自我类型的有效性。

=== "英文"

    Using host class protocol as a self-type in mixin methods allows more code re-usability for static typing of mixin classes. For example, one can define a protocol that defines common functionality for host classes instead of adding required abstract methods to every mixin:

    ```python
    class Lockable(Protocol):
        @property
        def lock(self) -> Lock: ...

    class AtomicCloseMixin:
        def atomic_close(self: Lockable) -> int:
            with self.lock:
                # perform actions

    class AtomicOpenMixin:
        def atomic_open(self: Lockable) -> int:
            with self.lock:
                # perform actions

    class File(AtomicCloseMixin, AtomicOpenMixin):
        def __init__(self) -> None:
            self.lock = Lock()

    class Bad(AtomicCloseMixin):
        pass

    f = File()
    b: Bad
    f.atomic_close()  # OK
    b.atomic_close()  # Error: Invalid self type for "atomic_close"
    ```

    Note that the explicit self-type is *required* to be a protocol whenever it is not a supertype of the current class. In this case mypy will check the validity of the self-type only at the call site.

### 替代构造函数的精确类型

Precise typing of alternative constructors

=== "中文"

    某些类可能定义替代构造函数。 如果这些类是通用的，则自类型允许为它们提供精确的签名：

    ```python
    T = TypeVar('T')

    class Base(Generic[T]):
        Q = TypeVar('Q', bound='Base[T]')

        def __init__(self, item: T) -> None:
            self.item = item

        @classmethod
        def make_pair(cls: Type[Q], item: T) -> tuple[Q, Q]:
            return cls(item), cls(item)

    class Sub(Base[T]):
        ...

    pair = Sub.make_pair('yes')  # Type is "tuple[Sub[str], Sub[str]]"
    bad = Sub[int].make_pair('no')  # Error: Argument 1 to "make_pair" of "Base"
                                    # has incompatible type "str"; expected "int"
    ```

=== "英文"

    Some classes may define alternative constructors. If these
    classes are generic, self-type allows giving them precise signatures:

    ```python
    T = TypeVar('T')

    class Base(Generic[T]):
        Q = TypeVar('Q', bound='Base[T]')

        def __init__(self, item: T) -> None:
            self.item = item

        @classmethod
        def make_pair(cls: Type[Q], item: T) -> tuple[Q, Q]:
            return cls(item), cls(item)

    class Sub(Base[T]):
        ...

    pair = Sub.make_pair('yes')  # Type is "tuple[Sub[str], Sub[str]]"
    bad = Sub[int].make_pair('no')  # Error: Argument 1 to "make_pair" of "Base"
                                    # has incompatible type "str"; expected "int"
    ```

## 异步/同步的类型检查

Typing async/await

=== "中文"

    Mypy 允许您输入使用“async/await”语法的协程。 有关协程的更多信息，请参阅 [`PEP - 492`](https://peps.python.org/pep-0492/) 和 [asyncio 文档](https://docs.python.org/3/library/asyncio.html)。

    使用“async def”定义的函数的类型与普通函数类似。 返回类型注释应该与您希望在“await”协程时返回的值的类型相同。

    ```python
    import asyncio

    async def format_string(tag: str, count: int) -> str:
        return f'T-minus {count} ({tag})'

    async def countdown(tag: str, count: int) -> str:
        while count > 0:
            my_str = await format_string(tag, count)  # type is inferred to be str
            print(my_str)
            await asyncio.sleep(0.1)
            count -= 1
        return "Blastoff!"

    asyncio.run(countdown("Millennium Falcon", 5))
    ```

    调用 `async def` 函数 *无需等待* 的结果将自动推断为 [`Coroutine[Any, Any, T]`](https://docs.python.org/3/library/typing.html#typing.Coroutine) 类型的值 ，它是 [`Awaitable[T]`](https://docs.python.org/3/library/typing.html#typing.Awaitable) 的子类型：

    ```python
    my_coroutine = countdown("Millennium Falcon", 5)
    reveal_type(my_coroutine)  # Revealed type is "typing.Coroutine[Any, Any, builtins.str]"
    ```

=== "英文"

    Mypy lets you type coroutines that use the `async/await` syntax. For more information regarding coroutines, see [`PEP - 492`](https://peps.python.org/pep-0492/) and the [asyncio documentation](https://docs.python.org/3/library/asyncio.html).

    Functions defined using `async def` are typed similar to normal functions. The return type annotation should be the same as the type of the value you expect to get back when `await`-ing the coroutine.

    ```python
    import asyncio

    async def format_string(tag: str, count: int) -> str:
        return f'T-minus {count} ({tag})'

    async def countdown(tag: str, count: int) -> str:
        while count > 0:
            my_str = await format_string(tag, count)  # type is inferred to be str
            print(my_str)
            await asyncio.sleep(0.1)
            count -= 1
        return "Blastoff!"

    asyncio.run(countdown("Millennium Falcon", 5))
    ```

    The result of calling an `async def` function *without awaiting* will automatically be inferred to be a value of type [`Coroutine[Any, Any, T]`](https://docs.python.org/3/library/typing.html#typing.Coroutine), which is a subtype of [`Awaitable[T]`](https://docs.python.org/3/library/typing.html#typing.Awaitable):

    ```python
    my_coroutine = countdown("Millennium Falcon", 5)
    reveal_type(my_coroutine)  # Revealed type is "typing.Coroutine[Any, Any, builtins.str]"
    ```

### 异步迭代器

Asynchronous iterators

=== "中文"

    如果您有异步迭代器，则可以在注释中使用 [`AsyncIterator`](https://docs.python.org/3/library/typing.html#typing.AsyncIterator) 类型：

    ```python
    from typing import Optional, AsyncIterator
    import asyncio

    class arange:
        def __init__(self, start: int, stop: int, step: int) -> None:
            self.start = start
            self.stop = stop
            self.step = step
            self.count = start - step

        def __aiter__(self) -> AsyncIterator[int]:
            return self

        async def __anext__(self) -> int:
            self.count += self.step
            if self.count == self.stop:
                raise StopAsyncIteration
            else:
                return self.count

    async def run_countdown(tag: str, countdown: AsyncIterator[int]) -> str:
        async for i in countdown:
            print(f'T-minus {i} ({tag})')
            await asyncio.sleep(0.1)
        return "Blastoff!"

    asyncio.run(run_countdown("Serenity", arange(5, 0, -1)))
    ```

    异步生成器（在 [`PEP 525`](https://peps.python.org/pep-0525/) 中引入）是创建异步迭代器的简单方法：

    ```python
    from typing import AsyncGenerator, Optional
    import asyncio

    # 也可以输入此作为返回 AsyncIterator[int]
    async def arange(start: int, stop: int, step: int) -> AsyncGenerator[int, None]:
        current = start
        while (step > 0 and current < stop) or (step < 0 and current > stop):
            yield current
            current += step

    asyncio.run(run_countdown("Battlestar Galactica", arange(5, 0, -1)))
    ```

    一个常见的混淆是“async def”函数中“yield”语句的存在会影响函数的类型：

    ```python
    from typing import AsyncIterator

    async def arange(stop: int) -> AsyncIterator[int]:
        # 调用时，arange 为您提供一个异步迭代器，相当于 Callable[[int], AsyncIterator[int]]
        i = 0
        while i < stop:
            yield i
            i += 1

    async def coroutine(stop: int) -> AsyncIterator[int]:
        # 调用时，协程会为您提供一些可以等待以获得异步迭代器的东西，相当于 Callable[[int], Coroutine[Any, Any, AsyncIterator[int]]]
        return arange(stop)

    async def main() -> None:
        reveal_type(arange(5))  # Revealed type is "typing.AsyncIterator[builtins.int]"
        reveal_type(coroutine(5))  # Revealed type is "typing.Coroutine[Any, Any, typing.AsyncIterator[builtins.int]]"

        await arange(5)  # Error: Incompatible types in "await" (actual type "AsyncIterator[int]", expected type "Awaitable[Any]")
        reveal_type(await coroutine(5))  # Revealed type is "typing.AsyncIterator[builtins.int]"
    ```

    当尝试定义基类、协议或重载时，有时会出现这种情况：

    ```python
    from typing import AsyncIterator, Protocol, overload

    class LauncherIncorrect(Protocol):
        # 因为 launch 没有 Yield，所以它的类型为 Callable[[], Coroutine[Any, Any, AsyncIterator[int]]] 而不是 Callable[[], AsyncIterator[int]]
        async def launch(self) -> AsyncIterator[int]:
            raise NotImplementedError

    class LauncherCorrect(Protocol):
        def launch(self) -> AsyncIterator[int]:
            raise NotImplementedError

    class LauncherAlsoCorrect(Protocol):
        async def launch(self) -> AsyncIterator[int]:
            raise NotImplementedError
            if False:
                yield 0

    # 重载的类型与实现无关。
    # 特别是，它们的类型不受实现是否包含“yield”的影响。
    # 使用 `def` 可以清楚地表明类型是 Callable[..., AsyncIterator[int]]，
    # 而使用 `async def` 它将是 Callable[..., Coroutine[Any, Any, AsyncIterator[int]]]
    @overload
    def launch(*, count: int = ...) -> AsyncIterator[int]: ...
    @overload
    def launch(*, time: float = ...) -> AsyncIterator[int]: ...

    async def launch(*, count: int = 0, time: float = 0) -> AsyncIterator[int]:
        # launch 的实现是一个异步生成器并包含一个yield
        yield 0
    ```

=== "英文"

    If you have an asynchronous iterator, you can use the [`AsyncIterator`](https://docs.python.org/3/library/typing.html#typing.AsyncIterator) type in your annotations:

    ```python
    from typing import Optional, AsyncIterator
    import asyncio

    class arange:
        def __init__(self, start: int, stop: int, step: int) -> None:
            self.start = start
            self.stop = stop
            self.step = step
            self.count = start - step

        def __aiter__(self) -> AsyncIterator[int]:
            return self

        async def __anext__(self) -> int:
            self.count += self.step
            if self.count == self.stop:
                raise StopAsyncIteration
            else:
                return self.count

    async def run_countdown(tag: str, countdown: AsyncIterator[int]) -> str:
        async for i in countdown:
            print(f'T-minus {i} ({tag})')
            await asyncio.sleep(0.1)
        return "Blastoff!"

    asyncio.run(run_countdown("Serenity", arange(5, 0, -1)))
    ```

    Async generators (introduced in [`PEP 525`](https://peps.python.org/pep-0525/)) are an easy way to create async iterators:

    ```python
    from typing import AsyncGenerator, Optional
    import asyncio

    # Could also type this as returning AsyncIterator[int]
    async def arange(start: int, stop: int, step: int) -> AsyncGenerator[int, None]:
        current = start
        while (step > 0 and current < stop) or (step < 0 and current > stop):
            yield current
            current += step

    asyncio.run(run_countdown("Battlestar Galactica", arange(5, 0, -1)))
    ```

    One common confusion is that the presence of a `yield` statement in an
    `async def` function has an effect on the type of the function:

    ```python
    from typing import AsyncIterator

    async def arange(stop: int) -> AsyncIterator[int]:
        # When called, arange gives you an async iterator
        # Equivalent to Callable[[int], AsyncIterator[int]]
        i = 0
        while i < stop:
            yield i
            i += 1

    async def coroutine(stop: int) -> AsyncIterator[int]:
        # When called, coroutine gives you something you can await to get an async iterator
        # Equivalent to Callable[[int], Coroutine[Any, Any, AsyncIterator[int]]]
        return arange(stop)

    async def main() -> None:
        reveal_type(arange(5))  # Revealed type is "typing.AsyncIterator[builtins.int]"
        reveal_type(coroutine(5))  # Revealed type is "typing.Coroutine[Any, Any, typing.AsyncIterator[builtins.int]]"

        await arange(5)  # Error: Incompatible types in "await" (actual type "AsyncIterator[int]", expected type "Awaitable[Any]")
        reveal_type(await coroutine(5))  # Revealed type is "typing.AsyncIterator[builtins.int]"
    ```

    This can sometimes come up when trying to define base classes, Protocols or overloads:

    ```python
    from typing import AsyncIterator, Protocol, overload

    class LauncherIncorrect(Protocol):
        # Because launch does not have yield, this has type
        # Callable[[], Coroutine[Any, Any, AsyncIterator[int]]]
        # instead of
        # Callable[[], AsyncIterator[int]]
        async def launch(self) -> AsyncIterator[int]:
            raise NotImplementedError

    class LauncherCorrect(Protocol):
        def launch(self) -> AsyncIterator[int]:
            raise NotImplementedError

    class LauncherAlsoCorrect(Protocol):
        async def launch(self) -> AsyncIterator[int]:
            raise NotImplementedError
            if False:
                yield 0

    # The type of the overloads is independent of the implementation.
    # In particular, their type is not affected by whether or not the
    # implementation contains a `yield`.
    # Use of `def`` makes it clear the type is Callable[..., AsyncIterator[int]],
    # whereas with `async def` it would be Callable[..., Coroutine[Any, Any, AsyncIterator[int]]]
    @overload
    def launch(*, count: int = ...) -> AsyncIterator[int]: ...
    @overload
    def launch(*, time: float = ...) -> AsyncIterator[int]: ...

    async def launch(*, count: int = 0, time: float = 0) -> AsyncIterator[int]:
        # The implementation of launch is an async generator and contains a yield
        yield 0
    ```
