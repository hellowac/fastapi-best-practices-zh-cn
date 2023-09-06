# 字面量和枚举

**Literal types and Enums**

## 字面量类型

Literal types

=== "中文"

    字面量类型可让您指示表达式等于某个特定的原始值。 例如，如果我们用 `Literal["foo"]` 类型注释一个变量，mypy 将理解该变量不仅是 `str` 类型，而且还等于字符串 `"foo"`。

    当注释根据调用者提供的确切值而表现不同的函数时，此功能主要有用。 例如，假设我们有一个函数“fetch_data(...)”，如果第一个参数为“True”，则返回 “bytes” ；如果第一个参数为“False”，则返回“str”。 我们可以使用“Literal[...]”和重载为此函数构造精确的类型签名：

    ```python
    from typing import overload, Union, Literal

    # 前两个重载使用 Literal[...]，因此我们可以获得精确的返回类型：

    @overload
    def fetch_data(raw: Literal[True]) -> bytes: ...
    @overload
    def fetch_data(raw: Literal[False]) -> str: ...

    # 最后一个重载是调用者提供常规布尔值时的后备：

    @overload
    def fetch_data(raw: bool) -> Union[bytes, str]: ...

    def fetch_data(raw: bool) -> Union[bytes, str]:
        # 省略实现
        ...

    reveal_type(fetch_data(True))        # Revealed type is "bytes"
    reveal_type(fetch_data(False))       # Revealed type is "str"

    # 没有注释声明的变量将继续具有“bool”的推断类型。

    variable = True
    reveal_type(fetch_data(variable))    # Revealed type is "Union[bytes, str]"
    ```

    !!! info "Note"

        本页中的示例从“typing”模块导入“Literal”以及“Final”和“TypedDict”。 这些类型已添加到 Python 3.8 中的“typing”中，但也可以通过“typing_extensions”包在 Python 3.4 - 3.7 中使用。

=== "英文"

    Literal types let you indicate that an expression is equal to some specific primitive value. For example, if we annotate a variable with type `Literal["foo"]`, mypy will understand that variable is not only of type `str`, but is also equal to specifically the string `"foo"`.

    This feature is primarily useful when annotating functions that behave differently based on the exact value the caller provides. For example, suppose we have a function `fetch_data(...)` that returns `bytes` if the first argument is `True`, and `str` if it's `False`. We can construct a precise type signature for this function using `Literal[...]` and overloads:

    ```python
    from typing import overload, Union, Literal

    # The first two overloads use Literal[...] so we can
    # have precise return types:

    @overload
    def fetch_data(raw: Literal[True]) -> bytes: ...
    @overload
    def fetch_data(raw: Literal[False]) -> str: ...

    # The last overload is a fallback in case the caller
    # provides a regular bool:

    @overload
    def fetch_data(raw: bool) -> Union[bytes, str]: ...

    def fetch_data(raw: bool) -> Union[bytes, str]:
        # Implementation is omitted
        ...

    reveal_type(fetch_data(True))        # Revealed type is "bytes"
    reveal_type(fetch_data(False))       # Revealed type is "str"

    # Variables declared without annotations will continue to have an
    # inferred type of 'bool'.

    variable = True
    reveal_type(fetch_data(variable))    # Revealed type is "Union[bytes, str]"
    ```

    !!! info "Note"

        The examples in this page import `Literal` as well as `Final` and `TypedDict` from the `typing` module. These types were added to `typing` in Python 3.8, but are also available for use in Python 3.4 - 3.7 via the `typing_extensions` package.

### 参数化字面量

Parameterizing Literals

