# 泛型

=== "中文"

    本节介绍如何定义自己的泛型类，该泛型类采用一个或多个类型参数，类似于 `list[X]` 等内置类型。 用户定义的泛型是一个相当高级的功能，您可以在不使用它们的情况下走得很远——请随意跳过本节并稍后再回来。

=== "英文"

    **Generics**
    
    This section explains how you can define your own generic classes that take one or more type parameters, similar to built-in types such as `list[X]`. User-defined generics are a moderately advanced feature and you can get far without ever using them -- feel free to skip this section and come back later.

## 定义泛型类

Defining generic classes

=== "中文"

    内置集合类是泛型类。 泛型类型有一个或多个类型参数，可以是任意类型。 例如，`dict[int, str]` 具有类型参数 “int” 和 “str” ，“list[int]” 具有类型参数 “int” 。

    程序还可以定义新的泛型类。 这是一个非常简单的泛型类，表示堆栈：

    ```python
    from typing import TypeVar, Generic

    T = TypeVar('T')

    class Stack(Generic[T]):
        def __init__(self) -> None:
            # Create an empty list with items of type T
            self.items: list[T] = []

        def push(self, item: T) -> None:
            self.items.append(item)

        def pop(self) -> T:
            return self.items.pop()

        def empty(self) -> bool:
            return not self.items
    ```

    Stack 类可用于表示任何类型的堆栈：`Stack[int]`、`Stack[tuple[int, str]]` 等。

    使用 `Stack` 与内置容器类型类似：

    ```python
    # 构造一个空的 Stack[int] 实例
    stack = Stack[int]()
    stack.push(2)
    stack.pop()
    stack.push('x')  # error: Argument 1 to "push" of "Stack" has incompatible type "str"; expected "int"
    ```

    泛型类型实例的构造经过类型检查：

    ```python
    class Box(Generic[T]):
        def __init__(self, content: T) -> None:
            self.content = content

    Box(1)       # OK, inferred type is Box[int]
    Box[int](1)  # Also OK
    Box[int]('some string')  # error: Argument 1 to "Box" has incompatible type "str"; expected "int"
    ```

=== "英文"

    The built-in collection classes are generic classes. Generic types have one or more type parameters, which can be arbitrary types. For example, `dict[int, str]` has the type parameters `int` and `str`, and `list[int]` has a type parameter `int`.

    Programs can also define new generic classes. Here is a very simple generic class that represents a stack:

    ```python
    from typing import TypeVar, Generic

    T = TypeVar('T')

    class Stack(Generic[T]):
        def __init__(self) -> None:
            # Create an empty list with items of type T
            self.items: list[T] = []

        def push(self, item: T) -> None:
            self.items.append(item)

        def pop(self) -> T:
            return self.items.pop()

        def empty(self) -> bool:
            return not self.items
    ```

    The `Stack` class can be used to represent a stack of any type: `Stack[int]`, `Stack[tuple[int, str]]`, etc.

    Using `Stack` is similar to built-in container types:

    ```python
    # Construct an empty Stack[int] instance
    stack = Stack[int]()
    stack.push(2)
    stack.pop()
    stack.push('x')  # error: Argument 1 to "push" of "Stack" has incompatible type "str"; expected "int"
    ```

    Construction of instances of generic types is type checked:

    ```python
    class Box(Generic[T]):
        def __init__(self, content: T) -> None:
            self.content = content

    Box(1)       # OK, inferred type is Box[int]
    Box[int](1)  # Also OK
    Box[int]('some string')  # error: Argument 1 to "Box" has incompatible type "str"; expected "int"
    ```

## 定义泛型类的子类

Defining subclasses of generic classes

