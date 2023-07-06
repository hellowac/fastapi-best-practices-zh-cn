# 类基础

=== "中文"

    本节将帮助您开始为类添加注解。 诸如 “int” 之类的内置类也遵循这些相同的规则。

=== "英文"

    **Class basics**

    This section will help get you started annotating your classes. Built-in classes such as `int` also follow these same rules.

## 实例和类属性

=== "中文"

    mypy 类型检查器会检测您是否尝试访问丢失的属性，这是一个非常常见的编程错误。 为了使其正常工作，必须在类中定义或初始化实例和类属性。 Mypy 推断属性的类型：
    
    ```python
    class A:
        def __init__(self, x: int) -> None:
            self.x = x  # Aha, attribute 'x' of type 'int'
    
    a = A(1)
    a.x = 2  # OK!
    a.y = 3  # Error: "A" has no attribute "y"
    ```
    
    这有点像每个类都有一个隐式定义的 [`__slots__`](https://docs.python.org/3/reference/datamodel.html#object.__slots__) 属性。 这仅在类型检查期间强制执行，而不是在程序运行时强制执行。
    
    您可以使用类型注释显式声明类主体中的变量类型：
    
    ```python
    class A:
        x: list[int]  # Declare attribute 'x' of type list[int]
    
    a = A()
    a.x = [1]     # OK
    ```
    
    通常在 Python 中，类体中定义的变量可以用作类变量或实例变量。 （如下一节所述，您可以使用 [`typing.ClassVar`](https://docs.python.org/3/library/typing.html#typing.ClassVar) 注释覆盖它。）
    
    同样，您可以为方法中定义的实例变量指定显式类型：
    
    ```python
    class A:
        def __init__(self) -> None:
            self.x: list[int] = []
    
        def f(self) -> None:
            self.y: Any = 0
    ```
    
    如果您使用 `self` 显式分配实例变量，则只能在方法中定义实例变量：

    ```python
    class A:
        def __init__(self) -> None:
            self.y = 1   # Define 'y'
            a = self
            a.x = 1      # Error: 'x' not defined
    ```

=== "英文"

    **Instance and class attributes**

    The mypy type checker detects if you are trying to access a missing attribute, which is a very common programming error. For this to work correctly, instance and class attributes must be defined or initialized within the class. Mypy infers the types of attributes:
    
    ```python
    class A:
        def __init__(self, x: int) -> None:
            self.x = x  # Aha, attribute 'x' of type 'int'
    
    a = A(1)
    a.x = 2  # OK!
    a.y = 3  # Error: "A" has no attribute "y"
    ```
    
    This is a bit like each class having an implicitly defined [`__slots__`](https://docs.python.org/3/reference/datamodel.html#object.__slots__) attribute. This is only enforced during type checking and not when your program is running.
    
    You can declare types of variables in the class body explicitly using a type annotation:
    
    ```python
    class A:
        x: list[int]  # Declare attribute 'x' of type list[int]
    
    a = A()
    a.x = [1]     # OK
    ```
    
    As in Python generally, a variable defined in the class body can be used as a class or an instance variable. (As discussed in the next section, you can override this with a [`typing.ClassVar`](https://docs.python.org/3/library/typing.html#typing.ClassVar) annotation.)
    
    Similarly, you can give explicit types to instance variables defined in a method:
    
    ```python
    class A:
        def __init__(self) -> None:
            self.x: list[int] = []
    
        def f(self) -> None:
            self.y: Any = 0
    ```
    
    You can only define an instance variable within a method if you assign to it explicitly using `self`:
    
    ```python
    class A:
        def __init__(self) -> None:
            self.y = 1   # Define 'y'
            a = self
            a.x = 1      # Error: 'x' not defined
    ```

## 注解 _\_init\_\_ 方法

=== "中文"

    [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) 方法有些特殊——它不返回值。 这最好表达为 `-> None`。 然而，由于许多人认为这是多余的，因此允许省略 [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) 方法的返回类型声明 **如果至少有一个参数被注释**。 例如，在以下类中 [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) 被认为是完全注释的：
    
    ```python
    class C1:
        def __init__(self) -> None:
            self.var = 42
    
    class C2:
        def __init__(self, arg: int):
            self.var = arg
    ```
    
    但是，如果 [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) 没有带注解的参数，也没有返回类型注解，则它被视为无类型注解方法：
    
    ```python
    class C3:
        def __init__(self):
            # 该主体将不会进行类型检查
            self.var = 42 + 'abc'
    ```

=== "英文"

    **Annotating \_\_init\_\_ methods**

    The [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) method is somewhat special -- it doesn't return a value.  This is best expressed as `-> None`.  However, since many feel this is redundant, it is allowed to omit the return type declaration on [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) methods **if at least one argument is annotated**.  For example, in the following classes [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) is considered fully annotated:
    
    ```python
    class C1:
        def __init__(self) -> None:
            self.var = 42
    
    class C2:
        def __init__(self, arg: int):
            self.var = arg
    ```
    
    However, if [`__init__`](https://docs.python.org/3/reference/datamodel.html#object.__init__) has no annotated arguments and no return type annotation, it is considered an untyped method:
    
    ```python
    class C3:
        def __init__(self):
            # This body is not type checked
            self.var = 42 + 'abc'
    ```

## 类属性注解

=== "中文"

    您可以使用 [`ClassVar[t]`](https://docs.python.org/3/library/typing.html#typing.ClassVar) 注释来显式声明不应在实例上设置特定属性：

    ```python
    from typing import ClassVar
    
    class A:
        x: ClassVar[int] = 0  # 仅类变量
    
    A.x += 1  # OK
    
    a = A()
    a.x = 1  # Error: 无法通过实例分配给类变量“x”
    print(a.x)  # OK -- 可以通过实例读取
    ```
    
    没有必要使用 [`ClassVar[t]`](https://docs.python.org/3/library/typing.html#typing.ClassVar) 注解所有类变量。 没有 [`ClassVar[t]`](https://docs.python.org/3/library/typing.html#typing.ClassVar) 注释的属性仍然可以用作类变量。 然而，mypy 不会阻止它被用作实例变量，如前面所讨论的：
    
    ```python
    class A:
        x = 0  # 可以用作类或实例变量
    
    A.x += 1  # OK
    
    a = A()
    a.x = 1  # 还可以
    ```
    
    请注意， [`ClassVar[t]`](https://docs.python.org/3/library/typing.html#typing.ClassVar) 不是一个类，并且您不能将它与 [`isinstance` 一起使用 ](https://docs.python.org/3/library/functions.html#isinstance) 或 [`issubclass`](https://docs.python.org/3/library/functions.html#issubclass)。 它不会改变 Python 运行时行为——它仅适用于类型检查器，例如 mypy（对人类读者也有帮助）。
    
    您还可以在 [`ClassVar[t]`](https://docs.python.org/3/library/typing.html#typing.ClassVar) 注释中省略方括号和变量类型，但这可能不会是你所期望的：
    
    ```python
    class A:
        y: ClassVar = 0  # 类型隐式为 Any!
    ```
    
    在这种情况下，属性的类型将隐式为“Any”。 这种行为将来会改变，因为它令人惊讶。
    
    显式的 [`ClassVar[t]`](https://docs.python.org/3/library/typing.html#typing.ClassVar) 可能特别方便区分具有可调用类型的类变量和实例变量。 例如：

    ```python
    from typing import Callable, ClassVar
    
    class A:
        foo: Callable[[int], None]
        bar: ClassVar[Callable[[A, int], None]]
        bad: Callable[[A], None]
    
    A().foo(42)  # OK
    A().bar(42)  # OK
    A().bad()  # Error: Too few arguments
    ```
    
    !!! info "Note"

        [`ClassVar[t]`](https://docs.python.org/3/library/typing.html#typing.ClassVar) 类型参数不能包含类型变量：`ClassVar[T]` 和 `ClassVar[list 如果 `T` 是类型变量，则 [T]]` 均无效（有关更多信息，请参阅 [`generic-classes`](https://mypy.readthedocs.io/en/stable/generics.html#generic-classes) 关于类型变量）。

=== "英文"

    **Class attribute annotations**

    You can use a [`ClassVar[t]`](https://docs.python.org/3/library/typing.html#typing.ClassVar) annotation to explicitly declare that a particular attribute should not be set on instances:

    ```python
    from typing import ClassVar
    
    class A:
        x: ClassVar[int] = 0  # Class variable only
    
    A.x += 1  # OK
    
    a = A()
    a.x = 1  # Error: Cannot assign to class variable "x" via instance
    print(a.x)  # OK -- can be read through an instance
    ```
    
    It's not necessary to annotate all class variables using [`ClassVar[t]`](https://docs.python.org/3/library/typing.html#typing.ClassVar). An attribute without the [`ClassVar[t]`](https://docs.python.org/3/library/typing.html#typing.ClassVar) annotation can still be used as a class variable. However, mypy won't prevent it from being used as an instance variable, as discussed previously:
    
    ```python
    class A:
        x = 0  # Can be used as a class or instance variable
    
    A.x += 1  # OK
    
    a = A()
    a.x = 1  # Also OK
    ```
    
    Note that [`ClassVar[t]`](https://docs.python.org/3/library/typing.html#typing.ClassVar) is not a class, and you can't use it with [`isinstance`](https://docs.python.org/3/library/functions.html#isinstance) or [`issubclass`](https://docs.python.org/3/library/functions.html#issubclass). It does not change Python runtime behavior -- it's only for type checkers such as mypy (and also helpful for human readers).
    
    You can also omit the square brackets and the variable type in a [`ClassVar[t]`](https://docs.python.org/3/library/typing.html#typing.ClassVar) annotation, but this might not do what you'd expect:
    
    ```python
    class A:
        y: ClassVar = 0  # Type implicitly Any!
    ```
    
    In this case the type of the attribute will be implicitly `Any`. This behavior will change in the future, since it's surprising.
    
    An explicit [`ClassVar[t]`](https://docs.python.org/3/library/typing.html#typing.ClassVar) may be particularly handy to distinguish between class and instance variables with callable types. For example:

    ```python
    from typing import Callable, ClassVar
    
    class A:
        foo: Callable[[int], None]
        bar: ClassVar[Callable[[A, int], None]]
        bad: Callable[[A], None]
    
    A().foo(42)  # OK
    A().bar(42)  # OK
    A().bad()  # Error: Too few arguments
    ```
    
    !!! info "Note"

        A [`ClassVar[t]`](https://docs.python.org/3/library/typing.html#typing.ClassVar) type parameter cannot include type variables: `ClassVar[T]` and `ClassVar[list[T]]` are both invalid if `T` is a type variable (see [`generic-classes`](https://mypy.readthedocs.io/en/stable/generics.html#generic-classes) for more about type variables).

## 重写静态类型方法

=== "中文"

    当重写静态类型方法时，mypy 检查重写是否具有兼容的签名：

    ```python
    class Base:
        def f(self, x: int) -> None:
            ...
    
    class Derived1(Base):
        def f(self, x: str) -> None:   # Error: type of 'x' incompatible (不兼容)
            ...
    
    class Derived2(Base):
        def f(self, x: int, y: int) -> None:  # Error: too many arguments
            ...
    
    class Derived3(Base):
        def f(self, x: int) -> None:   # OK
            ...
    
    class Derived4(Base):
        def f(self, x: float) -> None:   # OK: mypy 将 int 视为 float 的子类型
            ...
    
    class Derived5(Base):
        def f(self, x: int, y: int = 0) -> None:   # OK: 接受比基类更多参数的方法
    ```
    
    !!! info "Note"
        
        您还可以在重写中**协变**地改变返回类型。 例如，您可以使用 `list[int]` 等子类型覆盖返回类型 `Iterable[int]` 。 类似地，您可以**逆变地**改变参数类型——子类可以有更通用的参数类型。

    
    为了确保重命名方法时代码保持正确，将方法显式标记为覆盖基本方法会很有帮助。 这可以通过 `@override` 装饰器来完成。 如果随后重命名了基本方法，而未重命名重写方法，则 mypy 将显示错误：

    ```python
    from typing import override
    
    class Base:
        def f(self, x: int) -> None:
            ...
        def g_renamed(self, y: str) -> None:
            ...
    
    class Derived1(Base):
        @override
        def f(self, x: int) -> None:   # OK
            ...
    
        @override
        def g(self, y: str) -> None:   # Error: 没有找到对应的基础方法
            ...
    ```

    您还可以使用动态类型方法覆盖静态类型方法。 这允许动态类型代码覆盖库类中定义的方法，而无需担心它们的类型签名。

    一如既往，依赖动态类型代码可能是不安全的。 没有运行时强制方法重写返回与原始返回类型兼容的值，因为注解在运行时不起作用：

    ```python
    class Base:
        def inc(self, x: int) -> int:
            return x + 1
    
    class Derived(Base):
        def inc(self, x):   # Override, dynamically typed
            return 'hello'  # Incompatible with 'Base', but no mypy error
    ```

=== "英文"

    **Overriding statically typed methods**

    When overriding a statically typed method, mypy checks that the override has a compatible signature:

    ```python
    class Base:
        def f(self, x: int) -> None:
            ...
    
    class Derived1(Base):
        def f(self, x: str) -> None:   # Error: type of 'x' incompatible
            ...
    
    class Derived2(Base):
        def f(self, x: int, y: int) -> None:  # Error: too many arguments
            ...
    
    class Derived3(Base):
        def f(self, x: int) -> None:   # OK
            ...
    
    class Derived4(Base):
        def f(self, x: float) -> None:   # OK: mypy treats int as a subtype of float
            ...
    
    class Derived5(Base):
        def f(self, x: int, y: int = 0) -> None:   # OK: accepts more than the base
            ...                                    #     class method
    ```
    
    !!! info "Note"
        
        You can also vary return types **covariantly** in overriding. For example, you could override the return type `Iterable[int]` with a subtype such as `list[int]`. Similarly, you can vary argument types **contravariantly** -- subclasses can have more general argument types.

    
    In order to ensure that your code remains correct when renaming methods, it can be helpful to explicitly mark a method as overriding a base method. This can be done with the `@override` decorator. If the base method is then renamed while the overriding method is not, mypy will show an error:

    ```python
    from typing import override
    
    class Base:
        def f(self, x: int) -> None:
            ...
        def g_renamed(self, y: str) -> None:
            ...
    
    class Derived1(Base):
        @override
        def f(self, x: int) -> None:   # OK
            ...
    
        @override
        def g(self, y: str) -> None:   # Error: no corresponding base method found
            ...
    ```

    You can also override a statically typed method with a dynamically typed one. This allows dynamically typed code to override methods defined in library classes without worrying about their type signatures.

    As always, relying on dynamically typed code can be unsafe. There is no runtime enforcement that the method override returns a value that is compatible with the original return type, since annotations have no effect at runtime:

    ```python
    class Base:
        def inc(self, x: int) -> int:
            return x + 1
    
    class Derived(Base):
        def inc(self, x):   # Override, dynamically typed
            return 'hello'  # Incompatible with 'Base', but no mypy error
    ```

## 抽象基类和多重继承

=== "中文"

    Mypy 支持 Python [`抽象基类`](https://docs.python.org/3/library/abc.html) (ABC)。 抽象类至少有一个抽象方法或属性，必须由任何 **“具体”**（非抽象）子类实现。 您可以使用 [`abc.ABCMeta`](https://docs.python.org/3/library/abc.html#abc.ABCMeta) 元类和 [`@abc.abstractmethod`]( https://docs.python.org/3/library/abc.html#abc.abstractmethod）函数装饰器。 例子：

    ```python
    from abc import ABCMeta, abstractmethod
    
    class Animal(metaclass=ABCMeta):
        @abstractmethod
        def eat(self, food: str) -> None: pass
    
        @property
        @abstractmethod
        def can_walk(self) -> bool: pass
    
    class Cat(Animal):
        def eat(self, food: str) -> None:
            ...  # Body omitted
    
        @property
        def can_walk(self) -> bool:
            return True
    
    x = Animal()  # Error: 'Animal' is abstract due to 'eat' and 'can_walk'
    y = Cat()     # OK
    ```
    
    请注意，即使您省略了 [`abc.ABCMeta`](https://docs.python.org/3/library/abc.html#abc.ABCMeta) 元类，mypy 也会执行未实现的抽象方法的检查。 如果元类会导致运行时元类冲突，这可能很有用。
    
    由于您无法创建 ABC 的实例，因此它们最常用于类型注释中。 例如，此方法接受包含任意动物的任意迭代（具体“Animal”子类的实例）：

    ```python
    def feed_all(animals: Iterable[Animal], food: str) -> None:
        for animal in animals:
            animal.eat(food)
    ```
    
    Python 中 ABC 的工作方式有一个重要的特点——特定类是否是抽象的在某种程度上是隐含的。 在下面的示例中，“Derived”被视为抽象基类，因为“Derived”从“Base”继承了抽象“f”方法，并且没有显式实现它。 `Derived` 的定义不会从 mypy 中生成错误，因为它是有效的 ABC：

    ```python
    from abc import ABCMeta, abstractmethod
    
    class Base(metaclass=ABCMeta):
        @abstractmethod
        def f(self, x: int) -> None: pass
    
    class Derived(Base):  # No error -- Derived 隐式 抽象
        def g(self) -> None:
            ...
    ```
    
    但是，尝试创建 “Derived” 实例将被拒绝：
    
    ```python
    d = Derived()  # Error: 'Derived' is abstract
    ```
    
    !!! info "Note"
    
        忘记实现抽象方法是一个常见的错误。 如上所示，在这种情况下，类定义不会生成错误，但任何构造实例的尝试都将被标记为错误。
    
    Mypy 允许您省略抽象方法的主体，但如果这样做，通过 `super()` 调用此类方法是不安全的。 例如：

    ```python
    from abc import abstractmethod
    class Base:
        @abstractmethod
        def foo(self) -> int: pass
        @abstractmethod
        def bar(self) -> int:
            return 0
    class Sub(Base):
        def foo(self) -> int:
            return super().foo() + 1  # error: 通过 super() 调用带有简单主体的“Base”的抽象方法“foo”是不安全的
        @abstractmethod
        def bar(self) -> int:
            return super().bar() + 1  # 不过这没关系。
    ```
    
    一个类可以继承任意数量的类，包括抽象类和具体类。 与普通重写一样，动态类型方法可以重写或实现任何基类中定义的静态类型方法，包括抽象基类中定义的抽象方法。
    
    您可以使用普通属性或实例变量来实现抽象属性。

=== "英文"

    **Abstract base classes and multiple inheritance**

    Mypy supports Python [`abstract base classes`](https://docs.python.org/3/library/abc.html) (ABCs). Abstract classes have at least one abstract method or property that must be implemented by any *concrete* (non-abstract) subclass. You can define abstract base classes using the [`abc.ABCMeta`](https://docs.python.org/3/library/abc.html#abc.ABCMeta) metaclass and the [`@abc.abstractmethod`](https://docs.python.org/3/library/abc.html#abc.abstractmethod) function decorator. Example:

    ```python
    from abc import ABCMeta, abstractmethod
    
    class Animal(metaclass=ABCMeta):
        @abstractmethod
        def eat(self, food: str) -> None: pass
    
        @property
        @abstractmethod
        def can_walk(self) -> bool: pass
    
    class Cat(Animal):
        def eat(self, food: str) -> None:
            ...  # Body omitted
    
        @property
        def can_walk(self) -> bool:
            return True
    
    x = Animal()  # Error: 'Animal' is abstract due to 'eat' and 'can_walk'
    y = Cat()     # OK
    ```
    
    Note that mypy performs checking for unimplemented abstract methods even if you omit the [`abc.ABCMeta`](https://docs.python.org/3/library/abc.html#abc.ABCMeta) metaclass. This can be useful if the metaclass would cause runtime metaclass conflicts.
    
    Since you can't create instances of ABCs, they are most commonly used in type annotations. For example, this method accepts arbitrary iterables containing arbitrary animals (instances of concrete `Animal` subclasses):

    ```python
    def feed_all(animals: Iterable[Animal], food: str) -> None:
        for animal in animals:
            animal.eat(food)
    ```
    
    There is one important peculiarity about how ABCs work in Python -- whether a particular class is abstract or not is somewhat implicit. In the example below, `Derived` is treated as an abstract base class since `Derived` inherits an abstract `f` method from `Base` and doesn't explicitly implement it. The definition of `Derived` generates no errors from mypy, since it's a valid ABC:

    ```python
    from abc import ABCMeta, abstractmethod
    
    class Base(metaclass=ABCMeta):
        @abstractmethod
        def f(self, x: int) -> None: pass
    
    class Derived(Base):  # No error -- Derived is implicitly abstract
        def g(self) -> None:
            ...
    ```
    
    Attempting to create an instance of `Derived` will be rejected, however:
    
    ```python
    d = Derived()  # Error: 'Derived' is abstract
    ```
    
    !!! info "Note"
    
        It's a common error to forget to implement an abstract method. As shown above, the class definition will not generate an error in this case, but any attempt to construct an instance will be flagged as an error.
    
    Mypy allows you to omit the body for an abstract method, but if you do so, it is unsafe to call such method via `super()`. For example:

    ```python
    from abc import abstractmethod
    class Base:
        @abstractmethod
        def foo(self) -> int: pass
        @abstractmethod
        def bar(self) -> int:
            return 0
    class Sub(Base):
        def foo(self) -> int:
            return super().foo() + 1  # error: Call to abstract method "foo" of "Base"
                                      # with trivial body via super() is unsafe
        @abstractmethod
        def bar(self) -> int:
            return super().bar() + 1  # This is OK however.
    ```
    
    A class can inherit any number of classes, both abstract and concrete. As with normal overrides, a dynamically typed method can override or implement a statically typed method defined in any base class, including an abstract method defined in an abstract base class.
    
    You can implement an abstract property using either a normal property or an instance variable.

## Slots

=== "中文"

    当一个类显式定义了 [\_\_slots\_\_](https://docs.python.org/3/reference/datamodel.html#slots) 时，mypy 将检查分配给的所有属性是否都是 `__slots__` 的成员  ：

    ```python
    class Album:
        __slots__ = ('name', 'year')
    
        def __init__(self, name: str, year: int) -> None:
           self.name = name
           self.year = year
           # Error: Trying to assign name "released" that is not in "__slots__" of type "Album"
           self.released = True
    
    my_album = Album('Songs about Python', 2021)
    ```

    当满足以下条件时，Mypy 将仅检查 `__slots__` 的属性分配：
    
    1. 所有基类（内置基类除外）必须明确定义 `__slots__`（这反映了 Python 语义）。
    2. `__slots__` 不包括 `__dict__`。 如果 `__slots__` 包含 `__dict__` ，则可以设置任意属性，类似于未定义 `__slots__` 时（这反映了 Python 语义）。
    3. `__slots__` 中的所有值都必须是字符串文字。

=== "英文"

    **Slots**

    When a class has explicitly defined [\_\_slots\_\_](https://docs.python.org/3/reference/datamodel.html#slots), mypy will check that all attributes assigned to are members of `__slots__`:

    ```python
    class Album:
        __slots__ = ('name', 'year')
    
        def __init__(self, name: str, year: int) -> None:
           self.name = name
           self.year = year
           # Error: Trying to assign name "released" that is not in "__slots__" of type "Album"
           self.released = True
    
    my_album = Album('Songs about Python', 2021)
    ```

    Mypy will only check attribute assignments against `__slots__` when the following conditions hold:
    
    1. All base classes (except builtin ones) must have explicit `__slots__` defined (this mirrors Python semantics).
    2. `__slots__` does not include `__dict__`. If `__slots__` includes `__dict__`, arbitrary attributes can be set, similar to when `__slots__` is not defined (this mirrors Python semantics).
    3. All values in `__slots__` must be string literals.