=== "中文"

    字面量类型可以包含一个或多个文字 bool、int、str、byte 和 enum 值。 但是，字面量类型**不能**包含任意表达式：诸如“Literal[my_string.trim()]”、“Literal[x > 3]”或“Literal[3j + 4]”之类的类型都是非法的。

    包含两个或多个值的文字相当于这些值的并集。 因此， `Literal[-3, b"foo", MyEnum.A]` 相当于 `Union[Literal[-3], Literal[b] “foo”]，`Literal[[MyEnum.A]]`。 这使得编写涉及文字的更复杂的类型变得更加方便。

    字面量类型也可能包含“None”。 Mypy 会将 `Literal[None]` 视为等同于 `None`。 这意味着 `Literal[4, None]`、`Union[Literal[4], None]` 和 `Optional[Literal[4]]` 都是等价的。

    文字还可以包含其他字面量类型的别名。 例如，以下程序是合法的：

    ```python
    PrimaryColors = Literal["red", "blue", "yellow"]
    SecondaryColors = Literal["purple", "green", "orange"]
    AllowedColors = Literal[PrimaryColors, SecondaryColors]

    def paint(color: AllowedColors) -> None: ...

    paint("red")        # Type checks!
    paint("turquoise")  # Does not type check
    ```

    字面量不能包含任何其他类型或表达式。 这意味着执行 `Literal[my_instance]`、`Literal[Any]`、`Literal[3.14]` 或 `Literal[{"foo": 2, "bar": 5}]` 都是非法的。

=== "英文"

    Literal types may contain one or more literal bools, ints, strs, bytes, and enum values. However, literal types **cannot** contain arbitrary expressions: types like `Literal[my_string.trim()]`, `Literal[x > 3]`, or `Literal[3j + 4]` are all illegal.

    Literals containing two or more values are equivalent to the union of those values.= So, `Literal[-3, b"foo", MyEnum.A]` is equivalent to= `Union[Literal[-3], Literal[b"foo"], Literal[MyEnum.A]]`. This makes writing more= complex types involving literals a little more convenient.

    Literal types may also contain `None`. Mypy will treat `Literal[None]` as being equivalent to just `None`. This means that `Literal[4, None]`, `Union[Literal[4], None]`, and `Optional[Literal[4]]` are all equivalent.

    Literals may also contain aliases to other literal types. For example, the following program is legal:

    ```python
    PrimaryColors = Literal["red", "blue", "yellow"]
    SecondaryColors = Literal["purple", "green", "orange"]
    AllowedColors = Literal[PrimaryColors, SecondaryColors]

    def paint(color: AllowedColors) -> None: ...

    paint("red")        # Type checks!
    paint("turquoise")  # Does not type check
    ```

    Literals may not contain any other kind of type or expression. This means doing `Literal[my_instance]`, `Literal[Any]`, `Literal[3.14]`, or `Literal[{"foo": 2, "bar": 5}]` are all illegal.

### 声明字面量

Declaring literal variables

=== "中文"

    您必须显式向变量添加注释以声明它具有字面量类型：

    ```python
    a: Literal[19] = 19
    reveal_type(a)          # Revealed type is "Literal[19]"
    ```

    为了保持向后兼容性，没有此注解的变量**不**假定为字面量：

    ```python
    b = 19
    reveal_type(b)          # Revealed type is "int"
    ```

    如果您发现在类型提示中重复变量的值很乏味，您可以将变量更改为“Final”（请参阅[“final_attrs”](https://mypy.readthedocs.io/en/stable/final_attrs.html#final-attrs)）：

    ```python
    from typing import Final, Literal

    def expects_literal(x: Literal[19]) -> None: pass

    c: Final = 19

    reveal_type(c)          # Revealed type is "Literal[19]?"
    expects_literal(c)      # ...and this type checks!
    ```

    如果您没有在 “Final” 中提供显式类型，则 “c” 的类型将变为*上下文相关*：在执行类型检查之前，只要使用原始分配的值，mypy 基本上都会尝试“替换”原始分配的值。 这就是为什么 `c` 的显示类型是 `Literal[19]?`：末尾的问号反映了这种上下文相关的性质。

    例如，mypy 将对上述程序进行类型检查，几乎就像这样编写的：

    ```python
    from typing import Final, Literal

    def expects_literal(x: Literal[19]) -> None: pass

    reveal_type(19)
    expects_literal(19)
    ```

    这意味着虽然将变量更改为“Final”与添加显式“Literal[...]”注释并不完全相同，但在实践中通常会产生相同的效果。

    上下文相关类型与真实字面量类型的行为不同的主要情况是当您尝试在未明确期望“Literal[...]”的地方使用这些类型时。 例如，比较和对比当您尝试将这些类型附加到列表时会发生什么：

    ```python
    from typing import Final, Literal

    a: Final = 19
    b: Literal[19] = 19

    # Mypy 会选择在这里推断 list[int]。
    list_of_ints = []
    list_of_ints.append(a)
    reveal_type(list_of_ints)  # Revealed type is "list[int]"

    # 但是，如果您要附加的变量是显式文字，则 mypy 将推断 list[Literal[19]]。
    list_of_lits = []
    list_of_lits.append(b)
    reveal_type(list_of_lits)  # Revealed type is "list[Literal[19]]"
    ```

=== "英文"

    You must explicitly add an annotation to a variable to declare that it has a literal type:

    ```python
    a: Literal[19] = 19
    reveal_type(a)          # Revealed type is "Literal[19]"
    ```

    In order to preserve backwards-compatibility, variables without this annotation are **not** assumed to be literals:

    ```python
    b = 19
    reveal_type(b)          # Revealed type is "int"
    ```

    If you find repeating the value of the variable in the type hint to be tedious, you can instead change the variable to be `Final` (see [`final_attrs`](https://mypy.readthedocs.io/en/stable/final_attrs.html#final-attrs)):

    ```python
    from typing import Final, Literal

    def expects_literal(x: Literal[19]) -> None: pass

    c: Final = 19

    reveal_type(c)          # Revealed type is "Literal[19]?"
    expects_literal(c)      # ...and this type checks!
    ```

    If you do not provide an explicit type in the `Final`, the type of `c` becomes *context-sensitive*: mypy will basically try "substituting" the original assigned value whenever it's used before performing type checking. This is why the revealed type of `c` is `Literal[19]?`: the question mark at the end reflects this context-sensitive nature.

    For example, mypy will type check the above program almost as if it were written like so:

    ```python
    from typing import Final, Literal

    def expects_literal(x: Literal[19]) -> None: pass

    reveal_type(19)
    expects_literal(19)
    ```

    This means that while changing a variable to be `Final` is not quite the same thing as adding an explicit `Literal[...]` annotation, it often leads to the same effect in practice.

    The main cases where the behavior of context-sensitive vs true literal types differ are when you try using those types in places that are not explicitly expecting a `Literal[...]`. For example, compare and contrast what happens when you try appending these types to a list:

    ```python
    from typing import Final, Literal

    a: Final = 19
    b: Literal[19] = 19

    # Mypy will choose to infer list[int] here.
    list_of_ints = []
    list_of_ints.append(a)
    reveal_type(list_of_ints)  # Revealed type is "list[int]"

    # But if the variable you're appending is an explicit Literal, mypy
    # will infer list[Literal[19]].
    list_of_lits = []
    list_of_lits.append(b)
    reveal_type(list_of_lits)  # Revealed type is "list[Literal[19]]"
    ```

### 智能索引

Intelligent indexing

=== "中文"

    我们可以使用 Literal 类型更精确地索引到结构化异构类型，例如元组、NamedTuples 和 TypedDicts。 此功能称为“智能索引”。

    例如，当我们使用某些 int 索引元组时，推断的类型通常是元组项类型的并集。 但是，如果我们只想要与某个特定索引相对应的类型，我们可以使用字面量类型，如下所示：

    ```python
    from typing import TypedDict

    tup = ("foo", 3.4)

    # 使用 int 文字进行索引可以为我们提供该索引的确切类型
    reveal_type(tup[0])  # Revealed type is "str"

    # 但是如果我们希望索引是一个变量怎么办？ 通常 mypy 不会确切知道索引是什么，因此会返回不太精确的类型：
    int_index = 0
    reveal_type(tup[int_index])  # Revealed type is "Union[str, float]"

    # 但是如果我们使用 Literal 类型或 Final int，我们就可以恢复原来的精度：
    lit_index: Literal[0] = 0
    fin_index: Final = 0
    reveal_type(tup[lit_index])  # Revealed type is "str"
    reveal_type(tup[fin_index])  # Revealed type is "str"

    # 我们可以使用 TypedDict 和 str 键做同样的事情：
    class MyDict(TypedDict):
        name: str
        main_id: int
        backup_id: int

    d: MyDict = {"name": "Saanvi", "main_id": 111, "backup_id": 222}
    name_key: Final = "name"
    reveal_type(d[name_key])  # Revealed type is "str"

    # 您还可以使用字面量联合进行索引
    id_key: Literal["main_id", "backup_id"]
    reveal_type(d[id_key])    # Revealed type is "int"
    ```

=== "英文"

    We can use Literal types to more precisely index into structured heterogeneous types such as tuples, NamedTuples, and TypedDicts. This feature is known as *intelligent indexing*.

    For example, when we index into a tuple using some int, the inferred type is normally the union of the tuple item types. However, if we want just the type corresponding to some particular index, we can use Literal types like so:

    ```python
    from typing import TypedDict

    tup = ("foo", 3.4)

    # Indexing with an int literal gives us the exact type for that index
    reveal_type(tup[0])  # Revealed type is "str"

    # But what if we want the index to be a variable? Normally mypy won't
    # know exactly what the index is and so will return a less precise type:
    int_index = 0
    reveal_type(tup[int_index])  # Revealed type is "Union[str, float]"

    # But if we use either Literal types or a Final int, we can gain back
    # the precision we originally had:
    lit_index: Literal[0] = 0
    fin_index: Final = 0
    reveal_type(tup[lit_index])  # Revealed type is "str"
    reveal_type(tup[fin_index])  # Revealed type is "str"

    # We can do the same thing with with TypedDict and str keys:
    class MyDict(TypedDict):
        name: str
        main_id: int
        backup_id: int

    d: MyDict = {"name": "Saanvi", "main_id": 111, "backup_id": 222}
    name_key: Final = "name"
    reveal_type(d[name_key])  # Revealed type is "str"

    # You can also index using unions of literals
    id_key: Literal["main_id", "backup_id"]
    reveal_type(d[id_key])    # Revealed type is "int"
    ```

### 标记联合类型

Tagged unions

=== "中文"

    当您有类型的联合时，通常可以使用“isinstance”检查来区分联合中的每种类型。 例如，如果您有一个类型为“Union[int, str]”的变量“x”，您可以通过执行“if isinstance(x, int): ..”来编写一些仅当“x”是 int 时才运行的代码。 .`.

    然而，这样做并不总是可能或方便。 例如，不可能使用“isinstance”来区分两个不同的 TypedDict，因为在运行时，您的变量只是一个字典。

    相反，您可以做的是使用不同的文字类型*标签*或*标记*您的 TypedDicts。 然后，您可以通过检查标签来区分每种 TypedDict：

    ```python
    from typing import Literal, TypedDict, Union

    class NewJobEvent(TypedDict):
        tag: Literal["new-job"]
        job_name: str
        config_file_path: str

    class CancelJobEvent(TypedDict):
        tag: Literal["cancel-job"]
        job_id: int

    Event = Union[NewJobEvent, CancelJobEvent]

    def process_event(event: Event) -> None:
        # 由于我们确保两个 TypedDict 都有一个名为“tag”的键，因此执行“event[“tag”]”是安全的。 
        # 该表达式通常具有 Literal["new-job", "cancel-job"] 类型，
        # 但下面的检查会将类型范围缩小为 Literal["new-job"] 或 Literal["cancel-job"]。
        #
        # 这又将“event”的类型缩小为 NewJobEvent 或 CancelJobEvent。
        if event["tag"] == "new-job":
            print(event["job_name"])
        else:
            print(event["job_id"])
    ```

    虽然此功能在使用 TypedDicts 时非常有用，但您也可以对常规对象、元组或命名元组使用相同的技术。

    类似地，标签不需要是专门的 str 文字：它们可以是您通常可以在“if”语句等中缩小范围的任何类型。 
    例如，您可以将标签设置为 int 或 Enum Literals，甚至可以使用“isinstance()”缩小常规类：

    ```python
    from typing import Generic, TypeVar, Union

    T = TypeVar('T')

    class Wrapper(Generic[T]):
        def __init__(self, inner: T) -> None:
            self.inner = inner

    def process(w: Union[Wrapper[int], Wrapper[str]]) -> None:
        # 执行“if isinstance(w, Wrapper[int])”不起作用：isinstance 要求第二个参数始终是*已删除的*类型，没有泛型。 这是因为泛型是一个仅输入的概念，并且在运行时不以“isinstance”始终可以检查的方式存在。
        #
        # 然而，我们可以通过检查“w.inner”的类型来缩小“w”本身来回避这个问题：
        if isinstance(w.inner, int):
            reveal_type(w)  # Revealed type is "Wrapper[int]"
        else:
            reveal_type(w)  # Revealed type is "Wrapper[str]"
    ```

    在其他编程语言中，此功能有时称为“求和类型”或“可区分联合类型”。