=== "中文"

    用户定义的泛型类和 [`typing`](https://docs.python.org/3/library/typing.html#module-typing) 中定义的泛型类可以用作另一个类的基类（泛型 或非泛型）。 例如：

    ```python
    from typing import Generic, TypeVar, Mapping, Iterator

    KT = TypeVar('KT')
    VT = TypeVar('VT')

    # 这是 Mapping 的通用子类
    class MyMap(Mapping[KT, VT]):
        def __getitem__(self, k: KT) -> VT: ...
        def __iter__(self) -> Iterator[KT]: ...
        def __len__(self) -> int: ...

    items: MyMap[str, int]  # OK

    # 这是 dict 的非泛型子类
    class StrDict(dict[str, str]):
        def __str__(self) -> str:
            return f'StrDict({super().__str__()})'


    data: StrDict[int, int]  # Error! StrDict is not generic
    data2: StrDict  # OK

    #这是一个用户定义的泛型类
    class Receiver(Generic[T]):
        def accept(self, value: T) -> None: ...

    # 这是 Receiver 的通用子类
    class AdvancedReceiver(Receiver[T]): ...
    ```

    !!! info "Note"
    
        如果您希望 mypy 将用户定义的类视为映射（以及序列的 [`Sequence`](https://docs.python.org/3/library/typing.html#typing.Sequence) 等），则必须添加显式的 [`Mapping`](https://docs.python.org/3/library/typing.html#typing.Mapping) 基类。 这是因为 mypy 不为这些 ABC 使用“结构子类型”，不像 [`Iterable`](https://docs.python.org/3/library/typing.html#typing.Iterable) 等更简单的协议，它使用 [`结构子类型`](https://mypy.readthedocs.io/en/stable/protocols.html#protocol-types)
    

    如果还有其他包含类型变量的基类，例如上例中的 `Mapping[KT, VT]`，则可以从基类中省略 [`Generic`](https://docs.python.org/3/library/typing.html#typing.Generic)。 如果您在基数中包含“Generic[...]”，那么它应该列出其他基数中存在的所有类型变量（或更多，如果需要）。 类型变量的顺序由以下规则定义：

    - 如果存在“Generic[...]”，则变量的顺序始终由它们在“Generic[...]”中的顺序确定。
    - 如果基数中没有“Generic[...]”，则所有类型变量都按字典顺序收集（即按首次出现）。

    For example:

    ```python
    from typing import Generic, TypeVar, Any

    T = TypeVar('T')
    S = TypeVar('S')
    U = TypeVar('U')

    class One(Generic[T]): ...
    class Another(Generic[T]): ...

    class First(One[T], Another[S]): ...
    class Second(One[T], Another[S], Generic[S, U, T]): ...

    x: First[int, str]        # Here T is bound to int, S is bound to str
    y: Second[int, str, Any]  # Here T is Any, S is int, and U is str
    ```

=== "英文"

    User-defined generic classes and generic classes defined in [`typing`](https://docs.python.org/3/library/typing.html#module-typing) can be used as a base class for another class (generic or non-generic). For example:

    ```python
    from typing import Generic, TypeVar, Mapping, Iterator

    KT = TypeVar('KT')
    VT = TypeVar('VT')

    # This is a generic subclass of Mapping
    class MyMap(Mapping[KT, VT]):
        def __getitem__(self, k: KT) -> VT: ...
        def __iter__(self) -> Iterator[KT]: ...
        def __len__(self) -> int: ...

    items: MyMap[str, int]  # OK

    # This is a non-generic subclass of dict
    class StrDict(dict[str, str]):
        def __str__(self) -> str:
            return f'StrDict({super().__str__()})'


    data: StrDict[int, int]  # Error! StrDict is not generic
    data2: StrDict  # OK

    # This is a user-defined generic class
    class Receiver(Generic[T]):
        def accept(self, value: T) -> None: ...

    # This is a generic subclass of Receiver
    class AdvancedReceiver(Receiver[T]): ...
    ```

    !!! info "Note"
    
        You have to add an explicit [`Mapping`](https://docs.python.org/3/library/typing.html#typing.Mapping) base class if you want mypy to consider a user-defined class as a mapping (and [`Sequence`](https://docs.python.org/3/library/typing.html#typing.Sequence) for sequences, etc.). This is because mypy doesn't use *structural subtyping* for these ABCs, unlike simpler protocols like [`Iterable`](https://docs.python.org/3/library/typing.html#typing.Iterable), which use [`structural subtyping`](https://mypy.readthedocs.io/en/stable/protocols.html#protocol-types).
    

    [`Generic`](https://docs.python.org/3/library/typing.html#typing.Generic) can be omitted from bases if there are other base classes that include type variables, such as `Mapping[KT, VT]` in the above example. If you include `Generic[...]` in bases, then it should list all type variables present in other bases (or more, if needed). The order of type variables is defined by the following rules:

    - If `Generic[...]` is present, then the order of variables is always determined by their order in `Generic[...]`.
    - If there are no `Generic[...]` in bases, then all type variables are collected in the lexicographic order (i.e. by first appearance).

    For example:

    ```python
    from typing import Generic, TypeVar, Any

    T = TypeVar('T')
    S = TypeVar('S')
    U = TypeVar('U')

    class One(Generic[T]): ...
    class Another(Generic[T]): ...

    class First(One[T], Another[S]): ...
    class Second(One[T], Another[S], Generic[S, U, T]): ...

    x: First[int, str]        # Here T is bound to int, S is bound to str
    y: Second[int, str, Any]  # Here T is Any, S is int, and U is str
    ```

## 泛型函数

Generic functions

=== "中文"

    类型变量可用于定义泛型函数：

    ```python
    from typing import TypeVar, Sequence

    T = TypeVar('T')

    # A generic function!
    def first(seq: Sequence[T]) -> T:
        return seq[0]
    ```

    与泛型类一样，类型变量可以替换为任何类型。 这意味着“first”可以与任何序列类型一起使用，并且返回类型派生自序列项类型。 例如：

    ```python
    reveal_type(first([1, 2, 3]))   # Revealed type is "builtins.int"
    reveal_type(first(['a', 'b']))  # Revealed type is "builtins.str"
    ```

    另请注意，类型变量的单个定义（例如上面的“T”）可以在多个泛型函数或类中使用。 在此示例中，我们在两个泛型函数中使用相同的类型变量：

    ```python
    from typing import TypeVar, Sequence

    T = TypeVar('T')      # 声明类型变量

    def first(seq: Sequence[T]) -> T:
        return seq[0]

    def last(seq: Sequence[T]) -> T:
        return seq[-1]
    ```

    变量的类型中不能包含类型变量，除非该类型变量绑定在包含泛型类或函数中。

=== "英文"

    Type variables can be used to define generic functions:

    ```python
    from typing import TypeVar, Sequence

    T = TypeVar('T')

    # A generic function!
    def first(seq: Sequence[T]) -> T:
        return seq[0]
    ```

    As with generic classes, the type variable can be replaced with any type. That means `first` can be used with any sequence type, and the return type is derived from the sequence item type. For example:

    ```python
    reveal_type(first([1, 2, 3]))   # Revealed type is "builtins.int"
    reveal_type(first(['a', 'b']))  # Revealed type is "builtins.str"
    ```

    Note also that a single definition of a type variable (such as `T` above) can be used in multiple generic functions or classes. In this example we use the same type variable in two generic functions:

    ```python
    from typing import TypeVar, Sequence

    T = TypeVar('T')      # Declare type variable

    def first(seq: Sequence[T]) -> T:
        return seq[0]

    def last(seq: Sequence[T]) -> T:
        return seq[-1]
    ```

    A variable cannot have a type variable in its type unless the type variable is bound in a containing generic class or function.

## 泛型方法和自身泛型

Generic methods and generic self

=== "中文"

    您还可以定义泛型方法 - 只需在方法签名中使用与类类型变量不同的类型变量。 特别是，“self” 参数也可以是通用的，允许方法返回访问点已知的最精确的类型。 例如，您可以通过这种方式检查一系列 setter 方法：

    ```python
    from typing import TypeVar

    T = TypeVar('T', bound='Shape')

    class Shape:
        def set_scale(self: T, scale: float) -> T:
            self.scale = scale
            return self

    class Circle(Shape):
        def set_radius(self, r: float) -> 'Circle':
            self.radius = r
            return self

    class Square(Shape):
        def set_width(self, w: float) -> 'Square':
            self.width = w
            return self

    circle: Circle = Circle().set_scale(0.5).set_radius(2.7)
    square: Square = Square().set_scale(0.5).set_width(3.2)
    ```

    如果不使用通用的 “self” ，最后两行将无法正确进行类型检查，因为 “set_scale” 的返回类型将是 “Shape” ，而它没有定义 “set_radius” 或 “set_width” 。

    其他用途是工厂方法，例如复制和反序列化。 对于类方法，您还可以使用 [`Type[T]`](https://docs.python.org/3/library/typing.html#typing.Type) 定义通用 `cls`

    ```python
    from typing import TypeVar, Type

    T = TypeVar('T', bound='Friend')

    class Friend:
        other: "Friend" = None

        @classmethod
        def make_pair(cls: Type[T]) -> tuple[T, T]:
            a, b = cls(), cls()
            a.other = b
            b.other = a
            return a, b

    class SuperFriend(Friend):
        pass

    a, b = SuperFriend.make_pair()
    ```

    请注意，当使用通用“self”重写方法时，您也必须返回通用“self”，或者返回当前类的实例。 在后一种情况下，您必须在所有未来的子类中实现此方法。

    另请注意，mypy 无法始终验证副本或反序列化方法的实现是否返回 self 的实际类型。 因此，您可能需要在这些方法内（但不是在调用站点）静默 mypy，可能通过使用 “Any” 类型或 “# type:ignore” 注释。

    请注意，mypy 允许您以某些不安全的方式使用通用自身类型，以支持常见的习惯用法。 例如，在参数类型中使用通用自类型是可以接受的，即使它不安全：

    ```python
    from typing import TypeVar

    T = TypeVar("T")

    class Base:
        def compare(self: T, other: T) -> bool:
            return False

    class Sub(Base):
        def __init__(self, x: int) -> None:
            self.x = x

        # 这是不安全的（见下文），但允许，因为这是一种常见模式，并且在实践中很少引起问题。
        def compare(self, other: Sub) -> bool:
            return self.x > other.x

    b: Base = Sub(42)
    b.compare(Base())  # 这里运行时错误: 'Base' object has no attribute 'x'
    ```

    有关 self 类型的一些高级用法，请参阅 [`additional examples`](./more_types.md#self类型的高级用法).

=== "英文"

    You can also define generic methods — just use a type variable in the method signature that is different from class type variables. In particular, the `self` argument may also be generic, allowing a method to return the most precise type known at the point of access. In this way, for example, you can type check a chain of setter methods:

    ```python
    from typing import TypeVar

    T = TypeVar('T', bound='Shape')

    class Shape:
        def set_scale(self: T, scale: float) -> T:
            self.scale = scale
            return self

    class Circle(Shape):
        def set_radius(self, r: float) -> 'Circle':
            self.radius = r
            return self

    class Square(Shape):
        def set_width(self, w: float) -> 'Square':
            self.width = w
            return self

    circle: Circle = Circle().set_scale(0.5).set_radius(2.7)
    square: Square = Square().set_scale(0.5).set_width(3.2)
    ```

    Without using generic `self`, the last two lines could not be type checked properly, since the return type of `set_scale` would be `Shape`, which doesn't define `set_radius` or `set_width`.

    Other uses are factory methods, such as copy and deserialization. For class methods, you can also define generic `cls`, using [`Type[T]`](https://docs.python.org/3/library/typing.html#typing.Type):

    ```python
    from typing import TypeVar, Type

    T = TypeVar('T', bound='Friend')

    class Friend:
        other: "Friend" = None

        @classmethod
        def make_pair(cls: Type[T]) -> tuple[T, T]:
            a, b = cls(), cls()
            a.other = b
            b.other = a
            return a, b

    class SuperFriend(Friend):
        pass

    a, b = SuperFriend.make_pair()
    ```

    Note that when overriding a method with generic `self`, you must either return a generic `self` too, or return an instance of the current class. In the latter case, you must implement this method in all future subclasses.

    Note also that mypy cannot always verify that the implementation of a copy or a deserialization method returns the actual type of self. Therefore you may need to silence mypy inside these methods (but not at the call site), possibly by making use of the `Any` type or a `# type: ignore` comment.

    Note that mypy lets you use generic self types in certain unsafe ways in order to support common idioms. For example, using a generic self type in an argument type is accepted even though it's unsafe:

    ```python
    from typing import TypeVar

    T = TypeVar("T")

    class Base:
        def compare(self: T, other: T) -> bool:
            return False

    class Sub(Base):
        def __init__(self, x: int) -> None:
            self.x = x

        # This is unsafe (see below) but allowed because it's
        # a common pattern and rarely causes issues in practice.
        def compare(self, other: Sub) -> bool:
            return self.x > other.x

    b: Base = Sub(42)
    b.compare(Base())  # Runtime error here: 'Base' object has no attribute 'x'
    ```

    For some advanced uses of self types, see [`additional examples`](./more_types.md#self类型的高级用法).

## 使用 Typing.Self 自动标注self类型

Automatic self types using typing.Self

=== "中文"

    由于上述模式非常常见，因此 mypy 支持 [`PEP-673`](https://peps.python.org/pep-0673/) 中引入的更简单的语法，以使它们更易于使用。 您可以导入特殊类型“typing.Self”，而不是定义类型变量并为“self”使用显式注释，该类型会自动转换为以当前类为上限的类型变量，并且不需要 “self”（或类方法中的“cls”）的注释。 使用 `Self` 可以使上一节的示例变得更简单：

    ```python
    from typing import Self

    class Friend:
        other: Self | None = None

        @classmethod
        def make_pair(cls) -> tuple[Self, Self]:
            a, b = cls(), cls()
            a.other = b
            b.other = a
            return a, b

    class SuperFriend(Friend):
        pass

    a, b = SuperFriend.make_pair()
    ```

    这比使用显式类型变量更紧凑。 此外，除了方法之外，您还可以在属性注释中使用“Self”。

    !!! info "Note"

        要在早于 3.11 的 Python 版本上使用此功能，您需要从 `typing_extensions`（版本 4.0 或更高版本）导入 `Self`。

=== "英文"

    Since the patterns described above are quite common, mypy supports a simpler syntax, introduced in [`PEP-673`](https://peps.python.org/pep-0673/), to make them easier to use. Instead of defining a type variable and using an explicit annotation for `self`, you can import the special type `typing.Self` that is automatically transformed into a type variable with the current class as the upper bound, and you don't need an annotation for `self` (or `cls` in class methods). The example from the previous section can be made simpler by using `Self`:

    ```python
    from typing import Self

    class Friend:
        other: Self | None = None

        @classmethod
        def make_pair(cls) -> tuple[Self, Self]:
            a, b = cls(), cls()
            a.other = b
            b.other = a
            return a, b

    class SuperFriend(Friend):
        pass

    a, b = SuperFriend.make_pair()
    ```

    This is more compact than using explicit type variables. Also, you can use `Self` in attribute annotations in addition to methods.

    !!! info "Note"

        To use this feature on Python versions earlier than 3.11, you will need to import `Self` from `typing_extensions` (version 4.0 or newer).

## 泛型类型的变体

Variance of generic types

=== "中文"

    就泛型类型之间的子类型关系而言，泛型类型主要分为三种：不变、协变和逆变。 假设我们有一对类型“A”和“B”，并且“B”是“A”的子类型，它们的定义如下：

    - 如果 `MyCovGen[B]` 始终是 `MyCovGen[A]` 的子类型，则泛型类 `MyCovGen[T]` 中的类型变量 `T` 称为协变。
    - 如果 `MyContraGen[A]` 始终是 `MyContraGen[B]` 的子类型，则泛型类 `MyContraGen[T]` 中的类型变量 `T` 称为逆变。
    - 如果上述两个条件都不成立，则泛型类 “MyInvGen[T]” 中的 “T” 称为不变式。

    让我们通过几个简单的例子来说明这一点：

    ```python
    # 我们将在下面的示例中使用这些类
    class Shape: ...
    class Triangle(Shape): ...
    class Square(Shape): ...
    ```

    - 大多数不可变容器，例如 [`Sequence`](https://docs.python.org/3/library/typing.html#typing.Sequence) 和 [`FrozenSet`](https://docs.python.org/3/library/typing.html#typing.FrozenSet) 是协变的. [`Union`](https://docs.python.org/3/library/typing.html#typing.Union) 在所有变量中也是协变的： `Union[Triangle, int]` 是 `Union[Shape, int]` 的子类型。

    ```python
    def count_lines(shapes: Sequence[Shape]) -> int:
        return sum(shape.num_sides for shape in shapes)

    triangles: Sequence[Triangle]
    count_lines(triangles)  # OK

    def foo(triangle: Triangle, num: int):
        shape_or_number: Union[Shape, int]
        # 三角形是形状，形状是有效的 Union[Shape, int]
        shape_or_number = triangle
    ```

    协变应该相对直观，但逆变和不变可能更难推理。

    - [`Callable`](https://docs.python.org/3/library/typing.html#typing.Callable) 是在参数类型中表现逆变的类型示例。 也就是说，尽管“Shape”是“Triangle”的超类型，但 “Callable[[Shape], int]” 是 “Callable[[Triangle], int]” 的子类型。 要理解这一点，请考虑：

    ```python
    def cost_of_paint_required(
        triangle: Triangle,
        area_calculator: Callable[[Triangle], float]
    ) -> float:
        return area_calculator(triangle) * DOLLAR_PER_SQ_FT

    # 这直接有效
    def area_of_triangle(triangle: Triangle) -> float: ...
    cost_of_paint_required(triangle, area_of_triangle)  # OK

    # 但这也有效！
    def area_of_any_shape(shape: Shape) -> float: ...
    cost_of_paint_required(triangle, area_of_any_shape)  # OK
    ```

    `cost_of_paint_required` 需要一个可以计算三角形面积的可调用函数。 如果我们给它一个可以计算任意形状（不仅仅是三角形）面积的可调用函数，一切仍然有效。

    - [`List`](https://docs.python.org/3/library/typing.html#typing.List) 是不变的泛型类型。 天真的人会认为它是协变的，就像上面的 [`Sequence`](https://docs.python.org/3/library/typing.html#typing.Sequence) 一样，但请考虑以下代码：

    ```python
    class Circle(Shape):
        # 旋转方法仅在 Circle 上定义，在 Shape 上没有定义
        def rotate(self): ...

    def add_one(things: list[Shape]) -> None:
        things.append(Shape())

    my_circles: list[Circle] = []
    add_one(my_circles)     # 这可能看起来很安全，但是......
    my_circles[-1].rotate()  # ...这将会失败，因为 my_circles[0] 现在是一个 Shape，而不是一个 Circle
    ```

    不变类型的另一个例子是 [`Dict`](https://docs.python.org/3/library/typing.html#typing.Dict)。 大多数可变容器是不变的。

    默认情况下，mypy 假定所有用户定义的泛型都是不变的。 要将给定的泛型类声明为协变或逆变，请使用使用特殊关键字参数 “covariant” 或 “contravariant” 定义的类型变量。 例如：

    ```python
    from typing import Generic, TypeVar

    T_co = TypeVar('T_co', covariant=True)

    class Box(Generic[T_co]):  # 该类型被声明为协变的
        def __init__(self, content: T_co) -> None:
            self._content = content

        def get_content(self) -> T_co:
            return self._content

    def look_into(box: Box[Animal]): ...

    my_box = Box(Cat())
    look_into(my_box)  # 好的，但是 mypy 会在这里抱怨不变类型
    ```

=== "英文"

    There are three main kinds of generic types with respect to subtype relations between them: invariant, covariant, and contravariant. Assuming that we have a pair of types `A` and `B`, and `B` is a subtype of `A`, these are defined as follows:

    - A generic class `MyCovGen[T]` is called covariant in type variable `T` if `MyCovGen[B]` is always a subtype of `MyCovGen[A]`.
    - A generic class `MyContraGen[T]` is called contravariant in type variable `T` if `MyContraGen[A]` is always a subtype of `MyContraGen[B]`.
    - A generic class `MyInvGen[T]` is called invariant in `T` if neither of the above is true.

    Let us illustrate this by few simple examples:

    ```python
    # We'll use these classes in the examples below
    class Shape: ...
    class Triangle(Shape): ...
    class Square(Shape): ...
    ```

    - Most immutable containers, such as [`Sequence`](https://docs.python.org/3/library/typing.html#typing.Sequence) and [`FrozenSet`](https://docs.python.org/3/library/typing.html#typing.FrozenSet) are covariant. [`Union`](https://docs.python.org/3/library/typing.html#typing.Union) is also covariant in all variables: `Union[Triangle, int]` is a subtype of `Union[Shape, int]`.

    ```python
    def count_lines(shapes: Sequence[Shape]) -> int:
        return sum(shape.num_sides for shape in shapes)

    triangles: Sequence[Triangle]
    count_lines(triangles)  # OK

    def foo(triangle: Triangle, num: int):
        shape_or_number: Union[Shape, int]
        # a Triangle is a Shape, and a Shape is a valid Union[Shape, int]
        shape_or_number = triangle
    ```

    Covariance should feel relatively intuitive, but contravariance and invariance can be harder to reason about.

    - [`Callable`](https://docs.python.org/3/library/typing.html#typing.Callable) is an example of type that behaves contravariant in types of arguments. That is, `Callable[[Shape], int]` is a subtype of `Callable[[Triangle], int]`, despite `Shape` being a supertype of `Triangle`. To understand this, consider:

    ```python
    def cost_of_paint_required(
        triangle: Triangle,
        area_calculator: Callable[[Triangle], float]
    ) -> float:
        return area_calculator(triangle) * DOLLAR_PER_SQ_FT

    # This straightforwardly works
    def area_of_triangle(triangle: Triangle) -> float: ...
    cost_of_paint_required(triangle, area_of_triangle)  # OK

    # But this works as well!
    def area_of_any_shape(shape: Shape) -> float: ...
    cost_of_paint_required(triangle, area_of_any_shape)  # OK
    ```

    `cost_of_paint_required` needs a callable that can calculate the area of a triangle. If we give it a callable that can calculate the area of an arbitrary shape (not just triangles), everything still works.

    - [`List`](https://docs.python.org/3/library/typing.html#typing.List) is an invariant generic type. Naively, one would think that it is covariant, like [`Sequence`](https://docs.python.org/3/library/typing.html#typing.Sequence) above, but consider this code:

    ```python
    class Circle(Shape):
        # The rotate method is only defined on Circle, not on Shape
        def rotate(self): ...

    def add_one(things: list[Shape]) -> None:
        things.append(Shape())

    my_circles: list[Circle] = []
    add_one(my_circles)     # This may appear safe, but...
    my_circles[-1].rotate()  # ...this will fail, since my_circles[0] is now a Shape, not a Circle
    ```

    Another example of invariant type is [`Dict`](https://docs.python.org/3/library/typing.html#typing.Dict). Most mutable containers are invariant.

    By default, mypy assumes that all user-defined generics are invariant. To declare a given generic class as covariant or contravariant use type variables defined with special keyword arguments `covariant` or `contravariant`. For example:

    ```python
    from typing import Generic, TypeVar

    T_co = TypeVar('T_co', covariant=True)

    class Box(Generic[T_co]):  # this type is declared covariant
        def __init__(self, content: T_co) -> None:
            self._content = content

        def get_content(self) -> T_co:
            return self._content

    def look_into(box: Box[Animal]): ...

    my_box = Box(Cat())
    look_into(my_box)  # OK, but mypy would complain here for an invariant type
    ```

## 具有绑定的类型变量

Type variables with upper bounds

=== "中文"

    类型变量还可以被限制为具有特定类型的子类型的值。 该类型称为**类型变量的上限**，并使用 [`TypeVar`](https://docs.python.org/3/library/typing.html) 的 `bound=...` 关键字参数指定

    ```python
    from typing import TypeVar, SupportsAbs

    T = TypeVar('T', bound=SupportsAbs[float])
    ```

    在使用此类类型变量 `T` 的泛型函数的定义中，假定 `T` 表示的类型是其上限的子类型，因此该函数可以对类型 `T` 的值使用上限的方法。

    ```python
    def largest_in_absolute_value(*xs: T) -> T:
        return max(xs, key=abs)  # OK，因为 T 是 SupportsAbs[float] 的子类型。
    ```

    在调用此类函数时，类型“T”必须替换为其上限的子类型。 继续上面的例子：

    ```python
    largest_in_absolute_value(-3.5, 2)   # Okay, 具有 float 类型。
    largest_in_absolute_value(5+6j, 7)   # Okay, 具有复数类型。
    largest_in_absolute_value('a', 'b')  # Error: 'str' is not a subtype of SupportsAbs[float].
    ```

    泛型类的类型参数也可能有上限，它以相同的方式限制类型参数的有效值。

=== "英文"

    A type variable can also be restricted to having values that are subtypes of a specific type. This type is called the upper bound of the type variable, and is specified with the `bound=...` keyword argument to [`TypeVar`](https://docs.python.org/3/library/typing.html#typing.TypeVar).

    ```python
    from typing import TypeVar, SupportsAbs

    T = TypeVar('T', bound=SupportsAbs[float])
    ```

    In the definition of a generic function that uses such a type variable `T`, the type represented by `T` is assumed to be a subtype of its upper bound, so the function can use methods of the upper bound on values of type `T`.

    ```python
    def largest_in_absolute_value(*xs: T) -> T:
        return max(xs, key=abs)  # Okay, because T is a subtype of SupportsAbs[float].
    ```

    In a call to such a function, the type `T` must be replaced by a type that is a subtype of its upper bound. Continuing the example above:

    ```python
    largest_in_absolute_value(-3.5, 2)   # Okay, has type float.
    largest_in_absolute_value(5+6j, 7)   # Okay, has type complex.
    largest_in_absolute_value('a', 'b')  # Error: 'str' is not a subtype of SupportsAbs[float].
    ```

    Type parameters of generic classes may also have upper bounds, which restrict the valid values for the type parameter in the same way.

## 具有值限制的类型变量

Type variables with value restriction

=== "中文"

    默认情况下，类型变量可以替换为任何类型。 但是，有时使用只能将某些特定类型作为其值的类型变量很有用。 一个典型的例子是类型变量只能有值 “str” 和 “bytes” ：

    ```python
    from typing import TypeVar

    AnyStr = TypeVar('AnyStr', str, bytes)
    ```

    这实际上是一个常见的类型变量，[`AnyStr`](https://docs.python.org/3/library/typing.html#typing.AnyStr) 在 [`typing`](https:// docs.python.org/3/library/typing.html#module-typing)中，我们不需要自己定义它。

    我们可以使用 [`AnyStr`](https://docs.python.org/3/library/typing.html#typing.AnyStr) 定义一个可以连接两个字符串或字节对象的函数，但它不能使用其他参数类型调用：

    ```python
    from typing import AnyStr

    def concat(x: AnyStr, y: AnyStr) -> AnyStr:
        return x + y

    concat('a', 'b')    # Okay
    concat(b'a', b'b')  # Okay
    concat(1, 2)        # Error!
    ```

    重要的是，这与联合类型不同，因为不接受“str”和“bytes”的组合：

    ```python
    concat('string', b'bytes')   # Error!
    ```

    在这种情况下，这正是我们想要的，因为不可能连接字符串和字节对象！ 如果我们尝试使用 “Union” ，类型检查器会抱怨这种可能性：

    ```python
    def union_concat(x: Union[str, bytes], y: Union[str, bytes]) -> Union[str, bytes]:
        return x + y  # Error: can't concatenate str and bytes
    ```

    另一个有趣的特殊情况是使用 “str” 的子类型调用 “concat()” ：

    ```python
    class S(str): pass

    ss = concat(S('foo'), S('bar'))
    reveal_type(ss)  # 显示的类型是“builtins.str”
    ```

    您可能认为“ss”的类型是“S”，但实际上类型是“str”：子类型被提升为类型变量的有效值之一，在本例中是“str”。

    因此，这与 Java 等语言中的“有界量化” 略有不同，其中返回类型为 “S”。 mypy 实现这一点的方式对于 `concat` 来说是正确的，因为在上面的示例中，`concat` 实际上返回了一个 `str` 实例：

    ```python
    >>> print(type(ss))
    <class 'str'>
    ```

    在定义泛型类时，您还可以将 [`TypeVar`](https://docs.python.org/3/library/typing.html#typing.TypeVar) 与一组有限的可能值一起使用。 例如，mypy 使用类型 [`Pattern[AnyStr]`](https://docs.python.org/3/library/typing.html#typing.Pattern) 作为 [`re.compile`] 的返回值 （https://docs.python.org/3/library/re.html#re.compile），因为正则表达式可以基于字符串或字节模式。

    类型变量不能同时具有值限制（请参阅 [`type-variable-upper-bound`](https://mypy.readthedocs.io/en/stable/generics.html#type-variable-upper-bound) ）和上限。

=== "英文"

    By default, a type variable can be replaced with any type. However, sometimes it's useful to have a type variable that can only have some specific types as its value. A typical example is a type variable that can only have values `str` and `bytes`:

    ```python
    from typing import TypeVar

    AnyStr = TypeVar('AnyStr', str, bytes)
    ```

    This is actually such a common type variable that [`AnyStr`](https://docs.python.org/3/library/typing.html#typing.AnyStr) is defined in [`typing`](https://docs.python.org/3/library/typing.html#module-typing) and we don't need to define it ourselves.

    We can use [`AnyStr`](https://docs.python.org/3/library/typing.html#typing.AnyStr) to define a function that can concatenate two strings or bytes objects, but it can't be called with other argument types:

    ```python
    from typing import AnyStr

    def concat(x: AnyStr, y: AnyStr) -> AnyStr:
        return x + y

    concat('a', 'b')    # Okay
    concat(b'a', b'b')  # Okay
    concat(1, 2)        # Error!
    ```

    Importantly, this is different from a union type, since combinations of `str` and `bytes` are not accepted:

    ```python
    concat('string', b'bytes')   # Error!
    ```

    In this case, this is exactly what we want, since it's not possible to concatenate a string and a bytes object! If we tried to use `Union`, the type checker would complain about this possibility:

    ```python
    def union_concat(x: Union[str, bytes], y: Union[str, bytes]) -> Union[str, bytes]:
        return x + y  # Error: can't concatenate str and bytes
    ```

    Another interesting special case is calling `concat()` with a subtype of `str`:

    ```python
    class S(str): pass

    ss = concat(S('foo'), S('bar'))
    reveal_type(ss)  # Revealed type is "builtins.str"
    ```

    You may expect that the type of `ss` is `S`, but the type is actually `str`: a subtype gets promoted to one of the valid values for the type variable, which in this case is `str`.

    This is thus subtly different from *bounded quantification* in languages such as Java, where the return type would be `S`. The way mypy implements this is correct for `concat`, since `concat` actually returns a `str` instance in the above example:

    ```python
    >>> print(type(ss))
    <class 'str'>
    ```

    You can also use a [`TypeVar`](https://docs.python.org/3/library/typing.html#typing.TypeVar) with a restricted set of possible values when defining a generic class. For example, mypy uses the type [`Pattern[AnyStr]`](https://docs.python.org/3/library/typing.html#typing.Pattern) for the return value of [`re.compile`](https://docs.python.org/3/library/re.html#re.compile), since regular expressions can be based on a string or a bytes pattern.

    A type variable may not have both a value restriction (see [`type-variable-upper-bound`](https://mypy.readthedocs.io/en/stable/generics.html#type-variable-upper-bound)) and an upper bound.

## 声明装饰器

Declaring decorators

=== "中文"

    装饰器通常是采用一个函数作为参数并返回另一个函数的函数。 用类型来描述这种行为可能有点棘手。 我们将展示如何使用 “TypeVar” 和一种称为 “参数规范”(parameter specification) 的特殊类型变量来执行此操作。

    假设我们有以下装饰器，尚未进行类型注释，它保留原始函数的签名并仅打印装饰函数的名称：

    ```python
    def printing_decorator(func):
        def wrapper(*args, **kwds):
            print("Calling", func)
            return func(*args, **kwds)
        return wrapper
    ```

    我们用它来装饰函数 `add_forty_two`：

    ```python
    # 一个装饰函数。
    @printing_decorator
    def add_forty_two(value: int) -> int:
        return value + 42

    a = add_forty_two(3)
    ```

    由于 “printing_decorator” 没有类型注释，因此以下内容不会进行类型检查：

    ```python
    reveal_type(a)        # Revealed type is "Any"
    add_forty_two('foo')  # 无类型检查器错误 :(
    ```

    这是令人遗憾的事态！ 如果您使用 “--strict” 运行，mypy 甚至会提醒您以下事实：“无类型装饰器使函数 “add_forty_two” 无类型

    请注意，类装饰器的处理方式与 mypy 中的函数装饰器不同：装饰类不会删除其类型，即使装饰器具有不完整的类型注解。

    以下是注解装饰器的方法：

    ```python
    from typing import Any, Callable, TypeVar, cast

    F = TypeVar('F', bound=Callable[..., Any])

    # 保留签名的装饰器。
    def printing_decorator(func: F) -> F:
        def wrapper(*args, **kwds):
            print("Calling", func)
            return func(*args, **kwds)
        return cast(F, wrapper)

    @printing_decorator
    def add_forty_two(value: int) -> int:
        return value + 42

    a = add_forty_two(3)
    reveal_type(a)      # Revealed type is "builtins.int"
    add_forty_two('x')  # Argument 1 to "add_forty_two" has incompatible type "str"; expected "int"
    ```

    这仍然存在一些缺陷。 首先，我们需要使用不安全的 [`cast`](https://docs.python.org/3/library/typing.html#typing.cast) 来说服 mypy `wrapper()` 与 `func` 具有相同的签名。 请参阅 [`casts`](./type_narrowing.md#转换)。

    其次，“wrapper()”函数没有进行严格的类型检查，尽管包装函数通常足够小，这不是一个大问题。 这也是在 `printing_decorator()` 的 `return` 语句中调用 [`cast`](https://docs.python.org/3/library/typing.html#typing.cast) 的原因。

    但是，我们可以使用参数规范（[`ParamSpec`](https://docs.python.org/3/library/typing.html#typing.ParamSpec)）来获得更忠实的类型注释：

    ```python
    from typing import Callable, TypeVar
    from typing_extensions import ParamSpec

    P = ParamSpec('P')
    T = TypeVar('T')

    def printing_decorator(func: Callable[P, T]) -> Callable[P, T]:
        def wrapper(*args: P.args, **kwds: P.kwargs) -> T:
            print("Calling", func)
            return func(*args, **kwds)
        return wrapper
    ```

    参数规范还允许您描述改变输入函数签名的装饰器：

    ```python
    from typing import Callable, TypeVar
    from typing_extensions import ParamSpec

    P = ParamSpec('P')
    T = TypeVar('T')

    # 我们在返回类型中重用“P”，但将“T”替换为“str”
    def stringify(func: Callable[P, T]) -> Callable[P, str]:
        def wrapper(*args: P.args, **kwds: P.kwargs) -> str:
            return str(func(*args, **kwds))
        return wrapper

    @stringify
    def add_forty_two(value: int) -> int:
        return value + 42

    a = add_forty_two(3)
    reveal_type(a)      # Revealed type is "builtins.str"
    add_forty_two('x')  # error: Argument 1 to "add_forty_two" has incompatible type "str"; expected "int"
    ```

    或者插入一个参数：

    ```python
    from typing import Callable, TypeVar
    from typing_extensions import Concatenate, ParamSpec

    P = ParamSpec('P')
    T = TypeVar('T')

    def printing_decorator(func: Callable[P, T]) -> Callable[Concatenate[str, P], T]:
        def wrapper(msg: str, /, *args: P.args, **kwds: P.kwargs) -> T:
            print("Calling", func, "with", msg)
            return func(*args, **kwds)
        return wrapper

    @printing_decorator
    def add_forty_two(value: int) -> int:
        return value + 42

    a = add_forty_two('three', 3)
    ```

=== "英文"

    Decorators are typically functions that take a function as an argument and return another function. Describing this behaviour in terms of types can be a little tricky; we'll show how you can use `TypeVar` and a special kind of type variable called a *parameter specification* to do so.

    Suppose we have the following decorator, not type annotated yet, that preserves the original function's signature and merely prints the decorate function's name:

    ```python
    def printing_decorator(func):
        def wrapper(*args, **kwds):
            print("Calling", func)
            return func(*args, **kwds)
        return wrapper
    ```

    and we use it to decorate function `add_forty_two`:

    ```python
    # A decorated function.
    @printing_decorator
    def add_forty_two(value: int) -> int:
        return value + 42

    a = add_forty_two(3)
    ```

    Since `printing_decorator` is not type-annotated, the following won't get type checked:

    ```python
    reveal_type(a)        # Revealed type is "Any"
    add_forty_two('foo')  # No type checker error :(
    ```

    This is a sorry state of affairs! If you run with `--strict`, mypy will even alert you to this fact: `Untyped decorator makes function "add_forty_two" untyped`

    Note that class decorators are handled differently than function decorators in mypy: decorating a class does not erase its type, even if the decorator has incomplete type annotations.

    Here's how one could annotate the decorator:

    ```python
    from typing import Any, Callable, TypeVar, cast

    F = TypeVar('F', bound=Callable[..., Any])

    # A decorator that preserves the signature.
    def printing_decorator(func: F) -> F:
        def wrapper(*args, **kwds):
            print("Calling", func)
            return func(*args, **kwds)
        return cast(F, wrapper)

    @printing_decorator
    def add_forty_two(value: int) -> int:
        return value + 42

    a = add_forty_two(3)
    reveal_type(a)      # Revealed type is "builtins.int"
    add_forty_two('x')  # Argument 1 to "add_forty_two" has incompatible type "str"; expected "int"
    ```

    This still has some shortcomings. First, we need to use the unsafe [`cast`](https://docs.python.org/3/library/typing.html#typing.cast) to convince mypy that `wrapper()` has the same signature as `func`. See [`casts`](./type_narrowing.md#转换).

    Second, the `wrapper()` function is not tightly type checked, although wrapper functions are typically small enough that this is not a big problem. This is also the reason for the [`cast`](https://docs.python.org/3/library/typing.html#typing.cast) call in the `return` statement in `printing_decorator()`.

    However, we can use a parameter specification ([`ParamSpec`](https://docs.python.org/3/library/typing.html#typing.ParamSpec)), for a more faithful type annotation:

    ```python
    from typing import Callable, TypeVar
    from typing_extensions import ParamSpec

    P = ParamSpec('P')
    T = TypeVar('T')

    def printing_decorator(func: Callable[P, T]) -> Callable[P, T]:
        def wrapper(*args: P.args, **kwds: P.kwargs) -> T:
            print("Calling", func)
            return func(*args, **kwds)
        return wrapper
    ```

    Parameter specifications also allow you to describe decorators that alter the signature of the input function:

    ```python
    from typing import Callable, TypeVar
    from typing_extensions import ParamSpec

    P = ParamSpec('P')
    T = TypeVar('T')

    # We reuse 'P' in the return type, but replace 'T' with 'str'
    def stringify(func: Callable[P, T]) -> Callable[P, str]:
        def wrapper(*args: P.args, **kwds: P.kwargs) -> str:
            return str(func(*args, **kwds))
        return wrapper

    @stringify
    def add_forty_two(value: int) -> int:
        return value + 42

    a = add_forty_two(3)
    reveal_type(a)      # Revealed type is "builtins.str"
    add_forty_two('x')  # error: Argument 1 to "add_forty_two" has incompatible type "str"; expected "int"
    ```

    Or insert an argument:

    ```python
    from typing import Callable, TypeVar
    from typing_extensions import Concatenate, ParamSpec

    P = ParamSpec('P')
    T = TypeVar('T')

    def printing_decorator(func: Callable[P, T]) -> Callable[Concatenate[str, P], T]:
        def wrapper(msg: str, /, *args: P.args, **kwds: P.kwargs) -> T:
            print("Calling", func, "with", msg)
            return func(*args, **kwds)
        return wrapper

    @printing_decorator
    def add_forty_two(value: int) -> int:
        return value + 42

    a = add_forty_two('three', 3)
    ```

### 装饰器工厂

Decorator factories

=== "中文"

    接受参数并返回装饰器（也称为二阶装饰器）的函数同样通过泛型获得支持：

    ```python
    from typing import Any, Callable, TypeVar

    F = TypeVar('F', bound=Callable[..., Any])

    def route(url: str) -> Callable[[F], F]:
        ...

    @route(url='/')
    def index(request: Any) -> str:
        return 'Hello world'
    ```

    有时，同一个装饰器同时支持裸调用和带参数的调用。 这可以通过与 [`@overload`](https://docs.python.org/3/library/typing.html#typing.overload) 结合来实现：

    ```python
    from typing import Any, Callable, Optional, TypeVar, overload

    F = TypeVar('F', bound=Callable[..., Any])

    # 裸装饰器的使用
    @overload
    def atomic(__func: F) -> F: ...
    # 带参数的装饰器
    @overload
    def atomic(*, savepoint: bool = True) -> Callable[[F], F]: ...

    # 实现
    def atomic(__func: Optional[Callable[..., Any]] = None, *, savepoint: bool = True):
        def decorator(func: Callable[..., Any]):
            ...  # 代码放在这里
        if __func is not None:
            return decorator(__func)
        else:
            return decorator

    # 用法
    @atomic
    def func1() -> None: ...

    @atomic(savepoint=False)
    def func2() -> None: ...
    ```

=== "英文"

    Functions that take arguments and return a decorator (also called second-order decorators), are similarly supported via generics:

    ```python
    from typing import Any, Callable, TypeVar

    F = TypeVar('F', bound=Callable[..., Any])

    def route(url: str) -> Callable[[F], F]:
        ...

    @route(url='/')
    def index(request: Any) -> str:
        return 'Hello world'
    ```

    Sometimes the same decorator supports both bare calls and calls with arguments. This can be achieved by combining with [`@overload`](https://docs.python.org/3/library/typing.html#typing.overload):

    ```python
    from typing import Any, Callable, Optional, TypeVar, overload

    F = TypeVar('F', bound=Callable[..., Any])

    # Bare decorator usage
    @overload
    def atomic(__func: F) -> F: ...
    # Decorator with arguments
    @overload
    def atomic(*, savepoint: bool = True) -> Callable[[F], F]: ...

    # Implementation
    def atomic(__func: Optional[Callable[..., Any]] = None, *, savepoint: bool = True):
        def decorator(func: Callable[..., Any]):
            ...  # Code goes here
        if __func is not None:
            return decorator(__func)
        else:
            return decorator

    # Usage
    @atomic
    def func1() -> None: ...

    @atomic(savepoint=False)
    def func2() -> None: ...
    ```

## 泛型协议

Generic protocols

=== "中文"

    Mypy 支持通用协议（另请参阅 [`protocol-types`](./protocol_and_struct_subtyping.md#协议和结构子类型)）。 一些[`预定义协议`](https://mypy.readthedocs.io/en/stable/protocols.html#predefined-protocols)是通用的，例如[`Iterable[T]`](https://docs. python.org/3/library/typing.html#typing.Iterable)，并且您可以定义其他通用协议。 泛型协议大多遵循泛型类的正常规则。 例子：

    ```python
    from typing import TypeVar
    from typing_extensions import Protocol

    T = TypeVar('T')

    class Box(Protocol[T]):
        content: T

    def do_stuff(one: Box[str], other: Box[bytes]) -> None:
        ...

    class StringWrapper:
        def __init__(self, content: str) -> None:
            self.content = content

    class BytesWrapper:
        def __init__(self, content: bytes) -> None:
            self.content = content

    do_stuff(StringWrapper('one'), BytesWrapper(b'other'))  # OK

    x: Box[float] = ...
    y: Box[int] = ...
    x = y  # Error -- Box is invariant
    ```

    请注意，根据 [`PEP 544：泛型协议`](https://peps.python.org/pep-0544/#generic-protocols)，允许使用 `class ClassName(Protocol[T])` 作为 `class ClassName(Protocol, Generic[T])` 的简写，

    泛型协议和普通泛型类之间的主要区别在于，mypy 检查协议中泛型类型变量的声明差异是否与它们在协议定义中的使用方式相匹配。 此示例中的协议被拒绝，因为类型变量“T”被协变地用作返回类型，但类型变量是不变的：

    ```python
    from typing import Protocol, TypeVar

    T = TypeVar('T')

    class ReadOnlyBox(Protocol[T]):  # error: 在需要协变的协议中使用不变类型变量“T”
        def content(self) -> T: ...
    ```

    此示例正确使用协变类型变量：

    ```python
    from typing import Protocol, TypeVar

    T_co = TypeVar('T_co', covariant=True)

    class ReadOnlyBox(Protocol[T_co]):  # OK
        def content(self) -> T_co: ...

    ax: ReadOnlyBox[float] = ...
    ay: ReadOnlyBox[int] = ...
    ax = ay  # OK -- ReadOnlyBox is covariant
    ```

    有关变体的更多信息，请参阅[`泛型变体`](#泛型类型的变体)。

    泛型协议也可以是递归的。 例子：

    ```python
    T = TypeVar('T')

    class Linked(Protocol[T]):
        val: T
        def next(self) -> 'Linked[T]': ...

    class L:
        val: int
        def next(self) -> 'L': ...

    def last(seq: Linked[T]) -> T: ...

    result = last(L())
    reveal_type(result)  # Revealed type is "builtins.int"
    ```

=== "英文"

    Mypy supports generic protocols (see also [`protocol-types`](./protocol_and_struct_subtyping.md#协议和结构子类型)). Several [`predefined protocols`](https://mypy.readthedocs.io/en/stable/protocols.html#predefined-protocols) are generic, such as [`Iterable[T]`](https://docs.python.org/3/library/typing.html#typing.Iterable), and you can define additional generi protocols. Generic protocols mostly follow the normal rules for generic classes. Example:

    ```python
    from typing import TypeVar
    from typing_extensions import Protocol

    T = TypeVar('T')

    class Box(Protocol[T]):
        content: T

    def do_stuff(one: Box[str], other: Box[bytes]) -> None:
        ...

    class StringWrapper:
        def __init__(self, content: str) -> None:
            self.content = content

    class BytesWrapper:
        def __init__(self, content: bytes) -> None:
            self.content = content

    do_stuff(StringWrapper('one'), BytesWrapper(b'other'))  # OK

    x: Box[float] = ...
    y: Box[int] = ...
    x = y  # Error -- Box is invariant
    ```

    Note that `class ClassName(Protocol[T])` is allowed as a shorthand for `class ClassName(Protocol, Generic[T])`, as per [`PEP 544: Generic protocols`](https://peps.python.org/pep-0544/#generic-protocols),

    The main difference between generic protocols and ordinary generic classes is that mypy checks that the declared variances of generic type variables in a protocol match how they are used in the protocol definition.  The protocol in this example is rejected, since the type variable `T` is used covariantly as a return type, but the type variable is invariant:

    ```python
    from typing import Protocol, TypeVar

    T = TypeVar('T')

    class ReadOnlyBox(Protocol[T]):  # error: Invariant type variable "T" used in protocol where covariant one is expected
        def content(self) -> T: ...
    ```

    This example correctly uses a covariant type variable:

    ```python
    from typing import Protocol, TypeVar

    T_co = TypeVar('T_co', covariant=True)

    class ReadOnlyBox(Protocol[T_co]):  # OK
        def content(self) -> T_co: ...

    ax: ReadOnlyBox[float] = ...
    ay: ReadOnlyBox[int] = ...
    ax = ay  # OK -- ReadOnlyBox is covariant
    ```

    See [`variance-of-generics`](https://mypy.readthedocs.io/en/stable/generics.html#variance-of-generics) for more about variance.

    Generic protocols can also be recursive. Example:

    ```python
    T = TypeVar('T')

    class Linked(Protocol[T]):
        val: T
        def next(self) -> 'Linked[T]': ...

    class L:
        val: int
        def next(self) -> 'L': ...

    def last(seq: Linked[T]) -> T: ...

    result = last(L())
    reveal_type(result)  # Revealed type is "builtins.int"
    ```

## 泛型类型别名

Generic type aliases

=== "中文"

    类型别名可以是泛型的。 在这种情况下，它们可以通过两种方式使用： 带下标的别名相当于带有替换类型变量的原始类型，因此类型参数的数量必须与泛型类型别名中自由类型变量的数量相匹配。 无下标的别名被视为原始类型，自由变量替换为“Any”。 示例（遵循 [`PEP 484：类型别名`](https://peps.python.org/pep-0484/#type-aliases)）：

    ```python
    from typing import TypeVar, Iterable, Union, Callable

    S = TypeVar('S')

    TInt = tuple[int, S]
    UInt = Union[S, int]
    CBack = Callable[..., S]

    def response(query: str) -> UInt[str]:  # Same as Union[str, int]
        ...
    def activate(cb: CBack[S]) -> S:        # Same as Callable[..., S]
        ...
    table_entry: TInt  # Same as tuple[int, Any]

    T = TypeVar('T', int, float, complex)

    Vec = Iterable[tuple[T, T]]

    def inproduct(v: Vec[T]) -> T:
        return sum(x*y for x, y in v)

    def dilate(v: Vec[T], scale: T) -> Vec[T]:
        return ((x * scale, y * scale) for x, y in v)

    v1: Vec[int] = []      # Same as Iterable[tuple[int, int]]
    v2: Vec = []           # Same as Iterable[tuple[Any, Any]]
    v3: Vec[int, int] = [] # Error: Invalid alias, too many type arguments!
    ```

    类型别名可以像其他名称一样从模块导入。 一个别名也可以针对另一个别名，尽管不建议构建复杂的别名链——这会妨碍代码的可读性，从而违背了使用别名的目的。 例子：

    ```python
    from typing import TypeVar, Generic, Optional
    from example1 import AliasType
    from example2 import Vec

    # AliasType and Vec are type aliases (Vec as defined above)

    def fun() -> AliasType:
        ...

    T = TypeVar('T')

    class NewVec(Vec[T]):
        ...

    for i, j in NewVec[int]():
        ...

    OIntVec = Optional[Vec[int]]
    ```

    Using type variable bounds or values in generic aliases has the same effect as in generic classes/functions.

    在泛型类/函数中使用的泛型别名中使用类型变量绑定或值与具有相同的效果。

=== "英文"

    Type aliases can be generic. In this case they can be used in two ways: Subscripted aliases are equivalent to original types with substituted type variables, so the number of type arguments must match the number of free type variables in the generic type alias. Unsubscripted aliases are treated as original types with free variables replaced with `Any`. Examples (following [`PEP 484: Type aliases`](https://peps.python.org/pep-0484/#type-aliases)):

    ```python
    from typing import TypeVar, Iterable, Union, Callable

    S = TypeVar('S')

    TInt = tuple[int, S]
    UInt = Union[S, int]
    CBack = Callable[..., S]

    def response(query: str) -> UInt[str]:  # Same as Union[str, int]
        ...
    def activate(cb: CBack[S]) -> S:        # Same as Callable[..., S]
        ...
    table_entry: TInt  # Same as tuple[int, Any]

    T = TypeVar('T', int, float, complex)

    Vec = Iterable[tuple[T, T]]

    def inproduct(v: Vec[T]) -> T:
        return sum(x*y for x, y in v)

    def dilate(v: Vec[T], scale: T) -> Vec[T]:
        return ((x * scale, y * scale) for x, y in v)

    v1: Vec[int] = []      # Same as Iterable[tuple[int, int]]
    v2: Vec = []           # Same as Iterable[tuple[Any, Any]]
    v3: Vec[int, int] = [] # Error: Invalid alias, too many type arguments!
    ```

    Type aliases can be imported from modules just like other names. An alias can also target another alias, although building complex chains of aliases is not recommended -- this impedes code readability, thus defeating the purpose of using aliases.  Example:

    ```python
    from typing import TypeVar, Generic, Optional
    from example1 import AliasType
    from example2 import Vec

    # AliasType and Vec are type aliases (Vec as defined above)

    def fun() -> AliasType:
        ...

    T = TypeVar('T')

    class NewVec(Vec[T]):
        ...

    for i, j in NewVec[int]():
        ...

    OIntVec = Optional[Vec[int]]
    ```

    Using type variable bounds or values in generic aliases has the same effect as in generic classes/functions.

## 泛型类内部结构

Generic class internals

=== "中文"

    您可能想知道当您索引泛型类时在运行时会发生什么。 索引向原始类返回一个*泛型别名*，该别名在实例化时返回原始类的实例：

    ```python
    >>> from typing import TypeVar, Generic
    >>> T = TypeVar('T')
    >>> class Stack(Generic[T]): ...
    >>> Stack
    __main__.Stack
    >>> Stack[int]
    __main__.Stack[int]
    >>> instance = Stack[int]()
    >>> instance.__class__
    __main__.Stack
    ```

    泛型别名可以被实例化或子类化，类似于真实的类，但上面的示例说明类型变量在运行时被删除。 泛型 “Stack” 实例只是普通的 Python 对象，除了重载索引运算符的元类之外，它们没有额外的运行时开销或魔力，因为它们是泛型的。

    请注意，在 Python 3.8 及更低版本中，内置类型 [`list`](https://docs.python.org/3/library/stdtypes.html#list)、[`dict`](https:// docs.python.org/3/library/stdtypes.html#dict) 和其他不支持索引。 这就是为什么我们有别名 [`List`](https://docs.python.org/3/library/typing.html#typing.List)、[`Dict`](https://docs.python. org/3/library/typing.html#typing.Dict) 等在 [`typing`](https://docs.python.org/3/library/typing.html#module-typing) 模块中。 对这些别名进行索引会为您提供一个泛型别名，该别名类似于通过在较新版本的 Python 中直接对目标类建立索引而构造的泛型别名：

    ```python
    >>> # 仅与 Python 3.8 及更低版本相关
    >>> # 对于 Python 3.9 及以上版本，更喜欢 `list[int]` 语法
    >>> from typing import List
    >>> List[int]
    typing.List[int]
    ```

    请注意，“typing” 中的通用别名不支持构造实例：

    ```python
    >>> from typing import List
    >>> List[int]()
    Traceback (most recent call last):
    ...
    TypeError: Type List cannot be instantiated; use list() instead
    ```

=== "英文"

    You may wonder what happens at runtime when you index a generic class. Indexing returns a *generic alias* to the original class that returns instances of the original class on instantiation:

    ```python
    >>> from typing import TypeVar, Generic
    >>> T = TypeVar('T')
    >>> class Stack(Generic[T]): ...
    >>> Stack
    __main__.Stack
    >>> Stack[int]
    __main__.Stack[int]
    >>> instance = Stack[int]()
    >>> instance.__class__
    __main__.Stack
    ```

    Generic aliases can be instantiated or subclassed, similar to real classes, but the above examples illustrate that type variables are erased at runtime. Generic `Stack` instances are just ordinary Python objects, and they have no extra runtime overhead or magic due to being generic, other than a metaclass that overloads the indexing operator.

    Note that in Python 3.8 and lower, the built-in types [`list`](https://docs.python.org/3/library/stdtypes.html#list), [`dict`](https://docs.python.org/3/library/stdtypes.html#dict) and others do not support indexing. This is why we have the aliases [`List`](https://docs.python.org/3/library/typing.html#typing.List), [`Dict`](https://docs.python.org/3/library/typing.html#typing.Dict) and so on in the [`typing`](https://docs.python.org/3/library/typing.html#module-typing) module. Indexing these aliases gives you a generic alias that resembles generic aliases constructed by directly indexing the target class in more recent versions of Python:

    ```python
    >>> # Only relevant for Python 3.8 and below
    >>> # For Python 3.9 onwards, prefer `list[int]` syntax
    >>> from typing import List
    >>> List[int]
    typing.List[int]
    ```

    Note that the generic aliases in `typing` don't support constructing instances:

    ```python
    >>> from typing import List
    >>> List[int]()
    Traceback (most recent call last):
    ...
    TypeError: Type List cannot be instantiated; use list() instead
    ```
