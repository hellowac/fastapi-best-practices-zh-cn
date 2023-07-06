# 鸭子类型兼容性

=== "中文"

    在 Python 中，某些类型是兼容的，即使它们不是彼此的子类。 例如，每当需要 `float` 对象时，`int`对象都是有效的。 Mypy 通过 *duck 类型兼容性* 支持这种习惯用法。 一小组内置类型支持这一点：
    
    - `int` 是与 `float` 和 `complex` 兼容的鸭子类型。
    - `float` 是与 `complex` 兼容的鸭子类型。
    - `bytearray` 和 `memoryview` 是与 `bytes` 兼容的鸭子类型。
    
    例如，只要需要 `float` 对象，mypy 就会认为 `int` 对象有效。 因此，这样的代码干净整洁，并且行为也符合预期：

    ```python
    import math
    
    def degrees_to_radians(degrees: float) -> float:
        return math.pi * degrees / 180
    
    n = 90  # 推断类型“int”
    print(degrees_to_radians(n))  # Okay!
    ```

    您还可以经常使用[协议和结构子类型](./protocol_and_struct_subtyping.md)以更有原则和可扩展的方式实现类似的效果。 协议不适用于像`int`与`float`兼容的情况，因为`float`不是一个协议类，而是一个常规的具体类，并且许多标准库函数期望`float`（或`int`）的具体实例 `）。

=== "英文"

    **Duck type compatibility**

    In Python, certain types are compatible even though they aren't subclasses of each other. For example, `int` objects are valid whenever `float` objects are expected. Mypy supports this idiom via *duck type compatibility*. This is supported for a small set of built-in types:
    
    - `int` is duck type compatible with `float` and `complex`.
    - `float` is duck type compatible with `complex`.
    - `bytearray` and `memoryview` are duck type compatible with `bytes`.
    
    For example, mypy considers an `int` object to be valid whenever a `float` object is expected.  Thus code like this is nice and clean and also behaves as expected:

    ```python
    import math
    
    def degrees_to_radians(degrees: float) -> float:
        return math.pi * degrees / 180
    
    n = 90  # Inferred type 'int'
    print(degrees_to_radians(n))  # Okay!
    ```

    You can also often use [`protocol-types`](./protocol_and_struct_subtyping.md) to achieve a similar effect in a more principled and extensible fashion. Protocols don't apply to cases like `int` being compatible with `float`, since `float` is not a protocol class but a regular, concrete class, and many standard library functions expect concrete instances of `float` (or `int`).