=== "英文"

    When you have a union of types, you can normally discriminate between each type in the union by using `isinstance` checks. For example, if you had a variable `x` of type `Union[int, str]`, you could write some code that runs only if `x` is an int by doing `if isinstance(x, int): ...`.

    However, it is not always possible or convenient to do this. For example, it is not possible to use `isinstance` to distinguish between two different TypedDicts since at runtime, your variable will simply be just a dict.

    Instead, what you can do is *label* or *tag* your TypedDicts with a distinct Literal type. Then, you can discriminate between each kind of TypedDict by checking the label:

    ```python
    from typing import Literal, TypedDict, Union

    class NewJobEvent(TypedDict):
        tag: Literal["new-job"]
        job_name: str
        config_file_path: str

    class CancelJobEvent(TypedDict):
        tag: Literal["cancel-job"]
        job_id: int

    Event = Union[NewJobEvent, CancelJobEvent]

    def process_event(event: Event) -> None:
        # Since we made sure both TypedDicts have a key named 'tag', it's
        # safe to do 'event["tag"]'. This expression normally has the type
        # Literal["new-job", "cancel-job"], but the check below will narrow
        # the type to either Literal["new-job"] or Literal["cancel-job"].
        #
        # This in turns narrows the type of 'event' to either NewJobEvent
        # or CancelJobEvent.
        if event["tag"] == "new-job":
            print(event["job_name"])
        else:
            print(event["job_id"])
    ```

    While this feature is mostly useful when working with TypedDicts, you can also use the same technique with regular objects, tuples, or namedtuples.

    Similarly, tags do not need to be specifically str Literals: they can be any type you can normally narrow within `if` statements and the like. For example, you could have your tags be int or Enum Literals or even regular classes you narrow using `isinstance()`:

    ```python
    from typing import Generic, TypeVar, Union

    T = TypeVar('T')

    class Wrapper(Generic[T]):
        def __init__(self, inner: T) -> None:
            self.inner = inner

    def process(w: Union[Wrapper[int], Wrapper[str]]) -> None:
        # Doing `if isinstance(w, Wrapper[int])` does not work: isinstance requires
        # that the second argument always be an *erased* type, with no generics.
        # This is because generics are a typing-only concept and do not exist at
        # runtime in a way `isinstance` can always check.
        #
        # However, we can side-step this by checking the type of `w.inner` to
        # narrow `w` itself:
        if isinstance(w.inner, int):
            reveal_type(w)  # Revealed type is "Wrapper[int]"
        else:
            reveal_type(w)  # Revealed type is "Wrapper[str]"
    ```

    This feature is sometimes called "sum types" or "discriminated union types" in other programming languages.

