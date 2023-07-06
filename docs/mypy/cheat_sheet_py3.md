# 类型提示备忘录

**Type hints cheat sheet**

转自: <https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html>

## 变量

=== "中文"

    从技术上讲，下面显示的许多类型注解都是多余的，因为 mypy 通常可以从变量的值推断出变量的类型。 有关更多详细信息，请参阅[类型推断和类型注解](https://mypy.readthedocs.io/en/stable/type_inference_and_annotations.html#type-inference-and-annotations)。

    ```python
    # 这是声明变量类型的方式
    age: int = 1
    
    # 您不需要初始化变量来注解它
    a: int  # Ok (在分配之前在运行时没有值)
    
    # 这样做在条件分支中很有用
    child: bool
    if age < 18:
        child = True
    else:
        child = False
    ```

=== "原文"

    **Variables**

    Technically many of the type annotations shown below are redundant, since mypy can usually infer the type of a variable from its value. See Type inference and type annotations for more details.

    ```python
    # This is how you declare the type of a variable
    age: int = 1
    
    # You don't need to initialize a variable to annotate it
    a: int  # Ok (no value at runtime until assigned)
    
    # Doing so can be useful in conditional branches
    child: bool
    if age < 18:
        child = True
    else:
        child = False
    ```

## 有用的内置类型

=== "中文"

    ```python
    # 对于大多数类型，只需在注解中使用类型的名称, 请注意，mypy 通常可以从变量的值推断变量的类型，因此从技术上讲，这些注解是多余的
    x: int = 1
    x: float = 1.0
    x: bool = True
    x: str = "test"
    x: bytes = b"test"
    
    # 对于 Python 3.9+ 上的集合，集合项的类型位于括号中
    x: list[int] = [1]
    x: set[int] = {6, 7}
    
    # 对于映射，我们需要键和值的类型
    x: dict[str, float] = {"field": 2.0}  # Python 3.9+
    
    # 对于固定大小的元组，我们指定所有元素的类型
    x: tuple[int, str, float] = (3, "yes", 7.5)  # Python 3.9+
    
    # 对于可变大小的元组，我们使用一种类型和省略号
    x: tuple[int, ...] = (1, 2, 3)  # Python 3.9+
    
    # 在 Python 3.8 及更早版本中，集合类型的名称大写，并且类型是从“typing”模块导入的
    from typing import List, Set, Dict, Tuple
    x: List[int] = [1]
    x: Set[int] = {6, 7}
    x: Dict[str, float] = {"field": 2.0}
    x: Tuple[int, str, float] = (3, "yes", 7.5)
    x: Tuple[int, ...] = (1, 2, 3)
    
    from typing import Union, Optional
    
    # 在 Python 3.10+ 上，使用 | 当某值可能是几种类型之一时的运算符
    x: list[int | str] = [3, 5, "test", "fun"]  # Python 3.10+
    # 在早期版本中，使用 Union
    x: list[Union[int, str]] = [3, 5, "test", "fun"]
    
    # 使用 Optional[X] 作为可能为None的值
    # Optional[X] 与 X | None 相同 或 Union[X，None]
    x: Optional[str] = "something" if some_condition() else None
    if x is not None:
        # 由于 if 语句，Mypy 知道 x 不会在这里为 None
        print(x.upper())
    # 如果您知道由于 mypy 不理解的某些逻辑，某个值永远不可能为 None，请使用断言
    assert x is not None
    print(x.upper())
    ```

=== "原文"

    **Useful built-in types**

    ```python
    # For most types, just use the name of the type in the annotation
    # Note that mypy can usually infer the type of a variable from its value,
    # so technically these annotations are redundant
    x: int = 1
    x: float = 1.0
    x: bool = True
    x: str = "test"
    x: bytes = b"test"
    
    # For collections on Python 3.9+, the type of the collection item is in brackets
    x: list[int] = [1]
    x: set[int] = {6, 7}
    
    # For mappings, we need the types of both keys and values
    x: dict[str, float] = {"field": 2.0}  # Python 3.9+
    
    # For tuples of fixed size, we specify the types of all the elements
    x: tuple[int, str, float] = (3, "yes", 7.5)  # Python 3.9+
    
    # For tuples of variable size, we use one type and ellipsis
    x: tuple[int, ...] = (1, 2, 3)  # Python 3.9+
    
    # On Python 3.8 and earlier, the name of the collection type is
    # capitalized, and the type is imported from the 'typing' module
    from typing import List, Set, Dict, Tuple
    x: List[int] = [1]
    x: Set[int] = {6, 7}
    x: Dict[str, float] = {"field": 2.0}
    x: Tuple[int, str, float] = (3, "yes", 7.5)
    x: Tuple[int, ...] = (1, 2, 3)
    
    from typing import Union, Optional
    
    # On Python 3.10+, use the | operator when something could be one of a few types
    x: list[int | str] = [3, 5, "test", "fun"]  # Python 3.10+
    # On earlier versions, use Union
    x: list[Union[int, str]] = [3, 5, "test", "fun"]
    
    # Use Optional[X] for a value that could be None
    # Optional[X] is the same as X | None or Union[X, None]
    x: Optional[str] = "something" if some_condition() else None
    if x is not None:
        # Mypy understands x won't be None here because of the if-statement
        print(x.upper())
    # If you know a value can never be None due to some logic that mypy doesn't
    # understand, use an assert
    assert x is not None
    print(x.upper())
    ```

## 函数

=== "中文"

    ```python
    from typing import Callable, Iterator, Union, Optional
    
    # 这是注解函数定义的方式
    def stringify(num: int) -> str:
        return str(num)
    
    # 以下是指定多个参数的方法
    def plus(num1: int, num2: int) -> int:
        return num1 + num2
    
    # 如果函数没有返回值，则使用 None 作为返回类型
    # 参数的默认值位于类型注解之后
    def show(value: str, excitement: int = 10) -> None:
        print(value + "!" * excitement)
    
    # 请注意，没有类型的参数是动态类型的（视为 Any）
    # 并且该函数没有任何未检查的注解
    def untyped(x):
        x.anything() + 1 + "string"  # no errors

    # 这是注解可调用（函数）值的方式
    x: Callable[[int, float], float] = f
    def register(callback: Callable[[str], int]) -> None: ...
    
    # 生成整数的生成器函数实际上只是一个函数
    # 返回一个整数迭代器，这就是我们注解它的方式
    def gen(n: int) -> Iterator[int]:
        i = 0
        while i < n:
            yield i
            i += 1
    
    # 您当然可以将函数注解拆分为多行
    def send_email(address: Union[str, list[str]],
                   sender: str,
                   cc: Optional[list[str]],
                   bcc: Optional[list[str]],
                   subject: str = '',
                   body: Optional[list[str]] = None
                   ) -> bool:
        ...
    
    # Mypy 能够理解仅位置参数和仅关键字参数
    # 也可以使用以两个下划线开头的名称来标记仅位置参数
    def quux(x: int, /, *, y: int) -> None:
        pass
    
    quux(3, y=5)  # Ok
    quux(3, 5)  # error: “quux”的位置参数太多
    quux(x=3, y=5)  # error: “quux”的意外关键字参数“x”
    
    # 这表示每个位置参数和每个关键字参数都是一个“str”
    def call(self, *args: str, **kwargs: str) -> str:
        reveal_type(args)  # 揭示的类型是 "tuple[str, ...]"
        reveal_type(kwargs)  # 揭示的类型是 "dict[str, str]"
        request = make_request(*args, **kwargs)
        return self.do_api_query(request)
    ```

=== "原文"

    **Functions**

    ```python
    from typing import Callable, Iterator, Union, Optional
    
    # This is how you annotate a function definition
    def stringify(num: int) -> str:
        return str(num)
    
    # And here's how you specify multiple arguments
    def plus(num1: int, num2: int) -> int:
        return num1 + num2
    
    # If a function does not return a value, use None as the return type
    # Default value for an argument goes after the type annotation
    def show(value: str, excitement: int = 10) -> None:
        print(value + "!" * excitement)
    
    # Note that arguments without a type are dynamically typed (treated as Any)
    # and that functions without any annotations not checked
    def untyped(x):
        x.anything() + 1 + "string"  # no errors
    
    # This is how you annotate a callable (function) value
    x: Callable[[int, float], float] = f
    def register(callback: Callable[[str], int]) -> None: ...
    
    # A generator function that yields ints is secretly just a function that
    # returns an iterator of ints, so that's how we annotate it
    def gen(n: int) -> Iterator[int]:
        i = 0
        while i < n:
            yield i
            i += 1
    
    # You can of course split a function annotation over multiple lines
    def send_email(address: Union[str, list[str]],
                   sender: str,
                   cc: Optional[list[str]],
                   bcc: Optional[list[str]],
                   subject: str = '',
                   body: Optional[list[str]] = None
                   ) -> bool:
        ...
    
    # Mypy understands positional-only and keyword-only arguments
    # Positional-only arguments can also be marked by using a name starting with
    # two underscores
    def quux(x: int, /, *, y: int) -> None:
        pass
    
    quux(3, y=5)  # Ok
    quux(3, 5)  # error: Too many positional arguments for "quux"
    quux(x=3, y=5)  # error: Unexpected keyword argument "x" for "quux"
    
    # This says each positional arg and each keyword arg is a "str"
    def call(self, *args: str, **kwargs: str) -> str:
        reveal_type(args)  # Revealed type is "tuple[str, ...]"
        reveal_type(kwargs)  # Revealed type is "dict[str, str]"
        request = make_request(*args, **kwargs)
        return self.do_api_query(request)
    ```

## 类

=== "中文"

    ```python
    class BankAccount:
        # “__init__”方法不返回任何内容，因此它的返回类型为“None”，就像任何其他不返回任何内容的方法一样
        def __init__(self, account_name: str, initial_balance: int = 0) -> None:
            # mypy 将根据参数的类型推断这些实例变量的正确类型。
            self.account_name = account_name
            self.balance = initial_balance
    
        # 对于实例方法，省略“self”的类型
        def deposit(self, amount: int) -> None:
            self.balance += amount
    
        def withdraw(self, amount: int) -> None:
            self.balance -= amount
    
    # 用户定义的类作为注解中的类型有效
    account: BankAccount = BankAccount("Alice", 400)
    def transfer(src: BankAccount, dst: BankAccount, amount: int) -> None:
        src.withdraw(amount)
        dst.deposit(amount)
    
    # 接受 BankAccount 的函数也接受 BankAccount 的任何子类！
    class AuditedBankAccount(BankAccount):
        # 您可以选择在类主体中声明实例变量
        audit_log: list[str]
        # 这是一个具有默认值的实例变量
        auditor_name: str = "The Spanish Inquisition"
    
        def __init__(self, account_name: str, initial_balance: int = 0) -> None:
            super().__init__(account_name, initial_balance)
            self.audit_log: list[str] = []
    
        def deposit(self, amount: int) -> None:
            self.audit_log.append(f"Deposited {amount}")
            self.balance += amount
    
        def withdraw(self, amount: int) -> None:
            self.audit_log.append(f"Withdrew {amount}")
            self.balance -= amount
    
    audited = AuditedBankAccount("Bob", 300)
    transfer(audited, account, 100)  # 类型检查!
    
    # 可以使用ClassVar注解来声明类变量
    class Car:
        seats: ClassVar[int] = 4
        passengers: ClassVar[list[str]]
    
    # 如果您想要类上的动态属性，请让它覆盖“__setattr__”或“__getattr__”
    class A:
        # 如果 x 与“value”的类型相同，则这将允许分配给任何 A.x
        # （使用“value: Any”允许任意类型）
        def __setattr__(self, name: str, value: int) -> None: ...
    
        # 如果 x 与返回类型兼容，这将允许访问任何 A.x
        def __getattr__(self, name: str) -> int: ...
    
    a.foo = 42  # OK
    a.bar = 'Ex-parrot'  # 类型检查失败
    ```

=== "原文"

    **Classes**

    ```python
    class BankAccount:
        # The "__init__" method doesn't return anything, so it gets return
        # type "None" just like any other method that doesn't return anything
        def __init__(self, account_name: str, initial_balance: int = 0) -> None:
            # mypy will infer the correct types for these instance variables
            # based on the types of the parameters.
            self.account_name = account_name
            self.balance = initial_balance
    
        # For instance methods, omit type for "self"
        def deposit(self, amount: int) -> None:
            self.balance += amount
    
        def withdraw(self, amount: int) -> None:
            self.balance -= amount
    
    # User-defined classes are valid as types in annotations
    account: BankAccount = BankAccount("Alice", 400)
    def transfer(src: BankAccount, dst: BankAccount, amount: int) -> None:
        src.withdraw(amount)
        dst.deposit(amount)
    
    # Functions that accept BankAccount also accept any subclass of BankAccount!
    class AuditedBankAccount(BankAccount):
        # You can optionally declare instance variables in the class body
        audit_log: list[str]
        # This is an instance variable with a default value
        auditor_name: str = "The Spanish Inquisition"
    
        def __init__(self, account_name: str, initial_balance: int = 0) -> None:
            super().__init__(account_name, initial_balance)
            self.audit_log: list[str] = []
    
        def deposit(self, amount: int) -> None:
            self.audit_log.append(f"Deposited {amount}")
            self.balance += amount
    
        def withdraw(self, amount: int) -> None:
            self.audit_log.append(f"Withdrew {amount}")
            self.balance -= amount
    
    audited = AuditedBankAccount("Bob", 300)
    transfer(audited, account, 100)  # type checks!
    
    # You can use the ClassVar annotation to declare a class variable
    class Car:
        seats: ClassVar[int] = 4
        passengers: ClassVar[list[str]]
    
    # If you want dynamic attributes on your class, have it
    # override "__setattr__" or "__getattr__"
    class A:
        # This will allow assignment to any A.x, if x is the same type as "value"
        # (use "value: Any" to allow arbitrary types)
        def __setattr__(self, name: str, value: int) -> None: ...
    
        # This will allow access to any A.x, if x is compatible with the return type
        def __getattr__(self, name: str) -> int: ...
    
    a.foo = 42  # Works
    a.bar = 'Ex-parrot'  # Fails type checking
    ```

## 当你感到困惑或事情变得复杂时

=== "中文"

    ```python
    from typing import Union, Any, Optional, TYPE_CHECKING, cast
    
    # 要找出 mypy 为程序中任何位置的表达式推断出什么类型，请将其包装在 Reveal_type() 中。 Mypy 将打印一条带有类型的错误消息； 在运行代码之前再次删除它。
    reveal_type(1)  # 揭示的类型是 "builtins.int"
    
    # 如果您使用空容器或“None”初始化变量，您可能需要通过提供显式类型注解来帮助 mypy
    x: list[str] = []
    x: Optional[str] = None
    
    # 如果您不知道某事物的类型或者它太动态而无法为其编写类型，请使用 Any
    x: Any = mystery_function()
    # Mypy 会让你用 x 做任何事情！
    x.whatever() * x["you"] + x("want") - any(x) and all(x) is super  # no errors

    # 当您的代码混淆 mypy 或在 mypy 中遇到彻底的错误时，使用“type:ignore”注解来抑制给定行上的错误。
    # 好的做法是添加解释问题的评论。
    x = confusing_function()  # type: ignore  # 混乱的函数不会在这里返回 None 因为......
    
    # “cast”是一个辅助函数，可让您覆盖表达式的推断类型。 它仅适用于 mypy——没有运行时检查。
    a = [4]
    b = cast(list[int], a)  # 通过良好
    c = cast(list[str], a)  # 尽管是谎言，但仍通过良好（没有运行时检查）
    reveal_type(c)  # 揭示的类型是 "builtins.list[builtins.str]"
    print(c)  # 仍然打印 [4] ...该对象在运行时未更改或转换
    
    # 如果您想要 mypy 可以看到但不会在运行时执行的代码（或者想要 mypy 看不到的代码），请使用“TYPE_CHECKING”
    if TYPE_CHECKING:
        import json
    else:
        import orjson as json  # mypy 不知道这一点
    ```
    
    在某些情况下，类型注解可能会在运行时导致问题，请参阅[运行时注解问题](https://mypy.readthedocs.io/en/stable/runtime_troubles.html#runtime-troubles)来处理此问题。
    
    有关如何消除错误的详细信息，请参阅[消除类型错误](https://mypy.readthedocs.io/en/stable/type_inference_and_annotations.html#silencing-type-errors)。

=== "原文"

    **When you’re puzzled or when things are complicated**

    ```python
    from typing import Union, Any, Optional, TYPE_CHECKING, cast
    
    # To find out what type mypy infers for an expression anywhere in
    # your program, wrap it in reveal_type().  Mypy will print an error
    # message with the type; remove it again before running the code.
    reveal_type(1)  # Revealed type is "builtins.int"
    
    # If you initialize a variable with an empty container or "None"
    # you may have to help mypy a bit by providing an explicit type annotation
    x: list[str] = []
    x: Optional[str] = None
    
    # Use Any if you don't know the type of something or it's too
    # dynamic to write a type for
    x: Any = mystery_function()
    # Mypy will let you do anything with x!
    x.whatever() * x["you"] + x("want") - any(x) and all(x) is super  # no errors
    
    # Use a "type: ignore" comment to suppress errors on a given line,
    # when your code confuses mypy or runs into an outright bug in mypy.
    # Good practice is to add a comment explaining the issue.
    x = confusing_function()  # type: ignore  # confusing_function won't return None here because ...
    
    # "cast" is a helper function that lets you override the inferred
    # type of an expression. It's only for mypy -- there's no runtime check.
    a = [4]
    b = cast(list[int], a)  # Passes fine
    c = cast(list[str], a)  # Passes fine despite being a lie (no runtime check)
    reveal_type(c)  # Revealed type is "builtins.list[builtins.str]"
    print(c)  # Still prints [4] ... the object is not changed or casted at runtime
    
    # Use "TYPE_CHECKING" if you want to have code that mypy can see but will not
    # be executed at runtime (or to have code that mypy can't see)
    if TYPE_CHECKING:
        import json
    else:
        import orjson as json  # mypy is unaware of this
    ```
    
    In some cases type annotations can cause issues at runtime, see [Annotation issues at runtime](https://mypy.readthedocs.io/en/stable/runtime_troubles.html#runtime-troubles) for dealing with this.
    
    See [Silencing type errors](https://mypy.readthedocs.io/en/stable/type_inference_and_annotations.html#silencing-type-errors) for details on how to silence errors.

## 标准“鸭子类型”

=== "中文"

    在典型的 Python 代码中，许多可以将列表或字典作为参数的函数只需要它们的参数在某种程度上“类似于列表”或“类似于字典”。 “类似列表”或“类似字典”（或类似其他东西）的特定含义称为“鸭子类型”，并且在惯用的Python中常见的几种鸭子类型已标准化。

    ```python
    from typing import Mapping, MutableMapping, Sequence, Iterable
    
    # 将 Iterable 用于通用可迭代对象（“for”中可用的任何内容），并在需要序列（支持“len”和“__getitem__”）时使用 Sequence
    def f(ints: Iterable[int]) -> list[str]:
        return [str(x) for x in ints]
    
    f(range(1, 3))
    
    # Mapping 描述了一个我们不会改变的类似 dict 的对象（带有“__getitem__”），而 MutableMapping 则描述了一个我们可能会改变的对象（带有“__setitem__”）
    def f(my_mapping: Mapping[int, str]) -> list[int]:
        my_mapping[5] = 'maybe'  # mypy 会抱怨这行......
        return list(my_mapping.keys())
    
    f({3: 'yes', 4: 'no'})
    
    def f(my_mapping: MutableMapping[int, str]) -> set[str]:
        my_mapping[5] = 'maybe'  # ...但 mypy 对此表示同意。
        return set(my_mapping.values())
    
    f({3: 'yes', 4: 'no'})
    
    import sys
    from typing import IO
    
    # 对于应该接受或返回来自 open() 调用的对象的函数，请使用 IO[str] 或 IO[bytes]（请注意，IO 不区分读、写或其他模式）
    def get_sys_IO(mode: str = 'w') -> IO[str]:
        if mode == 'w':
            return sys.stdout
        elif mode == 'r':
            return sys.stdin
        else:
            return sys.stdout
    ```
    
    您甚至可以使用[协议和结构子类型](https://mypy.readthedocs.io/en/stable/protocols.html#protocol-types)创建自己的鸭子类型。

=== "原文"

    **Standard “duck types”**

    In typical Python code, many functions that can take a list or a dict as an argument only need their argument to be somehow “list-like” or “dict-like”. A specific meaning of “list-like” or “dict-like” (or something-else-like) is called a “duck type”, and several duck types that are common in idiomatic Python are standardized.

    ```python
    from typing import Mapping, MutableMapping, Sequence, Iterable
    
    # Use Iterable for generic iterables (anything usable in "for"),
    # and Sequence where a sequence (supporting "len" and "__getitem__") is
    # required
    def f(ints: Iterable[int]) -> list[str]:
        return [str(x) for x in ints]
    
    f(range(1, 3))
    
    # Mapping describes a dict-like object (with "__getitem__") that we won't
    # mutate, and MutableMapping one (with "__setitem__") that we might
    def f(my_mapping: Mapping[int, str]) -> list[int]:
        my_mapping[5] = 'maybe'  # mypy will complain about this line...
        return list(my_mapping.keys())
    
    f({3: 'yes', 4: 'no'})
    
    def f(my_mapping: MutableMapping[int, str]) -> set[str]:
        my_mapping[5] = 'maybe'  # ...but mypy is OK with this.
        return set(my_mapping.values())
    
    f({3: 'yes', 4: 'no'})
    
    import sys
    from typing import IO
    
    # Use IO[str] or IO[bytes] for functions that should accept or return
    # objects that come from an open() call (note that IO does not
    # distinguish between reading, writing or other modes)
    def get_sys_IO(mode: str = 'w') -> IO[str]:
        if mode == 'w':
            return sys.stdout
        elif mode == 'r':
            return sys.stdin
        else:
            return sys.stdout
    ```
    
    You can even make your own duck types using [Protocols and structural subtyping](https://mypy.readthedocs.io/en/stable/protocols.html#protocol-types).

## 前置引用（forward reference）

=== "中文"

    ```python
    # 您可能想在定义类之前引用它。这称为“前置类型（forward reference）”。
    def f(foo: A) -> int:  # 这将在运行时失败，因为“A”未定义
        ...
    
    # 但是，如果添加以下特殊导入：
    from __future__ import annotations
    # 它将在运行时工作，并且只要文件中稍后存在该名称的类，类型检查就会成功
    def f(foo: A) -> int:  # Ok
        ...
    
    # 另一种选择是将类型放在引号中
    def f(foo: 'A') -> int:  # Also ok
        ...
    
    class A:
        # 如果您需要在该类定义内的类型注解中引用该类，也会出现这种情况
        @classmethod
        def create(cls) -> A:
            ...
    ```

    有关更多详细信息，请参阅[类名前置引用](https://mypy.readthedocs.io/en/stable/runtime_troubles.html#forward-references)。

=== "原文"

    **Forward references**

    ```python
    # You may want to reference a class before it is defined.
    # This is known as a "forward reference".
    def f(foo: A) -> int:  # This will fail at runtime with 'A' is not defined
        ...
    
    # However, if you add the following special import:
    from __future__ import annotations
    # It will work at runtime and type checking will succeed as long as there
    # is a class of that name later on in the file
    def f(foo: A) -> int:  # Ok
        ...
    
    # Another option is to just put the type in quotes
    def f(foo: 'A') -> int:  # Also ok
        ...
    
    class A:
        # This can also come up if you need to reference a class in a type
        # annotation inside the definition of that class
        @classmethod
        def create(cls) -> A:
            ...
    ```

    See [Class name forward references](https://mypy.readthedocs.io/en/stable/runtime_troubles.html#forward-references) for more details.

## 装饰器

=== "中文"

    装饰器函数可以通过泛型来表达。 有关更多详细信息，请参阅[声明装饰器](https://mypy.readthedocs.io/en/stable/generics.html#declaring-decorators)。

    ```python
    from typing import Any, Callable, TypeVar

    F = TypeVar('F', bound=Callable[..., Any])
    
    def bare_decorator(func: F) -> F:
        ...
    
    def decorator_args(url: str) -> Callable[[F], F]:
        ...
    ```

=== "原文"

    **Decorators**

    Decorator functions can be expressed via generics. See [Declaring decorators](https://mypy.readthedocs.io/en/stable/generics.html#declaring-decorators) for more details.

    ```python
    from typing import Any, Callable, TypeVar

    F = TypeVar('F', bound=Callable[..., Any])
    
    def bare_decorator(func: F) -> F:
        ...
    
    def decorator_args(url: str) -> Callable[[F], F]:
        ...
    ```

## 协程和 asyncio

=== "中文"

    有关编写协程和异步代码的完整详细信息，请参阅[注解 async/await](https://mypy.readthedocs.io/en/stable/more_types.html#async-and-await)。

    ```python
    import asyncio

    # 协程的类型与普通函数类似
    
    async def countdown(tag: str, count: int) -> str:
        while count > 0:
            print(f'T-minus {count} ({tag})')
            await asyncio.sleep(0.1)
            count -= 1
        return "Blastoff!"
    ```

=== "原文"

    **Coroutines and asyncio**

    See [Typing async/await](https://mypy.readthedocs.io/en/stable/more_types.html#async-and-await) for the full detail on typing coroutines and asynchronous code.

    ```python
    import asyncio

    # A coroutine is typed like a normal function
    
    async def countdown(tag: str, count: int) -> str:
        while count > 0:
            print(f'T-minus {count} ({tag})')
            await asyncio.sleep(0.1)
            count -= 1
        return "Blastoff!"
    ```
