# 协议和结构子类型

=== "中文"

    Python 类型系统支持两种确定两个对象作为类型是否兼容的方法：名义子类型和结构子类型。
    
    *名义上的*子类型严格基于类层次结构。 如果类“Dog”继承类“Animal”，那么它是“Animal”的子类型。 当需要“Animal”实例时，可以使用“Dog”实例。 这种形式的子类型化是 Python 类型系统主要使用的：它很容易理解并生成清晰简洁的错误消息，并且与本机 {py:func}[`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) 检查的工作方式相匹配 - 基于类层次结构。
    
    *结构*子类型基于可以对对象执行的操作。 如果类“Dog”具有类“Animal”的所有属性和方法，并且具有兼容的类型，则类“Dog”是类“Animal”的结构子类型。
    
    结构子类型可以看作是鸭子类型的静态等价物，这是 Python 程序员所熟知的。 有关 Python 中协议和结构子类型的详细规范，请参阅 [`PEP 544`](https://peps.python.org/pep-0544/)。

=== "英文"

    **Protocols and structural subtyping**

    The Python type system supports two ways of deciding whether two objects are compatible as types: nominal subtyping and structural subtyping.

    *Nominal* subtyping is strictly based on the class hierarchy. If class `Dog` inherits class `Animal`, it's a subtype of `Animal`. Instances of `Dog` can be used when `Animal` instances are expected. This form of subtyping subtyping is what Python's type system predominantly uses: it's easy to understand and produces clear and concise error messages, and matches how the native [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) check works -- based on class hierarchy.

    *Structural* subtyping is based on the operations that can be performed with an object. Class `Dog` is a structural subtype of class `Animal` if the former has all attributes and methods of the latter, and with compatible types.

    Structural subtyping can be seen as a static equivalent of duck typing, which is well known to Python programmers. See [`PEP 544`](https://peps.python.org/pep-0544/) for the detailed specification of protocols and structural subtyping in Python.

## 预定义协议

=== "中文"

    [`typing`](https://docs.python.org/3/library/typing.html#module-typing) 模块定义了与常见 Python 协议相对应的各种协议类，例如 [`Iterable[T]`](https://docs.python.org/3/library/typing.html#typing.Iterable)。 如果一个类定义了合适的 [`__iter__`](https://docs.python.org/3/reference/datamodel.html#object.__iter__) 方法，mypy 就会理解它实现了可迭代协议并且与 [`Iterable[T]`](https://docs.python.org/3/library/typing.html#typing.Iterable) 兼容。 例如，下面的`IntList`是可迭代的，类型是`int`值：

    ```python
    from typing import Iterator, Iterable, Optional

    class IntList:
        def __init__(self, value: int, next: Optional['IntList']) -> None:
            self.value = value
            self.next = next

        def __iter__(self) -> Iterator[int]:
            current = self
            while current:
                yield current.value
                current = current.next

    def print_numbered(items: Iterable[int]) -> None:
        for n, x in enumerate(items):
            print(n + 1, x)

    x = IntList(3, IntList(5, None))
    print_numbered(x)  # OK
    print_numbered([4, 5])  # Also OK
    ```

    [`predefined_protocols_reference`](https://mypy.readthedocs.io/en/latest/protocols.html#predefined-protocols-reference) 列出了 [`typing`](https://docs.python.org/3/library/typing.html#module-typing) 中定义的所有协议以及您需要定义以实现每个协议的相应方法的签名。

=== "英文"

    **Predefined protocols**

    The [`typing`](https://docs.python.org/3/library/typing.html#module-typing) module defines various protocol classes that correspond to common Python protocols, such as [`Iterable[T]`](https://docs.python.org/3/library/typing.html#typing.Iterable). If a class defines a suitable [`__iter__`](https://docs.python.org/3/reference/datamodel.html#object.__iter__) method, mypy understands that it implements the iterable protocol and is compatible with [`Iterable[T]`](https://docs.python.org/3/library/typing.html#typing.Iterable). For example, `IntList` below is iterable, over `int` values:

    ```python
    from typing import Iterator, Iterable, Optional

    class IntList:
        def __init__(self, value: int, next: Optional['IntList']) -> None:
            self.value = value
            self.next = next

        def __iter__(self) -> Iterator[int]:
            current = self
            while current:
                yield current.value
                current = current.next

    def print_numbered(items: Iterable[int]) -> None:
        for n, x in enumerate(items):
            print(n + 1, x)

    x = IntList(3, IntList(5, None))
    print_numbered(x)  # OK
    print_numbered([4, 5])  # Also OK
    ```

    [`predefined_protocols_reference`](https://mypy.readthedocs.io/en/latest/protocols.html#predefined-protocols-reference) lists all protocols defined in [`typing`](https://docs.python.org/3/library/typing.html#module-typing) and the signatures of the corresponding methods you need to define to implement each protocol.

## 简单的自定义协议

=== "中文"

    您可以通过继承特殊的 `Protocol` 类来定义自己的协议类：

    ```python
    from typing import Iterable
    from typing_extensions import Protocol

    class SupportsClose(Protocol):
        #空方法体 (explicit '...')
        def close(self) -> None: ...

    class Resource:  # No SupportsClose base class!

        def close(self) -> None:
            self.resource.release()

        # ... 其他方法 ...

    def close_all(items: Iterable[SupportsClose]) -> None:
        for item in items:
            item.close()

    close_all([Resource(), open('some/file')])  # OK
    ```

    `Resource` 是 `SupportsClose` 协议的子类型，因为它定义了兼容的 `close` 方法。 [`open`](https://docs.python.org/3/library/functions.html#open) 返回的常规文件对象同样与协议兼容，因为它们支持 `close()`。

=== "英文"

    **Simple user-defined protocols**

    You can define your own protocol class by inheriting the special `Protocol` class:

    ```python
    from typing import Iterable
    from typing_extensions import Protocol

    class SupportsClose(Protocol):
        # Empty method body (explicit '...')
        def close(self) -> None: ...

    class Resource:  # No SupportsClose base class!

        def close(self) -> None:
        self.resource.release()

        # ... other methods ...

    def close_all(items: Iterable[SupportsClose]) -> None:
        for item in items:
            item.close()

    close_all([Resource(), open('some/file')])  # OK
    ```

    `Resource` is a subtype of the `SupportsClose` protocol since it defines a compatible `close` method. Regular file objects returned by [`open`](https://docs.python.org/3/library/functions.html#open) are similarly compatible with the protocol, as they support `close()`.

## 定义子协议和子类化协议

=== "中文"

    您还可以定义子协议。 可以使用多重继承来扩展和合并现有协议。 例子：

    ```python
    # ... 继续上一个示例

    class SupportsRead(Protocol):
        def read(self, amount: int) -> bytes: ...

    class TaggedReadableResource(SupportsClose, SupportsRead, Protocol):
        label: str

    class AdvancedResource(Resource):
        def __init__(self, label: str) -> None:
            self.label = label

        def read(self, amount: int) -> bytes:
            # some implementation
            ...

    resource: TaggedReadableResource
    resource = AdvancedResource('handle with care')  # OK
    ```

    请注意，从现有协议继承不会自动将子类转换为协议 - 它只是创建一个实现给定协议（或多个协议）的常规（非协议）类或 ABC。 如果您定义协议，{==**则 `Protocol` 基类必须始终显式存在**==}：

    ```python
    class NotAProtocol(SupportsClose):  # 这不是一个协议
        new_attr: int

    class Concrete:
    new_attr: int = 0

    def close(self) -> None:
        ...

    # Error: nominal subtyping used by default
    # Error: 默认使用的名义子类型
    x: NotAProtocol = Concrete()  # Error!
    ```

    您还可以在协议中包含方法的默认实现。 如果您明确地子类化这些协议，您可以继承这些默认实现。

    显式包含协议作为基类也是记录您的类实现特定协议的一种方式，并且它强制 mypy 验证您的类实现实际上与该协议兼容。 特别是，省略属性或方法体的值将使其隐式抽象：

    ```python
    class SomeProto(Protocol):
        attr: int  # 注意，没有右手边 （Note, no right hand side）
        def method(self) -> str: ...  # 从字面上看只是...这里 （Literally just ... here）

    class ExplicitSubclass(SomeProto):
        pass

    ExplicitSubclass()  # error: Cannot instantiate abstract class 'ExplicitSubclass'
                        # with abstract attributes 'attr' and 'method'
                        # error：无法使用抽象属性“attr”和“method”实例化抽象类“ExplicitSubclass”
    ```

    类似地，显式分配给协议实例可以是要求类型检查器验证您的类是否实现协议的一种方法：

    ```python
    _proto: SomeProto = cast(ExplicitSubclass, None)
    ```

=== "英文"

    **Defining subprotocols and subclassing protocols**

    You can also define subprotocols. Existing protocols can be extended
    and merged using multiple inheritance. Example:

    ```python
    # ... continuing from the previous example

    class SupportsRead(Protocol):
        def read(self, amount: int) -> bytes: ...

    class TaggedReadableResource(SupportsClose, SupportsRead, Protocol):
        label: str

    class AdvancedResource(Resource):
        def __init__(self, label: str) -> None:
            self.label = label

        def read(self, amount: int) -> bytes:
            # some implementation
            ...

    resource: TaggedReadableResource
    resource = AdvancedResource('handle with care')  # OK
    ```

    Note that inheriting from an existing protocol does not automatically
    turn the subclass into a protocol -- it just creates a regular
    (non-protocol) class or ABC that implements the given protocol (or
    protocols). The `Protocol` base class must always be explicitly
    present if you are defining a protocol:

    ```python
    class NotAProtocol(SupportsClose):  # This is NOT a protocol
        new_attr: int

    class Concrete:
    new_attr: int = 0

    def close(self) -> None:
        ...

    # Error: nominal subtyping used by default
    x: NotAProtocol = Concrete()  # Error!
    ```

    You can also include default implementations of methods in
    protocols. If you explicitly subclass these protocols you can inherit
    these default implementations.

    Explicitly including a protocol as a
    base class is also a way of documenting that your class implements a
    particular protocol, and it forces mypy to verify that your class
    implementation is actually compatible with the protocol. In particular,
    omitting a value for an attribute or a method body will make it implicitly
    abstract:

    ```python
    class SomeProto(Protocol):
        attr: int  # Note, no right hand side
        def method(self) -> str: ...  # Literally just ... here

    class ExplicitSubclass(SomeProto):
        pass

    ExplicitSubclass()  # error: Cannot instantiate abstract class 'ExplicitSubclass'
                        # with abstract attributes 'attr' and 'method'
    ```

    Similarly, explicitly assigning to a protocol instance can be a way to ask the
    type checker to verify that your class implements a protocol:

    ```python
    _proto: SomeProto = cast(ExplicitSubclass, None)
    ```

## 协议属性的不变性

=== "中文"

    协议的一个常见问题是协议属性是不变的。 例如：

    ```python
    class Box(Protocol):
        content: object

    class IntBox:
        content: int

    def takes_box(box: Box) -> None: ...

    takes_box(IntBox())  # error: Argument 1 to "takes_box" has incompatible type （不兼容类型） "IntBox"; expected "Box"
                        # note:  Following member(s) of "IntBox" have conflicts:
                        # note:      content: expected "object", got "int"
    ```

    这是因为 `Box` 将 `content` 定义为可变属性。 这就是为什么这是有问题的：

    ```python
    def takes_box_evil(box: Box) -> None:
        box.content = "asdf"  # 这很糟糕，因为 box.content 应该是一个对象

    my_int_box = IntBox()
    takes_box_evil(my_int_box)
    my_int_box.content + 1  # Oops, TypeError!
    ```

    可以通过使用 `@property` 在 `Box` 协议中将 `content` 声明为只读来解决此问题：

    ```python
    class Box(Protocol):
        @property
        def content(self) -> object: ...

    class IntBox:
        content: int

    def takes_box(box: Box) -> None: ...

    takes_box(IntBox(42))  # OK
    ```

=== "英文"

    **Invariance of protocol attributes**

    A common issue with protocols is that protocol attributes are invariant.
    For example:

    ```python
    class Box(Protocol):
        content: object

    class IntBox:
        content: int

    def takes_box(box: Box) -> None: ...

    takes_box(IntBox())  # error: Argument 1 to "takes_box" has incompatible type "IntBox"; expected "Box"
                        # note:  Following member(s) of "IntBox" have conflicts:
                        # note:      content: expected "object", got "int"
    ```

    This is because `Box` defines `content` as a mutable attribute.
    Here's why this is problematic:

    ```python
    def takes_box_evil(box: Box) -> None:
        box.content = "asdf"  # This is bad, since box.content is supposed to be an object

    my_int_box = IntBox()
    takes_box_evil(my_int_box)
    my_int_box.content + 1  # Oops, TypeError!
    ```

    This can be fixed by declaring `content` to be read-only in the `Box`
    protocol using `@property`:

    ```python
    class Box(Protocol):
        @property
        def content(self) -> object: ...

    class IntBox:
        content: int

    def takes_box(box: Box) -> None: ...

    takes_box(IntBox(42))  # OK
    ```

## 递归协议

=== "中文"

    协议可以是递归的（自引用）和相互递归的。 这对于声明抽象递归集合（例如树和链表）很有用：

    ```python
    from typing import TypeVar, Optional
    from typing_extensions import Protocol

    class TreeLike(Protocol):
        value: int

        @property
        def left(self) -> Optional['TreeLike']: ...

        @property
        def right(self) -> Optional['TreeLike']: ...

    class SimpleTree:
        def __init__(self, value: int) -> None:
            self.value = value
            self.left: Optional['SimpleTree'] = None
            self.right: Optional['SimpleTree'] = None

    root: TreeLike = SimpleTree(0)  # OK
    ```

=== "英文"

    **Recursive protocols**

    Protocols can be recursive (self-referential) and mutually recursive. This is useful for declaring abstract recursive collections such as trees and linked lists:

    ```python
    from typing import TypeVar, Optional
    from typing_extensions import Protocol

    class TreeLike(Protocol):
        value: int

        @property
        def left(self) -> Optional['TreeLike']: ...

        @property
        def right(self) -> Optional['TreeLike']: ...

    class SimpleTree:
        def __init__(self, value: int) -> None:
            self.value = value
            self.left: Optional['SimpleTree'] = None
            self.right: Optional['SimpleTree'] = None

    root: TreeLike = SimpleTree(0)  # OK
    ```

## isinstance() 和协议一起使用

=== "中文"

    如果使用`@runtime_checkable`类装饰器装饰协议类，则可以将其与 [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) 一起使用。 装饰器添加了对运行时结构检查的基本支持：

    ```python
    from typing_extensions import Protocol, runtime_checkable

    @runtime_checkable
    class Portable(Protocol):
        handles: int

    class Mug:
        def __init__(self) -> None:
            self.handles = 1

    def use(handles: int) -> None: ...

    mug = Mug()
    if isinstance(mug, Portable):  # Works at runtime!
    use(mug.handles)
    ```

    [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) 也适用于 [`预定义协议`](#预定义协议参考) 在 [`typing`](https://docs.python.org/3/library/typing.html#module-typing) 中，例如 [`Iterable`](https ://docs.python.org/3/library/typing.html#typing.Iterable)。


    !!! info "Warning"
    
        使用协议的 [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) 在运行时并不完全安全。 例如，不检查方法的签名。 运行时实现仅检查所有协议成员是否存在，而不检查它们是否具有正确的类型。 使用协议的 [`issubclass`](https://docs.python.org/3/library/functions.html#issubclass) 只会检查方法是否存在。


    !!! info "Note"
    
        使用协议的 [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) 也可能慢得惊人。 在许多情况下，使用 [`hasattr`](https://docs.python.org/3/library/functions.html#hasattr) 来检查属性是否存在会更好。

=== "英文"

    **Using isinstance() with protocols**

    You can use a protocol class with [isinstance()](https://docs.python.org/3/library/functions.html#isinstance) if you decorate it with the @runtime_checkable class decorator. The decorator adds rudimentary support for runtime structural checks:

    ```python
    from typing_extensions import Protocol, runtime_checkable

    @runtime_checkable
    class Portable(Protocol):
        handles: int

    class Mug:
        def __init__(self) -> None:
            self.handles = 1

    def use(handles: int) -> None: ...

    mug = Mug()
    if isinstance(mug, Portable):  # Works at runtime!
    use(mug.handles)
    ```

    [isinstance()](https://docs.python.org/3/library/functions.html#isinstance) also works with the [predefined](#预定义协议参考) protocols in [typing](https://docs.python.org/3/library/typing.html#module-typing) such as [Iterable](https://docs.python.org/3/library/typing.html#typing.Iterable).


    !!! info "Warning"
    
        使用协议的 [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) 在运行时并不完全安全。 例如，不检查方法的签名。 运行时实现仅检查所有协议成员是否存在，而不检查它们是否具有正确的类型。 使用协议的 [`issubclass`](https://docs.python.org/3/library/functions.html#issubclass) 只会检查方法是否存在。


    !!! info "Note"
    
        使用协议的 [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) 也可能慢得惊人。 在许多情况下，使用 [`hasattr`](https://docs.python.org/3/library/functions.html#hasattr) 来检查属性是否存在会更好。

## 回调协议

=== "中文"

    协议可用于定义灵活的回调类型，这些类型很难（甚至不可能）使用 [`Callable[...]`](https://docs.python.org/3/library/typing.html#types.Callable）语法，例如可变参数、重载和复杂的泛型回调。 它们是用特殊的 [`__call__`](https://docs.python.org/3/reference/datamodel.html#object.__call__) 成员定义的：

    ```python
    from typing import Optional, Iterable
    from typing_extensions import Protocol

    class Combiner(Protocol):
        def __call__(self, *vals: bytes, maxlen: Optional[int] = None) -> list[bytes]: ...

    def batch_proc(data: Iterable[bytes], cb_results: Combiner) -> bytes:
        for item in data:
            ...

    def good_cb(*vals: bytes, maxlen: Optional[int] = None) -> list[bytes]:
        ...
    def bad_cb(*vals: bytes, maxitems: Optional[int]) -> list[bytes]:
        ...

    batch_proc([], good_cb)  # OK
    batch_proc([], bad_cb)   # Error! Argument 2 has incompatible type because of
                            # different name and kind in the callback
    ```

    回调协议和 [`typing.Callable`](https://docs.python.org/3/library/typing.html#typing.Callable) 大部分类型可以互换使用。 [`__call__`](https://docs.python.org/3/reference/datamodel.html#object.__call__) 方法中的参数名称必须相同，除非使用双下划线前缀。 例如：

    ```python
    from typing import Callable, TypeVar
    from typing_extensions import Protocol

    T = TypeVar('T')

    class Copy(Protocol):
        def __call__(self, __origin: T) -> T: ...

    copy_a: Callable[[T], T]
    copy_b: Copy

    copy_a = copy_b  # OK
    copy_b = copy_a  # Also OK
    ```

=== "英文"

    **Callback protocols**

    Protocols can be used to define flexible callback types that are hard (or even impossible) to express using the [`Callable[...]`](https://docs.python.org/3/library/typing.html#typing.Callable) syntax, such as variadic, overloaded, and complex generic callbacks. They are defined with a special [`__call__`](https://docs.python.org/3/reference/datamodel.html#object.__call__) member:

    ```python
    from typing import Optional, Iterable
    from typing_extensions import Protocol

    class Combiner(Protocol):
        def __call__(self, *vals: bytes, maxlen: Optional[int] = None) -> list[bytes]: ...

    def batch_proc(data: Iterable[bytes], cb_results: Combiner) -> bytes:
        for item in data:
            ...

    def good_cb(*vals: bytes, maxlen: Optional[int] = None) -> list[bytes]:
        ...
    def bad_cb(*vals: bytes, maxitems: Optional[int]) -> list[bytes]:
        ...

    batch_proc([], good_cb)  # OK
    batch_proc([], bad_cb)   # Error! Argument 2 has incompatible type because of
                            # different name and kind in the callback
    ```

    Callback protocols and [`typing.Callable`](https://docs.python.org/3/library/typing.html#typing.Callable) types can be used mostly interchangeably. Argument names in [`__call__`](https://docs.python.org/3/reference/datamodel.html#object.__call__) methods must be identical, unless a double underscore prefix is used. For example:

    ```python
    from typing import Callable, TypeVar
    from typing_extensions import Protocol

    T = TypeVar('T')

    class Copy(Protocol):
        def __call__(self, __origin: T) -> T: ...

    copy_a: Callable[[T], T]
    copy_b: Copy

    copy_a = copy_b  # OK
    copy_b = copy_a  # Also OK
    ```

## 预定义协议参考

### 迭代协议

Iteration protocols

=== "中文"

    迭代协议在许多情况下都很有用。 例如，它们允许在 for 循环中迭代对象。

=== "英文"

    The iteration protocols are useful in many contexts. For example, they allow iteration of objects in for loops.

#### 可迭代泛型

Iterable\[T\]

=== "中文"

    [`上面的例子`](#预定义协议)有一个 [`__iter__`](https://docs.python.org/3/reference/datamodel.html#object.__iter__) 方法的简单实现。

    ```python
    def __iter__(self) -> Iterator[T]
    ```

    另请参阅[`typing.Iterable`](https://docs.python.org/3/library/typing.html#typing.Iterable)。

=== "英文"

    The [`example above`](#预定义协议) has a simple implementation of an [`__iter__`](https://docs.python.org/3/reference/datamodel.html#object.__iter__) method.

    ```python
    def __iter__(self) -> Iterator[T]
    ```

    See also [`typing.Iterable`](https://docs.python.org/3/library/typing.html#typing.Iterable).

#### 迭代器泛型

Iterator\[T\]

=== "中文"

    ```python
    def __next__(self) -> T
    def __iter__(self) -> Iterator[T]
    ```

    也可以看看 [`typing.Iterator`](https://docs.python.org/3/library/typing.html#typing.Iterator).

=== "英文"

    ```python
    def __next__(self) -> T
    def __iter__(self) -> Iterator[T]
    ```

    See also [`typing.Iterator`](https://docs.python.org/3/library/typing.html#typing.Iterator).

### 集合协议

Collection protocols

=== "中文"

    其中许多是通过内置容器类型实现的，例如 [`list`](https://docs.python.org/3/library/stdtypes.html#list) 和 [`dict`](https:// docs.python.org/3/library/stdtypes.html#dict），这些对于用户定义的集合对象也很有用。

=== "英文"

    **Collection protocols**

    Many of these are implemented by built-in container types such as [`list`](https://docs.python.org/3/library/stdtypes.html#list) and [`dict`](https://docs.python.org/3/library/stdtypes.html#dict), and these are also useful for user-defined collection objects.

#### Sized

=== "中文"

    这是支持 [`len(x)`](https://docs.python.org/3/library/functions.html#len) 的对象类型。

    ```python
    def __len__(self) -> int
    ```

    也可以看看 [`typing.Sized`](https://docs.python.org/3/library/typing.html#typing.Sized).

=== "英文"

    This is a type for objects that support [`len(x)`](https://docs.python.org/3/library/functions.html#len).

    ```python
    def __len__(self) -> int
    ```

    See also [`typing.Sized`](https://docs.python.org/3/library/typing.html#typing.Sized).

#### 泛型容器

Container\[T\]

=== "中文"

    这是支持 `in` 运算符的对象类型。

    ```python
    def __contains__(self, x: object) -> bool
    ```

    同样参考 [`Container`](https://docs.python.org/3/library/typing.html#typing.Container).

=== "英文"

    This is a type for objects that support the `in` operator.

    ```python
    def __contains__(self, x: object) -> bool
    ```

    See also [`Container`](https://docs.python.org/3/library/typing.html#typing.Container).

#### 集合泛型

Collection\[T\]

=== "中文"

    ```python
    def __len__(self) -> int
    def __iter__(self) -> Iterator[T]
    def __contains__(self, x: object) -> bool
    ```

    同样参考 [`Collection`](https://docs.python.org/3/library/typing.html#typing.Collection).

=== "英文"

    ```python
    def __len__(self) -> int
    def __iter__(self) -> Iterator[T]
    def __contains__(self, x: object) -> bool
    ```

    See also [`Collection`](https://docs.python.org/3/library/typing.html#typing.Collection).

### 一次性协议

One-off protocols

=== "中文"

    这些协议通常仅适用于单个标准库函数或类。

=== "英文"

    These protocols are typically only useful with a single standard library function or class.

#### 倒序泛型

Reversible\[T\]

=== "中文"

    这是支持 [`reversed(x)`](https://docs.python.org/3/library/functions.html#reversed) 的对象类型。

    ```python
    def __reversed__(self) -> Iterator[T]
    ```

    同样参考 [`Reversible`](https://docs.python.org/3/library/typing.html#typing.Reversible).

=== "英文"

    This is a type for objects that support [`reversed(x)`](https://docs.python.org/3/library/functions.html#reversed).

    ```python
    def __reversed__(self) -> Iterator[T]
    ```

    See also [`Reversible`](https://docs.python.org/3/library/typing.html#typing.Reversible).

#### 绝对值泛型

SupportsAbs\[T\]

=== "中文"

    这是支持 [`abs(x)`](https://docs.python.org/3/library/functions.html#abs) 的对象类型。 `T` 是 [`abs(x)`](https://docs.python.org/3/library/functions.html#abs) 返回的值的类型。

    ```python
    def __abs__(self) -> T
    ```

    同样参考 [`SupportsAbs`](https://docs.python.org/3/library/typing.html#typing.SupportsAbs).

=== "英文"

    This is a type for objects that support [`abs(x)`](https://docs.python.org/3/library/functions.html#abs). `T` is the type of value returned by [`abs(x)`](https://docs.python.org/3/library/functions.html#abs).

    ```python
    def __abs__(self) -> T
    ```

    See also [`SupportsAbs`](https://docs.python.org/3/library/typing.html#typing.SupportsAbs).

#### 支持字节

SupportsBytes

=== "中文"

    这是支持 [`bytes(x)`](https://docs.python.org/3/library/stdtypes.html#bytes) 的对象类型。

    ```python
    def __bytes__(self) -> bytes
    ```

    另请参阅 [`SupportsBytes`](https://docs.python.org/3/library/typing.html#typing.SupportsBytes)。

=== "英文"

    This is a type for objects that support [`bytes(x)`](https://docs.python.org/3/library/stdtypes.html#bytes).

    ```python
    def __bytes__(self) -> bytes
    ```

    See also [`SupportsBytes`](https://docs.python.org/3/library/typing.html#typing.SupportsBytes).

#### 支持复数

SupportsComplex

=== "中文"

    这是支持 [`complex(x)`](https://docs.python.org/3/library/functions.html#complex) 的对象类型。 请注意，不支持算术运算。

    ```python
    def __complex__(self) -> complex
    ```

    另请参阅 [`SupportsComplex`](https://docs.python.org/3/library/typing.html#typing.SupportsComplex).

=== "英文"

    This is a type for objects that support [`complex(x)`](https://docs.python.org/3/library/functions.html#complex). Note that no arithmetic operations are supported.

    ```python
    def __complex__(self) -> complex
    ```

    See also [`SupportsComplex`](https://docs.python.org/3/library/typing.html#typing.SupportsComplex).

#### 支持浮点数

SupportsFloat

=== "中文"

    这是支持 [`float(x)`](https://docs.python.org/3/library/functions.html#float) 的对象类型。 请注意，不支持算术运算。

    ```python
    def __float__(self) -> float
    ```

    另请参阅 [`SupportsFloat`](https://docs.python.org/3/library/typing.html#typing.SupportsFloat).

=== "英文"

    This is a type for objects that support [`float(x)`](https://docs.python.org/3/library/functions.html#float). Note that no arithmetic operations are supported.

    ```python
    def __float__(self) -> float
    ```

    See also [`SupportsFloat`](https://docs.python.org/3/library/typing.html#typing.SupportsFloat).

#### 支持整数

SupportsInt

=== "中文"

    这是支持 [`int(x)`](https://docs.python.org/3/library/functions.html#int) 的对象类型。 请注意，不支持算术运算。

    ```python
    def __int__(self) -> int
    ```

    另请参阅 [`SupportsInt`](https://docs.python.org/3/library/typing.html#typing.SupportsInt).

=== "英文"

    This is a type for objects that support [`int(x)`](https://docs.python.org/3/library/functions.html#int). Note that no arithmetic operations are supported.

    ```python
    def __int__(self) -> int
    ```

    See also [`SupportsInt`](https://docs.python.org/3/library/typing.html#typing.SupportsInt).

#### 支持Round泛型

SupportsRound\[T\]

=== "中文"

    这是支持 [`round(x)`](https://docs.python.org/3/library/functions.html#round) 的对象类型。

    ```python
    def __round__(self) -> T
    ```

    另请参阅 [`SupportsRound`](https://docs.python.org/3/library/typing.html#typing.SupportsRound).

=== "英文"

    This is a type for objects that support [`round(x)`](https://docs.python.org/3/library/functions.html#round).

    ```python
    def __round__(self) -> T
    ```

    See also [`SupportsRound`](https://docs.python.org/3/library/typing.html#typing.SupportsRound).

### 异步协议

Async protocols

=== "中文"

    这些协议在异步代码中很有用。 请参阅 [`async-and-await`](./more_types.md) 了解更多信息。

=== "英文"

    These protocols can be useful in async code. See [`async-and-await`](./more_types.md) for more information.

#### 可等待泛型对象

Awaitable\[T\]

=== "中文"

    ```python
    def __await__(self) -> Generator[Any, None, T]
    ```

    另请参阅 [`Awaitable`](https://docs.python.org/3/library/typing.html#typing.Awaitable).

=== "英文"

    ```python
    def __await__(self) -> Generator[Any, None, T]
    ```

    See also [`Awaitable`](https://docs.python.org/3/library/typing.html#typing.Awaitable).

#### 异步可迭代泛型对象

AsyncIterable\[T\]

=== "中文"

    ```python
    def __aiter__(self) -> AsyncIterator[T]
    ```

    另请参阅 [`AsyncIterable`](https://docs.python.org/3/library/typing.html#typing.AsyncIterable).

=== "英文"

    ```python
    def __aiter__(self) -> AsyncIterator[T]
    ```

    See also [`AsyncIterable`](https://docs.python.org/3/library/typing.html#typing.AsyncIterable).

#### 异步迭代器泛型对象

AsyncIterator\[T\]

=== "中文"

    ```python
    def __anext__(self) -> Awaitable[T]
    def __aiter__(self) -> AsyncIterator[T]
    ```

    另请参阅 [`AsyncIterator`](https://docs.python.org/3/library/typing.html#typing.AsyncIterator).

=== "英文"

    ```python
    def __anext__(self) -> Awaitable[T]
    def __aiter__(self) -> AsyncIterator[T]
    ```

    See also [`AsyncIterator`](https://docs.python.org/3/library/typing.html#typing.AsyncIterator).

### 上下文管理器协议

Context manager protocols

=== "中文"

    上下文管理器有两种协议——一种用于常规上下文管理器，另一种用于异步上下文管理器。 这些允许定义可在`with`和`async with`语句中使用的对象。

=== "英文"

    There are two protocols for context managers -- one for regular context managers and one for async ones. These allow defining objects that can be used in `with` and `async with` statements.

#### 上下文管理器泛型

ContextManager\[T\]

=== "中文"

    ```python
    def __enter__(self) -> T
    def __exit__(self,
                exc_type: Optional[Type[BaseException]],
                exc_value: Optional[BaseException],
                traceback: Optional[TracebackType]) -> Optional[bool]
    ```

    另请参阅 [`ContextManager`](https://docs.python.org/3/library/typing.html#typing.ContextManager).

=== "英文"

    ```python
    def __enter__(self) -> T
    def __exit__(self,
                exc_type: Optional[Type[BaseException]],
                exc_value: Optional[BaseException],
                traceback: Optional[TracebackType]) -> Optional[bool]
    ```

    See also [`ContextManager`](https://docs.python.org/3/library/typing.html#typing.ContextManager).

#### 异步上下文泛型管理器

AsyncContextManager\[T\]

=== "中文"

    ```python
    def __aenter__(self) -> Awaitable[T]
    def __aexit__(self,
                exc_type: Optional[Type[BaseException]],
                exc_value: Optional[BaseException],
                traceback: Optional[TracebackType]) -> Awaitable[Optional[bool]]
    ```

    另请参阅 [`AsyncContextManager`](https://docs.python.org/3/library/typing.html#typing.AsyncContextManager)。

=== "英文"

    ```python
    def __aenter__(self) -> Awaitable[T]
    def __aexit__(self,
                exc_type: Optional[Type[BaseException]],
                exc_value: Optional[BaseException],
                traceback: Optional[TracebackType]) -> Awaitable[Optional[bool]]
    ```

    See also [`AsyncContextManager`](https://docs.python.org/3/library/typing.html#typing.AsyncContextManager).