### 详尽性检查

Exhaustiveness checking

=== "中文"

    您可能想要检查某些代码是否涵盖了所有可能的“Literal”或“Enum”情况。 例子：

    ```python
    from typing import Literal

    PossibleValues = Literal['one', 'two']

    def validate(x: PossibleValues) -> bool:
        if x == 'one':
            return True
        elif x == 'two':
            return False
        raise ValueError(f'Invalid value: {x}')

    assert validate('one') is True
    assert validate('two') is False
    ```

    上面的代码很容易出错。 您可以向“PossibleValues”添加一个新的文字值，但忘记在“validate”函数中处理它：

    ```python
    PossibleValues = Literal['one', 'two', 'three']
    ```

    Mypy 不会发现“three”未被覆盖。 如果您希望 mypy 执行详尽检查，则需要更新代码以使用 `assert_never()` 检查：

    ```python
    from typing import Literal, NoReturn

    PossibleValues = Literal['one', 'two']

    def assert_never(value: NoReturn) -> NoReturn:
        # 这在运行时也有效
        assert False, f'This code should never be reached, got: {value}'

    def validate(x: PossibleValues) -> bool:
        if x == 'one':
            return True
        elif x == 'two':
            return False
        assert_never(x)
    ```

    现在，如果您向“PossibleValues”添加新值但不更新“validate”，mypy 将发现错误：

    ```python
    PossibleValues = Literal['one', 'two', 'three']

    def validate(x: PossibleValues) -> bool:
        if x == 'one':
            return True
        elif x == 'two':
            return False
        # Error: Argument 1 to "assert_never" has incompatible type "Literal['three']";
        # expected "NoReturn"
        assert_never(x)
    ```

    如果不需要运行时检查意外值，则可以省略上面示例中的“assert_never”调用，并且 mypy 仍会生成有关函数“validate”返回无值的错误：

    ```python
    PossibleValues = Literal['one', 'two', 'three']

    # Error: Missing return statement
    def validate(x: PossibleValues) -> bool:
        if x == 'one':
            return True
        elif x == 'two':
            return False
    ```

    匹配语句也支持详尽检查（Python 3.10 及更高版本）：

    ```python
    def validate(x: PossibleValues) -> bool:
        match x:
            case 'one':
                return True
            case 'two':
                return False
        assert_never(x)
    ```

