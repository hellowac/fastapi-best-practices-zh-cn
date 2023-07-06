# 元类

## aaa

=== "中文"

    元类 [`metaclasses`](https://docs.python.org/3/reference/datamodel.html#metaclasses) 是一个描述其他类的构造和行为的类，类似于类描述对象的构造和行为。 默认元类是 [`type`](https://docs.python.org/3/library/functions.html#type)，但可以使用其他元类。 元类允许创建 “一种不同类型的类”，例如 [`enum.Enum`](https://docs.python.org/3/library/enum.html#enum.Enum)、[`typing.NamedTuple`](https://docs.python.org/3/library/typing.html#typing.NamedTuple) 和单例。
    
    Mypy 对 [`abc.ABCMeta`](https://docs.python.org/3/library/abc.html#abc.ABCMeta) 和 `EnumMeta` 有一些特殊的理解。

=== "英文"

    **Metaclasses**

    A [`metaclasses`](https://docs.python.org/3/reference/datamodel.html#metaclasses) is a class that describes the construction and behavior of other classes, similarly to how classes describe the construction and behavior of objects. The default metaclass is [`type`](https://docs.python.org/3/library/functions.html#type), but it's possible to use other metaclasses. Metaclasses allows one to create "a different kind of class", such as [`enum.Enum`](https://docs.python.org/3/library/enum.html#enum.Enum)s、[`typing.NamedTuple`](https://docs.python.org/3/library/typing.html#typing.NamedTuple)s and singletons.
    
    Mypy has some special understanding of {py:class}`~abc.ABCMeta` and `EnumMeta`.

## 定义元类

=== "中文"

    ```python
    class M(type):
        pass
    
    class A(metaclass=M):
        pass
    ```

=== "英文"

    **Defining a metaclass**

    ```python
    class M(type):
        pass
    
    class A(metaclass=M):
        pass
    ```

## 元类使用示例

=== "中文"

    Mypy 支持在元类中查找属性：

    ```python
    from typing import Type, TypeVar, ClassVar
    T = TypeVar('T')
    
    class M(type):
        count: ClassVar[int] = 0
    
        def make(cls: Type[T]) -> T:
            M.count += 1
            return cls()
    
    class A(metaclass=M):
        pass
    
    a: A = A.make()  # make() 会在 M 中查找； 结果是 A 类型的对象
    print(A.count)
    
    class B(A):
        pass
    
    b: B = B.make()  # 元类是继承的
    print(B.count + " objects were created")  # Error: Unsupported operand types for + ("int" and "str")
    ```

=== "英文"

    **Metaclass usage example**

    Mypy supports the lookup of attributes in the metaclass:

    ```python
    from typing import Type, TypeVar, ClassVar
    T = TypeVar('T')
    
    class M(type):
        count: ClassVar[int] = 0
    
        def make(cls: Type[T]) -> T:
            M.count += 1
            return cls()
    
    class A(metaclass=M):
        pass
    
    a: A = A.make()  # make() is looked up at M; the result is an object of type A
    print(A.count)
    
    class B(A):
        pass
    
    b: B = B.make()  # metaclasses are inherited
    print(B.count + " objects were created")  # Error: Unsupported operand types for + ("int" and "str")
    ```

## 元类支持的陷阱和限制

=== "中文"

    请注意，元类对继承结构提出了一些要求，因此最好不要将元类和类层次结构结合起来：

    ```python
    class M1(type): pass
    class M2(type): pass
    
    class A1(metaclass=M1): pass
    class A2(metaclass=M2): pass
    
    class B1(A1, metaclass=M2): pass  # Mypy Error: metaclass conflict
    # 在运行时，上面的定义会引发异常
    # TypeError: metaclass conflict: 派生类的元类必须是其所有基类的元类的（非严格）子类
    
    class B12(A1, A2): pass  # Mypy Error: metaclass conflict
    
    # 这可以通过通用元类子类型来解决：
    class CorrectMeta(M1, M2): pass
    class B2(A1, A2, metaclass=CorrectMeta): pass  # OK, 运行时也没问题
    ```
    
    - Mypy 不理解动态计算的元类，例如 `class A(metaclass=f()): ...`
    - Mypy 不会也不可能理解任意元类代码。
    - Mypy 仅将 [`type`](https://docs.python.org/3/library/functions.html#type) 的子类识别为潜在的元类。

=== "英文"

    **Gotchas and limitations of metaclass support**

    Note that metaclasses pose some requirements on the inheritance structure,
    so it's better not to combine metaclasses and class hierarchies:

    ```python
    class M1(type): pass
    class M2(type): pass
    
    class A1(metaclass=M1): pass
    class A2(metaclass=M2): pass
    
    class B1(A1, metaclass=M2): pass  # Mypy Error: metaclass conflict
    # At runtime the above definition raises an exception
    # TypeError: metaclass conflict: the metaclass of a derived class must be a (non-strict) subclass of the metaclasses of all its bases
    
    class B12(A1, A2): pass  # Mypy Error: metaclass conflict
    
    # This can be solved via a common metaclass subtype:
    class CorrectMeta(M1, M2): pass
    class B2(A1, A2, metaclass=CorrectMeta): pass  # OK, runtime is also OK
    ```

    - Mypy does not understand dynamically-computed metaclasses, such as `class A(metaclass=f()): ...`
    - Mypy does not and cannot understand arbitrary metaclass code.
    - Mypy only recognizes subclasses of [`type`](https://docs.python.org/3/library/functions.html#type) as potential metaclasses.
