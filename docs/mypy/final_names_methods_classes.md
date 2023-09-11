# 最终的命名、方法、类

=== "中文"

    本节介绍这些相关功能：
    
    1. *最终名称* 是初始化后不应重新分配的变量或属性。 它们对于声明常量很有用。
    2. *最终方法* 不应在子类中重写。
    3. *最终类* 不应该被子类化。
    
    所有这些仅由 mypy 强制执行，并且仅在带注释的代码中执行。 Python 运行时不执行任何运行时强制措施。
    
    !!! info "Note"
    
        本页中的示例从 `typing` 模块导入 `Final` 和 `final` 。 这些类型已添加到 Python 3.8 中的 `typing` 中，但也可以通过 `typing_extensions` 包在 Python 3.4 - 3.7 中使用。

=== "英文"

    **Final names, methods and classes**
    
    This section introduces these related features:
    
    1. *Final names* are variables or attributes that should not be reassigned after initialization. They are useful for declaring constants.
    2. *Final methods* should not be overridden in a subclass.
    3. *Final classes* should not be subclassed.
    
    All of these are only enforced by mypy, and only in annotated code. There is no runtime enforcement by the Python runtime.
    
    !!! info "Note"
    
        The examples in this page import `Final` and `final` from the `typing` module. These types were added to `typing` in Python 3.8, but are also available for use in Python 3.4 - 3.7 via the `typing_extensions` package.

## 最终名称

Final names