=== "英文"

    You may want to check that some code covers all possible `Literal` or `Enum` cases. Example:

    ```python
    from typing import Literal

    PossibleValues = Literal['one', 'two']

    def validate(x: PossibleValues) -> bool:
        if x == 'one':
            return True
        elif x == 'two':
            return False
        raise ValueError(f'Invalid value: {x}')

    assert validate('one') is True
    assert validate('two') is False
    ```

    In the code above, it's easy to make a mistake. You can add a new literal value to `PossibleValues` but forget to handle it in the `validate` function:

    ```python
    PossibleValues = Literal['one', 'two', 'three']
    ```

    Mypy won't catch that `'three'` is not covered.  If you want mypy to perform an exhaustiveness check, you need to update your code to use an `assert_never()` check:

    ```python
    from typing import Literal, NoReturn

    PossibleValues = Literal['one', 'two']

    def assert_never(value: NoReturn) -> NoReturn:
        # This also works at runtime as well
        assert False, f'This code should never be reached, got: {value}'

    def validate(x: PossibleValues) -> bool:
        if x == 'one':
            return True
        elif x == 'two':
            return False
        assert_never(x)
    ```

    Now if you add a new value to `PossibleValues` but don't update `validate`, mypy will spot the error:

    ```python
    PossibleValues = Literal['one', 'two', 'three']

    def validate(x: PossibleValues) -> bool:
        if x == 'one':
            return True
        elif x == 'two':
            return False
        # Error: Argument 1 to "assert_never" has incompatible type "Literal['three']";
        # expected "NoReturn"
        assert_never(x)
    ```

    If runtime checking against unexpected values is not needed, you can leave out the `assert_never` call in the above example, and mypy will still generate an error about function `validate` returning without a value:

    ```python
    PossibleValues = Literal['one', 'two', 'three']

    # Error: Missing return statement
    def validate(x: PossibleValues) -> bool:
        if x == 'one':
            return True
        elif x == 'two':
            return False
    ```

    Exhaustiveness checking is also supported for match statements (Python 3.10 and later):

    ```python
    def validate(x: PossibleValues) -> bool:
        match x:
            case 'one':
                return True
            case 'two':
                return False
        assert_never(x)
    ```

