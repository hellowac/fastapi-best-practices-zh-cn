# 动态类型的代码

=== "中文"

    在 [getting-started-dynamic-vs-static](https://mypy.readthedocs.io/en/stable/getting_started.html#getting-started-dynamic-vs-static) 中，我们讨论了函数体如何 在其函数中没有任何显式类型注释的函数是`动态类型的`，并且 mypy 不会检查它们。 在本节中，我们将详细讨论这意味着什么以及如何在更细粒度的基础上启用动态类型。
    
    如果您的代码对于 mypy 来说太神奇而无法理解，您可以通过显式赋予变量或参数`Any`类型来使其动态类型化。 Mypy 基本上可以让您使用`Any`类型的值执行任何操作，包括将`Any`类型的值分配给任何类型的变量（反之亦然）。

    ```python
    from typing import Any
    
    num = 1         # 静态类型（推断为 int）
    num = 'x'       # error: Incompatible types in assignment (expression has type "str", variable has type "int")
    
    dyn: Any = 1    # 动态类型（类型 Any）
    dyn = 'x'       # OK
    
    num = dyn       # 没有错误，mypy 会让你将 Any 类型的值赋给任何变量
    num += 1        # Oops, mypy 仍然认为 num 是 int
    ```
    
    您可以将 `Any` 视为本地禁用类型检查的一种方法。 请参阅 [silencing-type-errors](https://mypy.readthedocs.io/en/stable/type_inference_and_annotations.html#silencing-type-errors) 来了解关闭类型检查器的其他方法。

=== "英文"

    **Dynamically typed code**

    In [`getting-started-dynamic-vs-static`](https://mypy.readthedocs.io/en/stable/getting_started.html#getting-started-dynamic-vs-static), we discussed how bodies of functions that don't have any explicit type annotations in their function are "dynamically typed" and that mypy will not check them. In this section, we'll talk a little bit more about what that means and how you can enable dynamic typing on a more fine grained basis.
    
    In cases where your code is too magical for mypy to understand, you can make a variable or parameter dynamically typed by explicitly giving it the type `Any`. Mypy will let you do basically anything with a value of type `Any`, including assigning a value of type `Any` to a variable of any type (or vice versa).

    ```python
    from typing import Any
    
    num = 1         # Statically typed (inferred to be int)
    num = 'x'       # error: Incompatible types in assignment (expression has type "str", variable has type "int")
    
    dyn: Any = 1    # Dynamically typed (type Any)
    dyn = 'x'       # OK
    
    num = dyn       # No error, mypy will let you assign a value of type Any to any variable
    num += 1        # Oops, mypy still thinks num is an int
    ```
    
    You can think of `Any` as a way to locally disable type checking. See [`silencing-type-errors`](https://mypy.readthedocs.io/en/stable/type_inference_and_annotations.html#silencing-type-errors) for other ways you can shut up the type checker.

## Operations on Any values

You can do anything using a value with type `Any`, and the type checker
will not complain:

```python
def f(x: Any) -> int:
    # All of these are valid!
    x.foobar(1, y=2)
    print(x[3] + 'f')
    if x:
        x.z = x(2)
    open(x).read()
    return x
```

Values derived from an `Any` value also usually have the type `Any`
implicitly, as mypy can't infer a more precise result type. For
example, if you get the attribute of an `Any` value or call a
`Any` value the result is `Any`:

```python
def f(x: Any) -> None:
    y = x.foo()
    reveal_type(y)  # Revealed type is "Any"
    z = y.bar("mypy will let you do anything to y")
    reveal_type(z)  # Revealed type is "Any"
```

`Any` types may propagate through your program, making type checking
less effective, unless you are careful.

Function parameters without annotations are also implicitly `Any`:

```python
def f(x) -> None:
    reveal_type(x)  # Revealed type is "Any"
    x.can.do["anything", x]("wants", 2)
```

You can make mypy warn you about untyped function parameters using the
{option}`--disallow-untyped-defs <mypy --disallow-untyped-defs>` flag.

Generic types missing type parameters will have those parameters implicitly
treated as `Any`:

```python
from typing import List

def f(x: List) -> None:
    reveal_type(x)        # Revealed type is "builtins.list[Any]"
    reveal_type(x[0])     # Revealed type is "Any"
    x[0].anything_goes()  # OK
```

You can make mypy warn you about untyped function parameters using the
{option}`--disallow-any-generics <mypy --disallow-any-generics>` flag.

Finally, another major source of `Any` types leaking into your program is from
third party libraries that mypy does not know about. This is particularly the case
when using the {option}`--ignore-missing-imports <mypy --ignore-missing-imports>`
flag. See {ref}`fix-missing-imports` for more information about this.

## Any vs. object

The type {py:class}`object` is another type that can have an instance of arbitrary
type as a value. Unlike `Any`, {py:class}`object` is an ordinary static type (it
is similar to `Object` in Java), and only operations valid for *all*
types are accepted for {py:class}`object` values. These are all valid:

```python
def f(o: object) -> None:
    if o:
        print(o)
    print(isinstance(o, int))
    o = 2
    o = 'foo'
```

These are, however, flagged as errors, since not all objects support these
operations:

```python
def f(o: object) -> None:
    o.foo()       # Error!
    o + 2         # Error!
    open(o)       # Error!
    n: int = 1
    n = o         # Error!
```

If you're not sure whether you need to use {py:class}`object` or `Any`, use
{py:class}`object` -- only switch to using `Any` if you get a type checker
complaint.

You can use different {ref}`type narrowing <type-narrowing>`
techniques to narrow {py:class}`object` to a more specific
type (subtype) such as `int`. Type narrowing is not needed with
dynamically typed values (values with type `Any`).