=== "中文"

    您可以使用`typing.Final`限定符来指示不应重新分配、重新定义或覆盖名称或属性。 这对于模块和类级别常量通常很有用，可以作为防止意外修改的方法。 Mypy 将阻止在类型检查代码中进一步分配最终名称：

    ```python
    from typing import Final

    RATE: Final = 3_000

    class Base:
        DEFAULT_ID: Final = 0

    RATE = 300  # Error: can't assign to final attribute
    Base.DEFAULT_ID = 1  # Error: can't override a final attribute
    ```

    最终属性的另一个用例是保护某些属性不被子类中覆盖：

    ```python
    from typing import Final

    class Window:
        BORDER_WIDTH: Final = 2.5
        ...

    class ListView(Window):
        BORDER_WIDTH = 3  # Error: can't override a final attribute
    ```

    您可以使用 [`@property`](https://docs.python.org/3/library/functions.html#property) 将属性设置为只读，但与 `Final` 不同，它不适用于模块属性，并且它不会阻止子类中的重写。

=== "英文"

    You can use the `typing.Final` qualifier to indicate that a name or attribute should not be reassigned, redefined, or overridden.  This is often useful for module and class level constants as a way to prevent unintended modification.  Mypy will prevent further assignments to final names in type-checked code:

    ```python
    from typing import Final

    RATE: Final = 3_000

    class Base:
        DEFAULT_ID: Final = 0

    RATE = 300  # Error: can't assign to final attribute
    Base.DEFAULT_ID = 1  # Error: can't override a final attribute
    ```

    Another use case for final attributes is to protect certain attributes from being overridden in a subclass:

    ```python
    from typing import Final

    class Window:
        BORDER_WIDTH: Final = 2.5
        ...

    class ListView(Window):
        BORDER_WIDTH = 3  # Error: can't override a final attribute
    ```

    You can use [`@property`](https://docs.python.org/3/library/functions.html#property) to make an attribute read-only, but unlike `Final`, it doesn't work with module attributes, and it doesn't prevent overriding in subclasses.

### 语法变体

Syntax variants

=== "中文"

    您可以以下列形式之一使用`Final`：

    - 您可以使用语法`Final[<type>]`提供显式类型。 例子：

      ```python
      ID: Final[int] = 1
      ```

      这里 mypy 会推断出“ID”的类型为“int”。

    - 您可以省略类型：

      ```python
      ID: Final = 1
      ```

      这里 mypy 会推断出 `ID` 的类型为 `Literal[1]`。 请注意，与泛型类不同，这与“Final[Any]”不同。

    - 在类体和存根文件中，您可以省略右侧，只写`ID: Final[int]`。

    - 最后，您可以编写`self.id: Final = 1`（也可以选择使用方括号中的类型）。 *仅*在 [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) 方法中允许这样做，以便最终实例属性仅在实例时分配一次 被建造。

=== "英文"

    You can use `Final` in one of these forms:

    - You can provide an explicit type using the syntax `Final[<type>]`. Example:

      ```python
      ID: Final[int] = 1
      ```

      Here mypy will infer type `int` for `ID`.

    - You can omit the type:

      ```python
      ID: Final = 1
      ```

      Here mypy will infer type `Literal[1]` for `ID`. Note that unlike for generic classes this is *not* the same as `Final[Any]`.

    - In class bodies and stub files you can omit the right hand side and just write
      `ID: Final[int]`.

    - Finally, you can write `self.id: Final = 1` (also optionally with a type in square brackets). This is allowed *only* in [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) methods, so that the final instance attribute is assigned only once when an instance is created.

### 使用`Final`的详情

Details of using `Final`

=== "中文"

    这是定义最终名称的两个主要规则：

    - 对于给定的属性，每个模块或类最多可以有一个最终声明。 不能存在具有相同名称的单独的类级和实例级常量。
    - 必须有“恰好一个”分配给最终名称。

    在没有初始化程序的类主体中声明的最终属性必须在 [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) 方法中初始化（您可以跳过存根文件中的初始化程序）：

    ```python
    class ImmutablePoint:
        x: Final[int]
        y: Final[int]  # Error: final attribute without an initializer

        def __init__(self) -> None:
            self.x = 1  # Good
    ```

    `Final` 只能用作赋值或变量注释中的最外层类型。 在任何其他位置使用它都是错误的。 特别是，`Final`不能在函数参数的注释中使用：

    ```python
    x: list[Final[int]] = []  # Error!

    def fun(x: Final[list[int]]) ->  None:  # Error!
        ...
    ```

    `Final` 和 [`typing.ClassVar`](https://docs.python.org/3/library/typing.html#typing.ClassVar) 不应一起使用。 Mypy 将自动推断最终声明的范围，具体取决于它是在类主体中还是在 [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) 中初始化 。

    最终属性不能被子类覆盖（即使有另一个显式最终声明）。 但请注意，最终属性可以覆盖只读属性：

    ```python
    class Base:
        @property
        def ID(self) -> int: ...

    class Derived(Base):
        ID: Final = 1  # OK
    ```

    将名称声明为最终名称只能保证该名称不会重新绑定到另一个值。 它并不使值变得不可变。 您可以使用不可变的 ABC 和容器来防止更改此类值：

    ```python
    x: Final = ['a', 'b']
    x.append('c')  # OK

    y: Final[Sequence[str]] = ['a', 'b']
    y.append('x')  # Error: Sequence is immutable
    z: Final = ('a', 'b')  # Also an option
    ```

=== "英文"

    These are the two main rules for defining a final name:

    - There can be *at most one* final declaration per module or class for a given attribute. There can't be separate class-level and instance-level constants with the same name.
    - There must be *exactly one* assignment to a final name.

    A final attribute declared in a class body without an initializer must be initialized in the [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) method (you can skip the initializer in stub files):

    ```python
    class ImmutablePoint:
        x: Final[int]
        y: Final[int]  # Error: final attribute without an initializer

        def __init__(self) -> None:
            self.x = 1  # Good
    ```

    `Final` can only be used as the outermost type in assignments or variable annotations. Using it in any other position is an error. In particular, `Final` can't be used in annotations for function arguments:

    ```python
    x: list[Final[int]] = []  # Error!

    def fun(x: Final[list[int]]) ->  None:  # Error!
        ...
    ```

    `Final` and {py:data}`~typing.ClassVar` should not be used together. Mypy will infer the scope of a final declaration automatically depending on whether it was initialized in the class body or in {py:meth}`__init__ <object.__init__>`.

    A final attribute can't be overridden by a subclass (even with another explicit final declaration). Note however that a final attribute can override a read-only property:

    ```python
    class Base:
        @property
        def ID(self) -> int: ...

    class Derived(Base):
        ID: Final = 1  # OK
    ```

    Declaring a name as final only guarantees that the name will not be re-bound to another value. It doesn't make the value immutable. You can use immutable ABCs and containers to prevent mutating such values:

    ```python
    x: Final = ['a', 'b']
    x.append('c')  # OK

    y: Final[Sequence[str]] = ['a', 'b']
    y.append('x')  # Error: Sequence is immutable
    z: Final = ('a', 'b')  # Also an option
    ```

## 最终方法

Final methods

=== "中文"

    与属性一样，有时保护方法免遭重写很有用。 您可以使用 `typing.final` 装饰器来实现此目的：

    ```python
    from typing import final

    class Base:
        @final
        def common_name(self) -> None:
            ...

    class Derived(Base):
        def common_name(self) -> None:  # Error: cannot override a final method
            ...
    ```

    这个`@final`装饰器可以与实例方法、类方法、静态方法和属性一起使用。

    对于重载方法，您应该在实现上添加`@final`以使其最终（或在存根中的第一个重载上）：

    ```python
    from typing import Any, overload

    class Base:
        @overload
        def method(self) -> None: ...
        @overload
        def method(self, arg: int) -> int: ...
        @final
        def method(self, x=None):
            ...
    ```

=== "英文"

    Like with attributes, sometimes it is useful to protect a method from overriding. You can use the `typing.final` decorator for this purpose:

    ```python
    from typing import final

    class Base:
        @final
        def common_name(self) -> None:
            ...

    class Derived(Base):
        def common_name(self) -> None:  # Error: cannot override a final method
            ...
    ```

    This `@final` decorator can be used with instance methods, class methods, static methods, and properties.

    For overloaded methods you should add `@final` on the implementation to make it final (or on the first overload in stubs):

    ```python
    from typing import Any, overload

    class Base:
        @overload
        def method(self) -> None: ...
        @overload
        def method(self, arg: int) -> int: ...
        @final
        def method(self, x=None):
            ...
    ```

## 最终类

Final classes

=== "中文"

    您可以将 `typing.final` 装饰器应用于一个类，以指示 mypy 它不应被子类化：

    ```python
    from typing import final

    @final
    class Leaf:
        ...

    class MyLeaf(Leaf):  # Error: Leaf can't be subclassed
        ...
    ```

    装饰器充当 mypy 的声明（以及人类的文档），但它实际上并不能阻止运行时的子类化。

    以下是使用 Final 类可能有用的一些情况：

    - 类并不是设计来进行子类化的。 也许子类化不会按预期工作，或者子类化很容易出错。
    - 子类化会使代码更难理解或维护。 例如，您可能希望防止基类和子类之间不必要的紧密耦合。
    - 您希望保留将来任意更改类实现的自由，而这些更改可能会破坏子类。

    定义至少一个抽象方法或属性并具有 `@final` 装饰器的抽象类将从 mypy 中生成错误，因为这些属性永远无法实现。

    ```python
    from abc import ABCMeta, abstractmethod
    from typing import final

    @final
    class A(metaclass=ABCMeta):  # error: Final class A has abstract attributes "f"
        @abstractmethod
        def f(self, x: int) -> None: pass
    ```

=== "英文"

    You can apply the `typing.final` decorator to a class to indicate to mypy that it should not be subclassed:

    ```python
    from typing import final

    @final
    class Leaf:
        ...

    class MyLeaf(Leaf):  # Error: Leaf can't be subclassed
        ...
    ```

    The decorator acts as a declaration for mypy (and as documentation for humans), but it doesn't actually prevent subclassing at runtime.

    Here are some situations where using a final class may be useful:

    - A class wasn't designed to be subclassed. Perhaps subclassing would not work as expected, or subclassing would be error-prone.
    - Subclassing would make code harder to understand or maintain. For example, you may want to prevent unnecessarily tight coupling between base classes and subclasses.
    - You want to retain the freedom to arbitrarily change the class implementation in the future, and these changes might break subclasses.

    An abstract class that defines at least one abstract method or property and has `@final` decorator will generate an error from mypy, since those attributes could never be implemented.

    ```python
    from abc import ABCMeta, abstractmethod
    from typing import final

    @final
    class A(metaclass=ABCMeta):  # error: Final class A has abstract attributes "f"
        @abstractmethod
        def f(self, x: int) -> None: pass
    ```