### 局限性

Limitations

=== "中文"

    Mypy 无法深入理解使用“Literal[..]”类型变量的表达式。 例如，如果您有一个类型为“Literal[3]”的变量“a”和另一个类型为“Literal[5]”的变量“b”，mypy 将推断“a + b”的类型为“int”，* *不是** 输入 `Literal[8]`。

    基本规则是文字类型被视为参数具有的任何类型的常规子类型。 例如，“Literal[3]”被视为“int”的子类型，因此将直接继承“int”的所有方法。 这意味着 `Literal[3].__add__` 接受与 `int.__add__` 相同的参数并具有相同的返回类型。

=== "英文"

    Mypy will not understand expressions that use variables of type `Literal[..]` on a deep level. For example, if you have a variable `a` of type `Literal[3]` and another variable `b` of type `Literal[5]`, mypy will infer that `a + b` has type `int`, **not** type `Literal[8]`.

    The basic rule is that literal types are treated as just regular subtypes of whatever type the parameter has. For example, `Literal[3]` is treated as a subtype of `int` and so will inherit all of `int`'s methods directly. This means that `Literal[3].__add__` accepts the same arguments and has the same return type as `int.__add__`.

## 枚举

Enums

=== "中文"

    Mypy 对 [`enum.Enum`](https://docs.python.org/3/library/enum.html#enum.Enum) 及其子类有特殊支持: [`enum.IntEnum`](https://docs.python.org/3/library/enum.html#enum.IntEnum), [`enum.Flag`](https://docs.python.org/3/library/enum.html#enum.Flag), [`enum.IntFlag`](https://docs.python.org/3/library/enum.html#enum.IntFlag), and [`enum.StrEnum`](https://docs.python.org/3/library/enum.html#enum.StrEnum).

    ```python
    from enum import Enum

    class Direction(Enum):
        up = 'up'
        down = 'down'

    reveal_type(Direction.up)  # Revealed type is "Literal[Direction.up]?"
    reveal_type(Direction.down)  # Revealed type is "Literal[Direction.down]?"
    ```

    You can use enums to annotate types as you would expect:

    ```python
    class Movement:
        def __init__(self, direction: Direction, speed: float) -> None:
            self.direction = direction
            self.speed = speed

    Movement(Direction.up, 5.0)  # ok
    Movement('up', 5.0)  # E: Argument 1 to "Movement" has incompatible type "str"; expected "Direction"
    ```

=== "英文"

    Mypy has special support for [`enum.Enum`](https://docs.python.org/3/library/enum.html#enum.Enum) and its subclasses: [`enum.IntEnum`](https://docs.python.org/3/library/enum.html#enum.IntEnum), [`enum.Flag`](https://docs.python.org/3/library/enum.html#enum.Flag), [`enum.IntFlag`](https://docs.python.org/3/library/enum.html#enum.IntFlag), and [`enum.StrEnum`](https://docs.python.org/3/library/enum.html#enum.StrEnum).

    ```python
    from enum import Enum

    class Direction(Enum):
        up = 'up'
        down = 'down'

    reveal_type(Direction.up)  # Revealed type is "Literal[Direction.up]?"
    reveal_type(Direction.down)  # Revealed type is "Literal[Direction.down]?"
    ```

    You can use enums to annotate types as you would expect:

    ```python
    class Movement:
        def __init__(self, direction: Direction, speed: float) -> None:
            self.direction = direction
            self.speed = speed

    Movement(Direction.up, 5.0)  # ok
    Movement('up', 5.0)  # E: Argument 1 to "Movement" has incompatible type "str"; expected "Direction"
    ```

### 详尽检查

Exhaustiveness checking

=== "中文"

    与“Literal”类型类似，“Enum”支持详尽检查。 让我们从一个定义开始：

    ```python
    from enum import Enum
    from typing import NoReturn

    def assert_never(value: NoReturn) -> NoReturn:
        # 这在运行时也有效：
        assert False, f'This code should never be reached, got: {value}'

    class Direction(Enum):
        up = 'up'
        down = 'down'
    ```

    Now, let's use an exhaustiveness check:

    ```python
    def choose_direction(direction: Direction) -> None:
        if direction is Direction.up:
            reveal_type(direction)  # N: Revealed type is "Literal[Direction.up]"
            print('Going up!')
            return
        elif direction is Direction.down:
            print('Down')
            return
        # 这条线永远不会到达
        assert_never(direction)
    ```

    If we forget to handle one of the cases, mypy will generate an error:

    ```python
    def choose_direction(direction: Direction) -> None:
        if direction == Direction.up:
            print('Going up!')
            return
        assert_never(direction)  # E: Argument 1 to "assert_never" has incompatible type "Direction"; expected "NoReturn"
    ```

    Exhaustiveness checking is also supported for match statements (Python 3.10 and later).

=== "英文"

    与“Literal”类型类似，“Enum”支持详尽检查。 让我们从一个定义开始：

    ```python
    from enum import Enum
    from typing import NoReturn

    def assert_never(value: NoReturn) -> NoReturn:
        # 这在运行时也有效：
        assert False, f'This code should never be reached, got: {value}'

    class Direction(Enum):
        up = 'up'
        down = 'down'
    ```

    现在，让我们使用详尽检查：

    ```python
    def choose_direction(direction: Direction) -> None:
        if direction is Direction.up:
            reveal_type(direction)  # N: Revealed type is "Literal[Direction.up]"
            print('Going up!')
            return
        elif direction is Direction.down:
            print('Down')
            return
        # 这条线永远不会到达
        assert_never(direction)
    ```

    If we forget to handle one of the cases, mypy will generate an error:

    ```python
    def choose_direction(direction: Direction) -> None:
        if direction == Direction.up:
            print('Going up!')
            return
        assert_never(direction)  # E: Argument 1 to "assert_never" has incompatible type "Direction"; expected "NoReturn"
    ```

    Exhaustiveness checking is also supported for match statements (Python 3.10 and later).

### 额外的枚举检查

Extra Enum checks

=== "中文"

    Mypy 还尝试像 Python 运行时一样支持“Enum”的特殊功能：

    - 任何带有值的“Enum”类都是隐式的[“final”](https://mypy.readthedocs.io/en/stable/final_attrs.html#final-attrs). 这是 CPython 中发生的情况：

    ```python
    >>> class AllDirection(Direction):
    ...     left = 'left'
    ...     right = 'right'
    Traceback (most recent call last):
        ...
    TypeError: AllDirection: cannot extend enumeration 'Direction'
    ```

    Mypy 也捕获了这个错误：

    ```python
    class AllDirection(Direction):  # E: Cannot inherit from final class "Direction"
        left = 'left'
        right = 'right'
    ```

    -所有“Enum”字段也隐式为“final”。

    ```python
    Direction.up = '^'  # E: Cannot assign to final attribute "up"
    ```

    - 检查所有字段名称是否唯一。

    ```python
    class Some(Enum):
        x = 1
        x = 2  # E: Attempted to reuse member name "x" in Enum definition "Some"
    ```

    - 基类没有冲突并且 mixin 类型是正确的。

    ```python
    class WrongEnum(str, int, enum.Enum):
        # E: Only a single data type mixin is allowed for Enum subtypes, found extra "int"
        ...

    class MixinAfterEnum(enum.Enum, Mixin): # E: No base classes are allowed after "enum.Enum"
        ...
    ```

=== "英文"

    Mypy also tries to support special features of `Enum` the same way Python's runtime does:

    - Any `Enum` class with values is implicitly [`final`](https://mypy.readthedocs.io/en/stable/final_attrs.html#final-attrs). This is what happens in CPython:

    ```python
    >>> class AllDirection(Direction):
    ...     left = 'left'
    ...     right = 'right'
    Traceback (most recent call last):
        ...
    TypeError: AllDirection: cannot extend enumeration 'Direction'
    ```

    Mypy also catches this error:

    ```python
    class AllDirection(Direction):  # E: Cannot inherit from final class "Direction"
        left = 'left'
        right = 'right'
    ```

    - All `Enum` fields are implicitly `final` as well.

    ```python
    Direction.up = '^'  # E: Cannot assign to final attribute "up"
    ```

    - All field names are checked to be unique.

    ```python
    class Some(Enum):
        x = 1
        x = 2  # E: Attempted to reuse member name "x" in Enum definition "Some"
    ```

    - Base classes have no conflicts and mixin types are correct.

    ```python
    class WrongEnum(str, int, enum.Enum):
        # E: Only a single data type mixin is allowed for Enum subtypes, found extra "int"
        ...

    class MixinAfterEnum(enum.Enum, Mixin): # E: No base classes are allowed after "enum.Enum"
        ...
    ```
