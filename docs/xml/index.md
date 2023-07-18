# Python中XML解析地图

原文: <https://realpython.com/python-xml-parser/>

=== "中文"

    如果您以前尝试过用 Python 解析 XML 文档，那么您就会知道这项任务有多么困难。 一方面，[Python 之禅](https://realpython.com/zen-of-python/)仅承诺一种显而易见的方法来实现您的目标。 同时，标准库遵循[“电池包含”](https://docs.python.org/3/tutorial/stdlib.html#batteries-included)的座右铭，让您从多个 XML 解析器中进行选择。 幸运的是，Python 社区通过创建更多的 XML 解析库解决了这个过剩问题。
    
    抛开笑话不谈，所有 XML 解析器都在充满或大或小的挑战的世界中占有一席之地。 熟悉可用的工具是值得的。
    
    **在本教程中，您将学习如何：**
    
    - 选择正确的 XML **解析模型**
    - 使用 **标准库** 中的 XML 解析器
    - 使用主要的 XML 解析**库**
    - 使用**数据绑定**以声明方式解析 XML 文档
    - 使用安全的 XML 解析器消除 **安全漏洞**
    
    您可以使用本教程作为路线图来指导您了解 Python 中 XML 解析器的混乱世界。 到最后，您将能够针对给定问题选择正确的 XML 解析器。 为了充分利用本教程，您应该已经熟悉 XML 及其构建块，以及如何在 Python 中使用文件。

=== "原文"

    If you’ve ever tried to parse an XML document in Python before, then you know how surprisingly difficult such a task can be. On the one hand, the [Zen of Python](https://realpython.com/zen-of-python/) promises only one obvious way to achieve your goal. At the same time, the standard library follows the [batteries included](https://docs.python.org/3/tutorial/stdlib.html#batteries-included) motto by letting you choose from not one but several XML parsers. Luckily, the Python community solved this surplus problem by creating even more XML parsing libraries.
    
    Jokes aside, all XML parsers have their place in a world full of smaller or bigger challenges. It’s worthwhile to familiarize yourself with the available tools.
    
    **In this tutorial, you’ll learn how to:**
    
    - Choose the right XML **parsing model**
    - Use the XML parsers in the **standard library**
    - Use major XML parsing **libraries**
    - Parse XML documents declaratively using **data binding**
    - Use safe XML parsers to eliminate **security vulnerabilities**
    
    You can use this tutorial as a roadmap to guide you through the confusing world of XML parsers in Python. By the end of it, you’ll be able to pick the right XML parser for a given problem. To get the most out of this tutorial, you should already be familiar with XML and its building blocks, as well as how to work with files in Python.

## 选择正确的 XML 解析模型

=== "中文"

    事实证明，您可以使用一些与语言无关的策略来处理 XML 文档。 每个都展示了不同的内存和速度权衡，这可以部分证明 Python 中可用的各种 XML 解析器的合理性。 在下一节中，您将了解它们的差异和优点。

=== "原文"

    It turns out that you can process XML documents using a few language-agnostic strategies. Each demonstrates different memory and speed trade-offs, which can partially justify the wide range of XML parsers available in Python. In the following section, you’ll find out their differences and strengths.

### 文档对象模型 (DOM)

=== "中文"

    从历史上看，第一个也是最广泛的解析 XML 的模型是 DOM，或者说是[文档对象模型](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)，最初定义 由万维网联盟 (W3C) 制定。 您可能已经听说过 DOM，因为 Web 浏览器通过 [JavaScript](https://realpython.com/python-vs-javascript/) 公开 DOM 接口，让您可以操作网站的 HTML 代码。 XML 和 HTML 都属于同一[标记语言](https://en.wikipedia.org/wiki/Markup_language)家族，这使得用 DOM 解析 XML 成为可能。

    DOM 可以说是最简单、最通用的模型。 它定义了一些**标准操作**，用于遍历和修改按对象层次结构排列的文档元素。 整个文档树的抽象表示存储在内存中，使您可以**随机访问**各个元素。
    
    虽然 DOM 树允许快速和**全向导航**，但首先构建其抽象表示可能非常耗时。 此外，XML 会作为一个整体**立即解析**，因此它必须相当小才能适合可用内存。 这使得 DOM 仅适用于中等大小的配置文件，而不适合多 GB 的 [XML 数据库](https://en.wikipedia.org/wiki/XML_database)。
    
    当方便比处理时间更重要并且内存不是问题时，请使用 DOM 解析器。 一些典型的用例是当您需要解析相对较小的文档或只需要不频繁地进行解析时。

=== "原文"

    Historically, the first and the most widespread model for parsing XML has been the DOM, or the [Document Object Model](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model), originally defined by the World Wide Web Consortium (W3C). You might have already heard about the DOM because web browsers expose a DOM interface through [JavaScript](https://realpython.com/python-vs-javascript/) to let you manipulate the HTML code of your websites. Both XML and HTML belong to the same family of [markup languages](https://en.wikipedia.org/wiki/Markup_language), which makes parsing XML with the DOM possible.

    The DOM is arguably the most straightforward and versatile model to use. It defines a handful of **standard operations** for traversing and modifying document elements arranged in a hierarchy of objects. An abstract representation of the entire document tree is stored in memory, giving you **random access** to the individual elements.
    
    While the DOM tree allows for fast and **omnidirectional navigation**, building its abstract representation in the first place can be time-consuming. Moreover, the XML gets **parsed at once**, as a whole, so it has to be reasonably small to fit the available memory. This renders the DOM suitable only for moderately large configuration files rather than multi-gigabyte [XML databases](https://en.wikipedia.org/wiki/XML_database).
    
    Use a DOM parser when convenience is more important than processing time and when memory is not an issue. Some typical use cases are when you need to parse a relatively small document or when you only need to do the parsing infrequently.

### XML 的简单 API (SAX)

=== "中文"

    为了解决 DOM 的缺点，Java 社区通过协作提出了一个库，该库随后成为解析其他语言中的 XML 的替代模型。 没有正式的规范，只有邮件列表上的有机讨论。 最终结果是一个**基于事件的流 API**，它按顺序对单个元素而不是整个树进行操作。

    元素按照它们在文档中出现的顺序从上到下进行处理。 解析器在文档中找到特定的 XML 节点时，会触发用户定义的[回调](https://en.wikipedia.org/wiki/Callback_(computer_programming))来处理它们。 这种方法称为**“推送”解析**，因为解析器将元素推送到您的函数。
    
    如果您对元素不感兴趣，SAX 还允许您丢弃它们。 这意味着它的内存占用比 DOM 低得多，并且可以处理任意大的文件，这非常适合**单遍处理**，例如索引、转换为其他格式等。
    
    然而，查找或修改随机树节点很麻烦，因为它通常需要多次遍历文档并跟踪访问的节点。 SAX 也不方便处理深度嵌套的元素。 最后，SAX 模型只允许**只读**解析。
    
    简而言之，SAX 在空间和时间上都很便宜，但在大多数情况下比 DOM 更难使用。 它非常适合解析非常大的文档或实时解析传入的 XML 数据。

=== "原文"

    To address the shortcomings of the DOM, the Java community came up with a library through a collaborative effort, which then became an alternative model for parsing XML in other languages. There was no formal specification, only organic discussions on a mailing list. The end result was an **event-based streaming API** that operates sequentially on individual elements rather than the whole tree.

    Elements are processed from top to bottom in the same order they appear in the document. The parser triggers user-defined [callbacks](https://en.wikipedia.org/wiki/Callback_(computer_programming)) to handle specific XML nodes as it finds them in the document. This approach is known as **“push” parsing** because elements are pushed to your functions by the parser.
    
    SAX also lets you discard elements if you’re not interested in them. This means it has a much lower memory footprint than DOM and can deal with arbitrarily large files, which is great for **single-pass processing** such as indexing, conversion to other formats, and so on.
    
    However, finding or modifying random tree nodes is cumbersome because it usually requires multiple passes on the document and tracking the visited nodes. SAX is also inconvenient for handling deeply nested elements. Finally, the SAX model just allows for **read-only** parsing.
    
    In short, SAX is cheap in terms of space and time but more difficult to use than DOM in most cases. It works well for parsing very large documents or parsing incoming XML data in real time.

### XML 流式 API (StAX)

=== "中文"

    虽然在 Python 中不太流行，但解析 XML 的第三种方法构建在 SAX 之上。 它扩展了**流**的思想，但使用了**“拉”解析**模型，这为您提供了更多控制权。 您可以将 StAX 视为一个[迭代器](https://docs.python.org/3/glossary.html#term-iterator) 通过 XML 文档推进 **游标对象**，其中按需自定义处理程序调用解析器 ，而不是相反。

    !!! info "Note"

        Note: 可以组合多个 XML 解析模型。 例如，您可以使用 SAX 或 StAX 在文档中快速查找有趣的数据，然后仅在内存中构建该特定分支的 DOM 表示。

    使用 StAX 可以让您更好地控制解析过程，并允许更方便的**状态管理**。 流中的事件仅在请求时才被使用，从而实现延迟评估。 除此之外，它的性能应该与 SAX 相当，具体取决于解析器的实现。

=== "原文"

    Although somewhat less popular in Python, this third approach to parsing XML builds on top of SAX. It extends the idea of **streaming** but uses a **“pull” parsing** model instead, which gives you more control. You can think of StAX as an [iterator](https://docs.python.org/3/glossary.html#term-iterator) advancing a **cursor object** through an XML document, where custom handlers call the parser on demand and not the other way around.

    !!! info "Note"

        Note: It’s possible to combine more than one XML parsing model. For example, you can use SAX or StAX to quickly find an interesting piece of data in the document and then build a DOM representation of only that particular branch in memory.

    Using StAX gives you more control over the parsing process and allows for more convenient **state management**. The events in the stream are only consumed when requested, enabling lazy evaluation. Other than that, its performance should be on par with SAX, depending on the parser implementation.

## 了解 Python 标准库中的 XML 解析器

=== "中文"

    在本节中，您将了解 Python 的内置 XML 解析器，几乎每个 Python 发行版都可以使用它们。 您将把这些解析器与示例 [可扩展矢量图形 (SVG)](https://en.wikipedia.org/wiki/Scalable_Vector_Graphics) 图像进行比较，这是一种基于 XML 的格式。 通过使用不同的解析器处理同一文档，您将能够选择最适合您的解析器。
    
    您将要保存在本地文件中以供参考的示例图像描绘了一张笑脸。 它由以下 XML 内容组成：

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="no"?>
    <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
      "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd" [
        <!ENTITY custom_entity "Hello">
    ]>
    <svg xmlns="http://www.w3.org/2000/svg"
      xmlns:inkscape="http://www.inkscape.org/namespaces/inkscape"
      viewBox="-105 -100 210 270" width="210" height="270">
      <inkscape:custom x="42" inkscape:z="555">Some value</inkscape:custom>
      <defs>
        <linearGradient id="skin" x1="0" x2="0" y1="0" y2="1">
          <stop offset="0%" stop-color="yellow" stop-opacity="1.0"/>
          <stop offset="75%" stop-color="gold" stop-opacity="1.0"/>
          <stop offset="100%" stop-color="orange" stop-opacity="1"/>
        </linearGradient>
      </defs>
      <g id="smiley" inkscape:groupmode="layer" inkscape:label="Smiley">
        <!-- Head -->
        <circle cx="0" cy="0" r="50"
          fill="url(#skin)" stroke="orange" stroke-width="2"/>
        <!-- Eyes -->
        <ellipse cx="-20" cy="-10" rx="6" ry="8" fill="black" stroke="none"/>
        <ellipse cx="20" cy="-10" rx="6" ry="8" fill="black" stroke="none"/>
        <!-- Mouth -->
        <path d="M-20 20 A25 25 0 0 0 20 20"
          fill="white" stroke="black" stroke-width="3"/>
      </g>
      <text x="-40" y="75">&custom_entity; &lt;svg&gt;!</text>
      <script>
        <![CDATA[
          console.log("CDATA disables XML parsing: <svg>")
          const smiley = document.getElementById("smiley")
          const eyes = document.querySelectorAll("ellipse")
          const setRadius = r => e => eyes.forEach(x => x.setAttribute("ry", r))
          smiley.addEventListener("mouseenter", setRadius(2))
          smiley.addEventListener("mouseleave", setRadius(8))
        ]]>
      </script>
    </svg>
    ```

    它以 **XML 声明** 开头，后跟 [文档类型定义 (DTD)](https://en.wikipedia.org/wiki/Document_type_definition) 和 `<svg>` **根元素** 。 DTD 是可选的，但如果您决定使用 XML 验证器，它可以帮助验证您的文档结构。 根元素指定 **默认命名空间** xmlns 以及用于特定于编辑器的元素和属性的**前缀命名空间** `xmlns:inkscape`。 该文件还包含：

    * 嵌套元素
    * 属性
    * 注释
    * 字符数据（CDATA）
    * 预定义和自定义实体

    继续，将 XML 保存在名为 smiley.svg 的文件中，然后使用现代 Web 浏览器打开它，该浏览器将运行末尾的 JavaScript 代码段：

    ![ASDF](./imgs/smiley_face.webp)

    该代码向图像添加了一个交互式组件。 当您将鼠标悬停在笑脸上时，它会眨眼睛。 如果您想使用方便的图形用户界面 (GUI) 编辑笑脸，则可以使用矢量图形编辑器（例如 [Adobe Illustrator](https://www.adobe.com/products/illustrator.html)  或 [Inkscape](https://inkscape.org/)打开该文件。

    !!! info "Note"

        Note: 与 JSON 或 YAML 不同，XML 的某些功能可能会被黑客利用。 Python 的 xml 包中提供的标准 XML 解析器不安全，并且容易受到[一系列攻击](https://docs.python.org/3/library/xml.html#xml-vulnerability)。 要安全地解析来自不受信任源的 XML 文档，最好选择安全的替代方案。 您可以跳至本教程的[最后一节](#defuse-the-xml-bomb-with-secure-parsers)以了解更多详细信息。

    值得注意的是，Python 的标准库定义了用于解析 XML 文档的**抽象接口**，同时允许您提供具体的解析器实现。 实际上，您很少这样做，因为 Python 捆绑了 Expat 库的绑定，该库是用 C 编写的广泛使用的开源 XML 解析器 . 标准库中的以下所有 Python 模块默认都在底层使用 Expat。
    
    不幸的是，虽然 Expat 解析器可以告诉您文档是否**格式良好**，但它无法根据 [XML 架构定义 (XSD)](https://en.wikipedia.org/wiki/XML_Schema_(W3C)) 或 [文档类型定义 (DTD)](https://en.wikipedia.org/wiki/Document_type_definition)进行文档结构验证。 为此，您必须使用稍后讨论的第三方库之一。

=== "原文"

    In this section, you’ll take a look at Python’s built-in XML parsers, which are available to you in nearly every Python distribution. You’re going to compare those parsers against a sample [Scalable Vector Graphics (SVG)](https://en.wikipedia.org/wiki/Scalable_Vector_Graphics) image, which is an XML-based format. By processing the same document with different parsers, you’ll be able to choose the one that suits you best.
    
    The sample image, which you’re about to save in a local file for reference, depicts a smiley face. It consists of the following XML content:

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="no"?>
    <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
      "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd" [
        <!ENTITY custom_entity "Hello">
    ]>
    <svg xmlns="http://www.w3.org/2000/svg"
      xmlns:inkscape="http://www.inkscape.org/namespaces/inkscape"
      viewBox="-105 -100 210 270" width="210" height="270">
      <inkscape:custom x="42" inkscape:z="555">Some value</inkscape:custom>
      <defs>
        <linearGradient id="skin" x1="0" x2="0" y1="0" y2="1">
          <stop offset="0%" stop-color="yellow" stop-opacity="1.0"/>
          <stop offset="75%" stop-color="gold" stop-opacity="1.0"/>
          <stop offset="100%" stop-color="orange" stop-opacity="1"/>
        </linearGradient>
      </defs>
      <g id="smiley" inkscape:groupmode="layer" inkscape:label="Smiley">
        <!-- Head -->
        <circle cx="0" cy="0" r="50"
          fill="url(#skin)" stroke="orange" stroke-width="2"/>
        <!-- Eyes -->
        <ellipse cx="-20" cy="-10" rx="6" ry="8" fill="black" stroke="none"/>
        <ellipse cx="20" cy="-10" rx="6" ry="8" fill="black" stroke="none"/>
        <!-- Mouth -->
        <path d="M-20 20 A25 25 0 0 0 20 20"
          fill="white" stroke="black" stroke-width="3"/>
      </g>
      <text x="-40" y="75">&custom_entity; &lt;svg&gt;!</text>
      <script>
        <![CDATA[
          console.log("CDATA disables XML parsing: <svg>")
          const smiley = document.getElementById("smiley")
          const eyes = document.querySelectorAll("ellipse")
          const setRadius = r => e => eyes.forEach(x => x.setAttribute("ry", r))
          smiley.addEventListener("mouseenter", setRadius(2))
          smiley.addEventListener("mouseleave", setRadius(8))
        ]]>
      </script>
    </svg>
    ```

    It starts with an **XML declaration**, followed by a [Document Type Definition (DTD)](https://en.wikipedia.org/wiki/Document_type_definition) and the `<svg>` **root element**. The DTD is optional, but it can help validate your document structure if you decide to use an XML validator. The root element specifies the **default namespace** xmlns as well as a **prefixed namespace** `xmlns:inkscape` for editor-specific elements and attributes. The document also contains:

    * Nested elements
    * Attributes
    * Comments
    * Character data (CDATA)
    * Predefined and custom entities

    Go ahead, save the XML in a file named smiley.svg, and open it using a modern web browser, which will run the JavaScript snippet present at the end:

    ![ASDF](./imgs/smiley_face.webp)

    The code adds an interactive component to the image. When you hover the mouse over the smiley face, it blinks its eyes. If you want to edit the smiley face using a convenient graphical user interface (GUI), then you can open the file using a vector graphics editor such as [Adobe Illustrator](https://www.adobe.com/products/illustrator.html) or [Inkscape](https://inkscape.org/).

    !!! info "Note"

        Note: Unlike JSON or YAML, some features of XML can be exploited by hackers. The standard XML parsers available in the xml package in Python are insecure and vulnerable to an [array of attacks](https://docs.python.org/3/library/xml.html#xml-vulnerabilities). To safely parse XML documents from an untrusted source, prefer secure alternatives. You can jump to the [last section](#defuse-the-xml-bomb-with-secure-parsers) in this tutorial for more details.

    It’s worth noting that Python’s standard library defines **abstract interfaces** for parsing XML documents while letting you supply concrete parser implementation. In practice, you rarely do that because Python bundles a binding for the [Expat](https://en.wikipedia.org/wiki/Expat_(library)) library, which is a widely used open-source XML parser written in C. All of the following Python modules in the standard library use Expat under the hood by default.
    
    Unfortunately, while the Expat parser can tell you if your document is **well-formed**, it can’t **validate** the structure of your documents against an [XML Schema Definition (XSD)](https://en.wikipedia.org/wiki/XML_Schema_(W3C)) or a [Document Type Definition (DTD)](https://en.wikipedia.org/wiki/Document_type_definition). For that, you’ll have to use one of the third-party libraries discussed later.

### xml.dom.minidom: 最小 DOM 实现

=== "中文"

    考虑到使用 DOM 解析 XML 文档可以说是最直接的，因此您在 Python 标准库中发现 DOM 解析器不会感到惊讶。 但令人惊讶的是，实际上有两个 DOM 解析器。
    
    xml.dom 包包含两个在 Python 中使用 DOM 的模块：

    1. xml.dom.minidom
    2. xml.dom.pulldom

    第一个是 DOM 接口的精简实现，符合相对较旧版本的 W3C 规范。 它提供了 DOM API 定义的常见对象，例如 Document、Element 和 Attr。 正如您即将发现的那样，该模块的文档很少，而且用途也相当有限。

    第二个模块的名称有点误导性，因为它定义了一个**流式拉解析器**，它可以选择生成文档树中当前节点的 DOM 表示。 您稍后将找到有关 pulldom 解析器的更多信息(https://realpython.com/python-xml-parser/#xmldompulldom-streaming-pull-parser)。
    
    minidom 中有两个函数可以让您解析来自各种数据源的 XML 数据。 一种接受文件名或[文件对象](https://docs.python.org/3/glossary.html#term-file-object)，而另一种接受[Python字符串](https://realpython.com/python-strings/):

    ```python
    >>> from xml.dom.minidom import parse, parseString

    >>> # 从文件名解析 XML
    >>> document = parse("smiley.svg")
    
    >>> # 从文件对象中解析 XML
    >>> with open("smiley.svg") as file:
    ...     document = parse(file)
    ...
    
    >>> # 从 Python 字符串解析 XML
    >>> document = parseString("""\
    ... <svg viewBox="-105 -100 210 270">
    ...   <!-- More content goes here... -->
    ... </svg>
    ... """)
    ```

    [三引号字符串](https://docs.python.org/3/glossary.html#term-triple-quoted-string) 有助于嵌入多行字符串文字，而无需在末尾使用连续字符 (\) 每行。 无论如何，您最终都会得到一个 Document 实例，它展示了熟悉的 DOM 界面，让您可以遍历树。
    
    除此之外，您还可以访问 XML 声明、DTD 和根元素：

    ```python
    >>> document = parse("smiley.svg")

    >>> # XML 声明
    >>> document.version, document.encoding, document.standalone
    ('1.0', 'UTF-8', False)
    
    >>> # 文档类型定义 (DTD)
    >>> dtd = document.doctype
    >>> dtd.entities["custom_entity"].childNodes
    [<DOM Text node "'Hello'">]
    
    >>> # 文档根节点
    >>> document.documentElement
    <DOM Element: svg at 0x7fc78c62d790>
    ```

    正如您所看到的，即使 Python 中默认的 XML 解析器无法验证文档，它仍然允许您检查 `.doctype`（DTD）（如果存在）。 请注意，XML 声明和 DTD 是可选的。 如果缺少 XML 声明或给定的 XML 属性，则相应的 Python 属性将为 [None](https://realpython.com/null-in-python/)。
    
    要通过 ID 查找元素，您必须使用 Document 实例而不是特定的父元素。 示例 SVG 图像有两个带有 id 属性的节点，但您找不到其中任何一个：

    ```python
    >>> document.getElementById("skin") is None
    True
    >>> document.getElementById("smiley") is None
    True
    ```

    对于只使用过 HTML 和 JavaScript 但以前没有使用过 XML 的人来说，这可能会感到惊讶。 虽然 HTML 定义了某些元素和属性（例如 <body> 或 id）的语义，但 XML 并未为其构建块附加任何含义。 您需要使用 DTD 或通过在 Python 中调用 `.setIdAttribute()` 显式地将属性标记为 ID，例如：

    | **定义风格** | **实现** |
    | -------------------- | -------------------|
    | DTD | <!ATTLIST linearGradient id ID #IMPLIED> |
    | Python | linearGradient.setIdAttribute("id")|

    但是，如果您的文档具有默认命名空间（示例 SVG 图像就是这种情况），那么使用 DTD 不足以解决问题。 为了解决这个问题，你可以在Python中递归地访问所有元素，检查它们是否具有 id 属性，并一次性将其指示为它们的ID：

    ```python
    >>> from xml.dom.minidom import parse, Node

    >>> def set_id_attribute(parent, attribute_name="id"):
    ...     if parent.nodeType == Node.ELEMENT_NODE:
    ...         if parent.hasAttribute(attribute_name):
    ...             parent.setIdAttribute(attribute_name)
    ...     for child in parent.childNodes:
    ...         set_id_attribute(child, attribute_name)
    ...
    >>> document = parse("smiley.svg")
    >>> set_id_attribute(document)
    ```

    您的自定义 `set_id_attribute()` 函数采用父元素和身份属性的可选名称，默认为 `id`。 当您在 SVG 文档上调用该函数时，所有具有 `id` 属性的子元素都将可以通过 DOM API 进行访问：

    ```python
    >>> document.getElementById("skin")
    <DOM Element: linearGradient at 0x7f82247703a0>
    
    >>> document.getElementById("smiley")
    <DOM Element: g at 0x7f8224770940>
    ```

    现在，您将获得与 `id` 属性值相对应的预期 XML 元素。
    
    使用 ID 最多可以查找一个唯一元素，但您也可以通过**标签名称**查找相似元素的集合。 与 `.getElementById()` 方法不同，您可以在文档或特定父元素上调用 `.getElementsByTagName()` 来缩小搜索范围：

    ```python
    >>> document.getElementsByTagName("ellipse")
    [
        <DOM Element: ellipse at 0x7fa2c944f430>,
        <DOM Element: ellipse at 0x7fa2c944f4c0>
    ]
    
    >>> root = document.documentElement
    >>> root.getElementsByTagName("ellipse")
    [
        <DOM Element: ellipse at 0x7fa2c944f430>,
        <DOM Element: ellipse at 0x7fa2c944f4c0>
    ]
    ```

    请注意，`.getElementsByTagName()` 始终返回元素的列表，而不是单个元素或 `None`。 在两种方法之间切换时忘记这一点是常见的错误来源。
    
    不幸的是，像 `<inkscape:custom>` 这样带有命名空间标识符**前缀**的元素不会被包含在内。 必须使用 `.getElementsByTagNameNS()` 来搜索它们，它需要不同的参数：

    ```python
    >>> document.getElementsByTagNameNS(
    ...     "http://www.inkscape.org/namespaces/inkscape",
    ...     "custom"
    ... )
    ...
    [<DOM Element: inkscape:custom at 0x7f97e3f2a3a0>]
    
    >>> document.getElementsByTagNameNS("*", "custom")
    [<DOM Element: inkscape:custom at 0x7f97e3f2a3a0>]
    ```

    第一个参数必须是 XML 命名空间，通常采用 [域名](https://en.wikipedia.org/wiki/Domain_name) 的形式，而第二个参数是标签名称。 请注意，命名空间前缀是无关紧要的！ 要搜索所有命名空间，您可以提供通配符 (*)。

    !!! info "Note"

        Note: 要查找 XML 文档中声明的命名空间，您可以检查根元素的属性。 理论上，它们可以在任何元素上声明，但您通常会在顶层元素中找到它们。

    一旦找到您感兴趣的元素，您就可以使用它在树上行走。 然而，minidom 的另一个令人不安的怪癖是它如何处理元素之间的**空白字符**：

    ```python
    >>> element = document.getElementById("smiley")

    >>> element.parentNode
    <DOM Element: svg at 0x7fc78c62d790>
    
    >>> element.firstChild
    <DOM Text node "'\n    '">
    
    >>> element.lastChild
    <DOM Text node "'\n  '">
    
    >>> element.nextSibling
    <DOM Text node "'\n  '">
    
    >>> element.previousSibling
    <DOM Text node "'\n  '">
    ```

    换行符和前导缩进被捕获为单独的树元素，这是规范所要求的。 有些解析器可以让你忽略这些，但 Python 不行。 但是，您可以做的是手动折叠此类节点中的空白：
    
    ```python
    >>> def remove_whitespace(node):
    ...     if node.nodeType == Node.TEXT_NODE:
    ...         if node.nodeValue.strip() == "":
    ...             node.nodeValue = ""
    ...     for child in node.childNodes:
    ...         remove_whitespace(child)
    ...
    >>> document = parse("smiley.svg")
    >>> set_id_attribute(document)
    >>> remove_whitespace(document)
    >>> document.normalize()
    ```

    请注意，您还必须调用 [`.normalize()`](https://docs.python.org/3/library/xml.dom.html#xml.dom.Node.normalize) 文档来组合相邻的文本节点 。 否则，您可能会得到一堆只有空格的冗余 XML 元素。 同样，递归是访问树元素的唯一方法，因为您无法使用循环迭代文档及其元素。 最后，这应该会给你预期的结果：

    ```python
    >>> element = document.getElementById("smiley")

    >>> element.parentNode
    <DOM Element: svg at 0x7fc78c62d790>
    
    >>> element.firstChild
    <DOM Comment node "' Head '">
    
    >>> element.lastChild
    <DOM Element: path at 0x7f8beea0f670>
    
    >>> element.nextSibling
    <DOM Element: text at 0x7f8beea0f700>
    
    >>> element.previousSibling
    <DOM Element: defs at 0x7f8beea0f160>
    
    >>> element.childNodes
    [
        <DOM Comment node "' Head '">,
        <DOM Element: circle at 0x7f8beea0f4c0>,
        <DOM Comment node "' Eyes '">,
        <DOM Element: ellipse at 0x7fa2c944f430>,
        <DOM Element: ellipse at 0x7fa2c944f4c0>,
        <DOM Comment node "' Mouth '">,
        <DOM Element: path at 0x7f8beea0f670>
    ]
    ```

    元素公开了一些有用的方法和属性，以便您查询其详细信息：

    ```python
    >>> element = document.getElementsByTagNameNS("*", "custom")[0]

    >>> element.prefix
    'inkscape'
    
    >>> element.tagName
    'inkscape:custom'
    
    >>> element.attributes
    <xml.dom.minidom.NamedNodeMap object at 0x7f6c9d83ba80>
    
    >>> dict(element.attributes.items())
    {'x': '42', 'inkscape:z': '555'}
    
    >>> element.hasChildNodes()
    True
    
    >>> element.hasAttributes()
    True
    
    >>> element.hasAttribute("x")
    True
    
    >>> element.getAttribute("x")
    '42'
    
    >>> element.getAttributeNode("x")
    <xml.dom.minidom.Attr object at 0x7f82244a05f0>
    
    >>> element.getAttribute("missing-attribute")
    ''
    ```

    例如，您可以检查元素的名称空间、标签名称或属性。 如果您要求缺少属性，那么您将得到一个空字符串 ('')。
    
    处理命名空间属性没有太大不同。 您只需记住相应地为属性名称添加前缀或提供域名：

    ```python
    >>> element.hasAttribute("z")
    False
    
    >>> element.hasAttribute("inkscape:z")
    True
    
    >>> element.hasAttributeNS(
    ...     "http://www.inkscape.org/namespaces/inkscape",
    ...     "z"
    ... )
    ...
    True
    
    >>> element.hasAttributeNS("*", "z")
    False
    ```

    奇怪的是，通配符 (*) 在这里不起作用，就像之前使用 `.getElementsByTagNameNS()` 方法一样。

    由于本教程仅涉及 XML 解析，因此您需要检查“minidom”文档以获取修改 DOM 树的方法。 它们大多遵循 W3C 规范。
    
    正如您所看到的，`minidom` 模块并不是很方便。 它的主要优点来自于它是标准库的一部分，这意味着您无需在项目中安装任何外部依赖项即可使用 DOM。

=== "原文"

    Considering that parsing XML documents using the DOM is arguably the most straightforward, you won’t be that surprised to find a DOM parser in the Python standard library. What is surprising, though, is that there are actually two DOM parsers.
    
    The xml.dom package houses two modules to work with DOM in Python:

    1. xml.dom.minidom
    2. xml.dom.pulldom

    The first is a stripped-down implementation of the DOM interface conforming to a relatively old version of the W3C specification. It provides common objects defined by the DOM API such as Document, Element, and Attr. This module is poorly documented and has quite limited usefulness, as you’re about to find out.

    The second module has a slightly misleading name because it defines a **streaming pull parser**, which can optionally produce a DOM representation of the current node in the document tree. You’ll find more information about the pulldom parser [later](https://realpython.com/python-xml-parser/#xmldompulldom-streaming-pull-parser).
    
    There are two functions in minidom that let you parse XML data from various data sources. One accepts either a filename or a [file object](https://docs.python.org/3/glossary.html#term-file-object), while another one expects a [Python string](https://realpython.com/python-strings/):

    ```python
    >>> from xml.dom.minidom import parse, parseString

    >>> # Parse XML from a filename
    >>> document = parse("smiley.svg")
    
    >>> # Parse XML from a file object
    >>> with open("smiley.svg") as file:
    ...     document = parse(file)
    ...
    
    >>> # Parse XML from a Python string
    >>> document = parseString("""\
    ... <svg viewBox="-105 -100 210 270">
    ...   <!-- More content goes here... -->
    ... </svg>
    ... """)
    ```

    The [triple-quoted string](https://docs.python.org/3/glossary.html#term-triple-quoted-string) helps embed a multiline string literal without using the continuation character (\) at the end of each line. In any case, you’ll end up with a Document instance, which exhibits the familiar DOM interface, letting you traverse the tree.
    
    Apart from that, you’ll be able to access the XML declaration, DTD, and the root element:

    ```python
    >>> document = parse("smiley.svg")

    >>> # XML Declaration
    >>> document.version, document.encoding, document.standalone
    ('1.0', 'UTF-8', False)
    
    >>> # Document Type Definition (DTD)
    >>> dtd = document.doctype
    >>> dtd.entities["custom_entity"].childNodes
    [<DOM Text node "'Hello'">]
    
    >>> # Document Root
    >>> document.documentElement
    <DOM Element: svg at 0x7fc78c62d790>
    ```

    As you can see, even though the default XML parser in Python can’t validate documents, it still lets you inspect .doctype, the DTD, if it’s present. Note that the XML declaration and DTD are optional. If the XML declaration or a given XML attribute is missing, then the corresponding Python attributes will be [None](https://realpython.com/null-in-python/).
    
    To find an element by ID, you must use the Document instance rather than a specific parent Element. The sample SVG image has two nodes with an id attribute, but you can’t find either of them:

    ```python
    >>> document.getElementById("skin") is None
    True
    >>> document.getElementById("smiley") is None
    True
    ```

    That may be surprising for someone who has only worked with HTML and JavaScript but hasn’t worked with XML before. While HTML defines the semantics for certain elements and attributes such as <body> or id, XML doesn’t attach any meaning to its building blocks. You need to mark an attribute as an ID explicitly using DTD or by calling .setIdAttribute() in Python, for example:

    | **Definition Style** | **Implementation** |
    | -------------------- | -------------------|
    | DTD | <!ATTLIST linearGradient id ID #IMPLIED> |
    | Python | linearGradient.setIdAttribute("id")|

    However, using a DTD isn’t enough to fix the problem if your document has a default namespace, which is the case for the sample SVG image. To address this, you can visit all elements [recursively](https://realpython.com/python-thinking-recursively/) in Python, check whether they have the id attribute, and indicate it as their ID in one go:

    ```python
    >>> from xml.dom.minidom import parse, Node

    >>> def set_id_attribute(parent, attribute_name="id"):
    ...     if parent.nodeType == Node.ELEMENT_NODE:
    ...         if parent.hasAttribute(attribute_name):
    ...             parent.setIdAttribute(attribute_name)
    ...     for child in parent.childNodes:
    ...         set_id_attribute(child, attribute_name)
    ...
    >>> document = parse("smiley.svg")
    >>> set_id_attribute(document)
    ```

    Your custom set_id_attribute() function takes a parent element and an optional name for the identity attribute, which defaults to "id". When you call that function on your SVG document, then all children elements that have an id attribute will become accessible through the DOM API:

    ```python
    >>> document.getElementById("skin")
    <DOM Element: linearGradient at 0x7f82247703a0>
    
    >>> document.getElementById("smiley")
    <DOM Element: g at 0x7f8224770940>
    ```

    Now, you’re getting the expected XML element corresponding to the id attribute’s value.
    
    Using an ID allows for finding at most one unique element, but you can also find a collection of similar elements by their **tag name**. Unlike the .getElementById() method, you can call `.getElementsByTagName()` on the document or a particular parent element to reduce the search scope:

    ```python
    >>> document.getElementsByTagName("ellipse")
    [
        <DOM Element: ellipse at 0x7fa2c944f430>,
        <DOM Element: ellipse at 0x7fa2c944f4c0>
    ]
    
    >>> root = document.documentElement
    >>> root.getElementsByTagName("ellipse")
    [
        <DOM Element: ellipse at 0x7fa2c944f430>,
        <DOM Element: ellipse at 0x7fa2c944f4c0>
    ]
    ```

    Notice that `.getElementsByTagName()` always returns a [list](https://realpython.com/python-lists-tuples/) of elements instead of a single element or None. Forgetting about it when you switch between both methods is a common source of errors.
    
    Unfortunately, elements like `<inkscape:custom>` that are **prefixed** with a namespace identifier won’t be included. They must be searched using `.getElementsByTagNameNS()`, which expects different arguments:

    ```python
    >>> document.getElementsByTagNameNS(
    ...     "http://www.inkscape.org/namespaces/inkscape",
    ...     "custom"
    ... )
    ...
    [<DOM Element: inkscape:custom at 0x7f97e3f2a3a0>]
    
    >>> document.getElementsByTagNameNS("*", "custom")
    [<DOM Element: inkscape:custom at 0x7f97e3f2a3a0>]
    ```

    The first argument must be the XML namespace, which typically has the form of a [domain name](https://en.wikipedia.org/wiki/Domain_name), while the second argument is the tag name. Notice that the namespace prefix is irrelevant! To search all namespaces, you can provide a wildcard character (*).

    !!! info "Note"

        Note: To find the namespaces declared in your XML document, you can check out the root element’s attributes. In theory, they could be declared on any element, but the top-level one is where you’d usually find them.

    Once you locate the element you’re interested in, you may use it to walk over the tree. However, another jarring quirk with minidom is how it handles **whitespace characters** between elements:

    ```python
    >>> element = document.getElementById("smiley")

    >>> element.parentNode
    <DOM Element: svg at 0x7fc78c62d790>
    
    >>> element.firstChild
    <DOM Text node "'\n    '">
    
    >>> element.lastChild
    <DOM Text node "'\n  '">
    
    >>> element.nextSibling
    <DOM Text node "'\n  '">
    
    >>> element.previousSibling
    <DOM Text node "'\n  '">
    ```

    The newline characters and leading indentation are captured as separate tree elements, which is what the specification requires. Some parsers let you ignore these, but not the Python one. What you can do, however, is collapse whitespace in such nodes manually:
    
    ```python
    >>> def remove_whitespace(node):
    ...     if node.nodeType == Node.TEXT_NODE:
    ...         if node.nodeValue.strip() == "":
    ...             node.nodeValue = ""
    ...     for child in node.childNodes:
    ...         remove_whitespace(child)
    ...
    >>> document = parse("smiley.svg")
    >>> set_id_attribute(document)
    >>> remove_whitespace(document)
    >>> document.normalize()
    ```

    Note that you also have to [`.normalize()`](https://docs.python.org/3/library/xml.dom.html#xml.dom.Node.normalize) the document to combine adjacent text nodes. Otherwise, you could end up with a bunch of redundant XML elements with just whitespace. Again, recursion is the only way to visit tree elements since you can’t iterate over the document and its elements with a loop. Finally, this should give you the expected result:

    ```python
    >>> element = document.getElementById("smiley")

    >>> element.parentNode
    <DOM Element: svg at 0x7fc78c62d790>
    
    >>> element.firstChild
    <DOM Comment node "' Head '">
    
    >>> element.lastChild
    <DOM Element: path at 0x7f8beea0f670>
    
    >>> element.nextSibling
    <DOM Element: text at 0x7f8beea0f700>
    
    >>> element.previousSibling
    <DOM Element: defs at 0x7f8beea0f160>
    
    >>> element.childNodes
    [
        <DOM Comment node "' Head '">,
        <DOM Element: circle at 0x7f8beea0f4c0>,
        <DOM Comment node "' Eyes '">,
        <DOM Element: ellipse at 0x7fa2c944f430>,
        <DOM Element: ellipse at 0x7fa2c944f4c0>,
        <DOM Comment node "' Mouth '">,
        <DOM Element: path at 0x7f8beea0f670>
    ]
    ```

    Elements expose a few helpful methods and properties to let you query their details:

    ```python
    >>> element = document.getElementsByTagNameNS("*", "custom")[0]

    >>> element.prefix
    'inkscape'
    
    >>> element.tagName
    'inkscape:custom'
    
    >>> element.attributes
    <xml.dom.minidom.NamedNodeMap object at 0x7f6c9d83ba80>
    
    >>> dict(element.attributes.items())
    {'x': '42', 'inkscape:z': '555'}
    
    >>> element.hasChildNodes()
    True
    
    >>> element.hasAttributes()
    True
    
    >>> element.hasAttribute("x")
    True
    
    >>> element.getAttribute("x")
    '42'
    
    >>> element.getAttributeNode("x")
    <xml.dom.minidom.Attr object at 0x7f82244a05f0>
    
    >>> element.getAttribute("missing-attribute")
    ''
    ```

    For instance, you can check an element’s namespace, tag name, or attributes. If you ask for a missing attribute, then you’ll get an empty string ('').
    
    Dealing with namespaced attributes isn’t much different. You just have to remember to prefix the attribute name accordingly or provide the domain name:

    ```python
    >>> element.hasAttribute("z")
    False
    
    >>> element.hasAttribute("inkscape:z")
    True
    
    >>> element.hasAttributeNS(
    ...     "http://www.inkscape.org/namespaces/inkscape",
    ...     "z"
    ... )
    ...
    True
    
    >>> element.hasAttributeNS("*", "z")
    False
    ```

    Strangely enough, the wildcard character (*) doesn’t work here as it did with the `.getElementsByTagNameNS()` method before.

    Since this tutorial is only about XML parsing, you’ll need to check the `minidom` documentation for methods that modify the DOM tree. They mostly follow the W3C specification.
    
    As you can see, the `minidom` module isn’t terribly convenient. Its main advantage comes from being part of the standard library, which means you don’t have to install any external dependencies in your project to work with the DOM.

### xml.sax: Python 的 SAX 接口

=== "中文"

    要开始在 Python 中使用 SAX，您可以使用与以前相同的 parse() 和 parseString() 便利函数，但使用 xml.sax 包中的函数。 您还必须至少提供一个必需的参数，该参数必须是一个**内容处理程序**实例。 本着 Java 的精神，您可以通过子类化特定基类来提供一个基类：

    ```python
    from xml.sax import parse
    from xml.sax.handler import ContentHandler
    
    class SVGHandler(ContentHandler):
        pass
    
    parse("smiley.svg", SVGHandler())
    ```

    内容处理程序在解析文档时接收与文档中的元素相对应的**事件流**。 运行此代码不会执行任何有用的操作，因为您的处理程序类是空的。 为了使其工作，您需要从超类重载一个或多个[回调方法](https://docs.python.org/3/library/xml.sax.handler.html#contenthandler-objects)。
    
    启动您最喜欢的编辑器，输入以下代码，并将其保存在名为“svg_handler.py”的文件中：

    ```python
    # svg_handler.py

    from xml.sax.handler import ContentHandler
    
    class SVGHandler(ContentHandler):
    
        def startElement(self, name, attrs):
            print(f"BEGIN: <{name}>, {attrs.keys()}")
    
        def endElement(self, name):
            print(f"END: </{name}>")
    
        def characters(self, content):
            if content.strip() != "":
                print("CONTENT:", repr(content))
    ```

    这个修改后的内容处理程序将一些事件[打印](https://realpython.com/python-print/)到标准输出上。 SAX 解析器将为您调用这三个方法来响应查找开始标记、结束标记以及它们之间的一些文本。 当您打开 Python 解释器的交互式会话时，导入您的内容处理程序并对其进行测试。 它应该产生以下输出：

    ```python
    >>> from xml.sax import parse
    >>> from svg_handler import SVGHandler
    >>> parse("smiley.svg", SVGHandler())
    BEGIN: <svg>, ['xmlns', 'xmlns:inkscape', 'viewBox', 'width', 'height']
    BEGIN: <inkscape:custom>, ['x', 'inkscape:z']
    CONTENT: 'Some value'
    END: </inkscape:custom>
    BEGIN: <defs>, []
    BEGIN: <linearGradient>, ['id', 'x1', 'x2', 'y1', 'y2']
    BEGIN: <stop>, ['offset', 'stop-color', 'stop-opacity']
    END: </stop>
    ⋮
    ```

    这本质上是[观察者设计模式](https://sourcemaking.com/design_patterns/observer)，它允许您将 XML 逐步转换为另一种分层格式。 假设您想将该 SVG 文件转换为简化的 [JSON](https://realpython.com/python-json/) 表示形式。 首先，您需要将内容处理程序对象存储在单独的变量中，以便稍后从中提取信息：

    ```python
    >>> from xml.sax import parse
    >>> from svg_handler import SVGHandler
    >>> handler = SVGHandler()
    >>> parse("smiley.svg", handler)
    ```

    由于 SAX 解析器发出事件时不提供有关其找到的元素的任何上下文，因此您需要跟踪您在树中的位置。 因此，将当前元素压入和弹出到[堆栈](https://realpython.com/how-to-implement-python-stack/)是有意义的，您可以通过常规[Python列表](https://realpython.com/python-lists-tuples/)。 您还可以定义一个辅助属性 `.current_element` ，它将返回放置在堆栈顶部的最后一个元素：

    ```python
    # svg_handler.py

    # ...
    
    class SVGHandler(ContentHandler):
    
        def __init__(self):
            super().__init__()
            self.element_stack = []
    
        @property
        def current_element(self):
            return self.element_stack[-1]
    
        # ...
    ```

    当 SAX 解析器发现新元素时，您可以立即捕获其标记名称和属性，同时为子元素和值创建占位符，这两者都是可选的。 现在，您可以将每个元素存储为`dict`对象。 将现有的 `.startElement()` 方法替换为新的实现：

    ```python
    # svg_handler.py

    # ...
    
    class SVGHandler(ContentHandler):
    
        # ...
    
        def startElement(self, name, attrs):
            self.element_stack.append({
                "name": name,
                "attributes": dict(attrs),
                "children": [],
                "value": ""
            })
    ```

    SAX 解析器为您提供属性作为[映射](https://docs.python.org/3/glossary.html#term-mapping)，您可以将其转换为普通的[Python 字典](https://realpython. com/python-dicts/) 并调用 `dict()` 函数。 元素值通常分布在多个部分中，您可以使用加号运算符 (+) 或相应的增强赋值语句将这些部分连接起来：

    ```python
    # svg_handler.py

    # ...
    
    class SVGHandler(ContentHandler):
    
        # ...
    
        def characters(self, content):
            self.current_element["value"] += content
    ```

    以这种方式聚合文本将确保多行内容最终出现在当前元素中。 例如，示例 SVG 文件中的`<script>`标记包含六行 JavaScript 代码，这些代码分别触发对`characters()` 回调的调用。

    最后，一旦解析器偶然发现结束标记，您就可以从堆栈中弹出当前元素并将其附加到其父元素的子元素中。 如果只剩下一个元素，那么它将是您应该保留以供以后使用的文档的根元素。 除此之外，您可能希望通过删除具有空值的键来清理当前元素：

    ```python
    # svg_handler.py

    # ...
    
    class SVGHandler(ContentHandler):
    
        # ...
    
        def endElement(self, name):
            clean(self.current_element)
            if len(self.element_stack) > 1:
                child = self.element_stack.pop()
                self.current_element["children"].append(child)
    
    def clean(element):
        element["value"] = element["value"].strip()
        for key in ("attributes", "children", "value"):
            if not element[key]:
                del element[key]
    ```

    请注意，`clean()`是在类主体之外定义的函数。 清理必须在最后完成，因为无法预先知道可能有多少文本片段需要连接。 您可以展开下面的可折叠部分以获取完整的内容处理程序代码。
    
    **用于 SVG 到 JSON 转换的 SAX 处理程序**

    ```python 
    # svg_handler.py

    from xml.sax.handler import ContentHandler
    
    class SVGHandler(ContentHandler):
    
        def __init__(self):
            super().__init__()
            self.element_stack = []
    
        @property
        def current_element(self):
            return self.element_stack[-1]
    
        def startElement(self, name, attrs):
            self.element_stack.append({
                "name": name,
                "attributes": dict(attrs),
                "children": [],
                "value": ""
            })
    
        def endElement(self, name):
            clean(self.current_element)
            if len(self.element_stack) > 1:
                child = self.element_stack.pop()
                self.current_element["children"].append(child)
    
        def characters(self, content):
            self.current_element["value"] += content
    
    def clean(element):
        element["value"] = element["value"].strip()
        for key in ("attributes", "children", "value"):
            if not element[key]:
                del element[key]
    ```

    现在，是时候通过解析 XML、从内容处理程序中提取根元素并将其转储到 JSON 字符串来测试所有内容了：

    ```python
    >>> from xml.sax import parse
    >>> from svg_handler import SVGHandler
    >>> handler = SVGHandler()
    >>> parse("smiley.svg", handler)
    >>> root = handler.current_element
    
    >>> import json
    >>> print(json.dumps(root, indent=4))
    {
        "name": "svg",
        "attributes": {
            "xmlns": "http://www.w3.org/2000/svg",
            "xmlns:inkscape": "http://www.inkscape.org/namespaces/inkscape",
            "viewBox": "-105 -100 210 270",
            "width": "210",
            "height": "270"
        },
        "children": [
            {
                "name": "inkscape:custom",
                "attributes": {
                    "x": "42",
                    "inkscape:z": "555"
                },
                "value": "Some value"
            },
    ⋮
    ```

    值得注意的是，与 DOM 相比，此实现没有内存增益，因为它像以前一样构建了整个文档的抽象表示。 不同之处在于您制作了自定义字典表示而不是标准 DOM 树。 但是，您可以想象在接收 SAX 事件时直接写入文件或数据库而不是内存。 这将有效地提高您的计算机内存限制。
    
    如果您想解析 XML 命名空间，那么您需要使用一些样板代码自行创建和配置 SAX 解析器，并实现略有不同的回调：

    ```python
    # svg_handler.py

    from xml.sax.handler import ContentHandler
    
    class SVGHandler(ContentHandler):
    
        def startPrefixMapping(self, prefix, uri):
            print(f"startPrefixMapping: {prefix=}, {uri=}")
    
        def endPrefixMapping(self, prefix):
            print(f"endPrefixMapping: {prefix=}")
    
        def startElementNS(self, name, qname, attrs):
            print(f"startElementNS: {name=}")
    
        def endElementNS(self, name, qname):
            print(f"endElementNS: {name=}")
    ```

    这些回调接收有关元素名称空间的附加参数。 要使 SAX 解析器实际触发这些回调而不是某些早期的回调，您必须显式启用 **XML 命名空间** 支持：

    ```python
    >>> from xml.sax import make_parser
    >>> from xml.sax.handler import feature_namespaces
    >>> from svg_handler import SVGHandler
    
    >>> parser = make_parser()
    >>> parser.setFeature(feature_namespaces, True)
    >>> parser.setContentHandler(SVGHandler())
    
    >>> parser.parse("smiley.svg")
    startPrefixMapping: prefix=None, uri='http://www.w3.org/2000/svg'
    startPrefixMapping: prefix='inkscape', uri='http://www.inkscape.org/namespaces/inkscape'
    startElementNS: name=('http://www.w3.org/2000/svg', 'svg')
    ⋮
    endElementNS: name=('http://www.w3.org/2000/svg', 'svg')
    endPrefixMapping: prefix='inkscape'
    endPrefixMapping: prefix=None
    ```

    设置此功能会将元素名称转换为由命名空间的域名和标签名称组成的元组。

    `xml.sax` 包提供了一个基于事件的 XML 解析器接口，该接口以原始 Java API 为模型。 与 DOM 相比，它有些限制，但应该足以实现基本的 XML 流推送解析器，而无需求助于第三方库。 考虑到这一点，Python 中有一个不太冗长的拉解析器，您接下来将探索它。

=== "原文"

    **xml.sax: The SAX Interface for Python**

    To start working with SAX in Python, you can use the same parse() and parseString() convenience functions as before, but from the xml.sax package instead. You also have to provide at least one more required argument, which must be a **content handler** instance. In the spirit of Java, you provide one by subclassing a specific base class:

    ```python
    from xml.sax import parse
    from xml.sax.handler import ContentHandler
    
    class SVGHandler(ContentHandler):
        pass
    
    parse("smiley.svg", SVGHandler())
    ```

    The content handler receives a **stream of events** corresponding to elements in your document as it’s being parsed. Running this code won’t do anything useful yet because your handler class is empty. To make it work, you’ll need to overload one or more [callback methods](https://docs.python.org/3/library/xml.sax.handler.html#contenthandler-objects) from the superclass.
    
    Fire up your favorite editor, type the following code, and save it in a file named `svg_handler.py`:

    ```python
    # svg_handler.py

    from xml.sax.handler import ContentHandler
    
    class SVGHandler(ContentHandler):
    
        def startElement(self, name, attrs):
            print(f"BEGIN: <{name}>, {attrs.keys()}")
    
        def endElement(self, name):
            print(f"END: </{name}>")
    
        def characters(self, content):
            if content.strip() != "":
                print("CONTENT:", repr(content))
    ```

    This modified content handler [prints](https://realpython.com/python-print/) out a few events onto the standard output. The SAX parser will call these three methods for you in response to finding the start tag, end tag, and some text between them. When you open an interactive session of the Python interpreter, import your content handler and give it a test drive. It should produce the following output:

    ```python
    >>> from xml.sax import parse
    >>> from svg_handler import SVGHandler
    >>> parse("smiley.svg", SVGHandler())
    BEGIN: <svg>, ['xmlns', 'xmlns:inkscape', 'viewBox', 'width', 'height']
    BEGIN: <inkscape:custom>, ['x', 'inkscape:z']
    CONTENT: 'Some value'
    END: </inkscape:custom>
    BEGIN: <defs>, []
    BEGIN: <linearGradient>, ['id', 'x1', 'x2', 'y1', 'y2']
    BEGIN: <stop>, ['offset', 'stop-color', 'stop-opacity']
    END: </stop>
    ⋮
    ```

    That’s essentially the [observer design pattern](https://sourcemaking.com/design_patterns/observer), which lets you translate XML into another hierarchical format incrementally. Say you wanted to convert that SVG file into a simplified [JSON](https://realpython.com/python-json/) representation. First, you’ll want to store your content handler object in a separate variable to extract information from it later:

    ```python
    >>> from xml.sax import parse
    >>> from svg_handler import SVGHandler
    >>> handler = SVGHandler()
    >>> parse("smiley.svg", handler)
    ```

    Since the SAX parser emits events without providing any context about the element it’s found, you need to keep track of where you are in the tree. Therefore, it makes sense to push and pop the current element onto a [stack](https://realpython.com/how-to-implement-python-stack/), which you can simulate through a regular [Python list](https://realpython.com/python-lists-tuples/). You may also define a helper property .current_element that will return the last element placed on the top of the stack:

    ```python
    # svg_handler.py

    # ...
    
    class SVGHandler(ContentHandler):
    
        def __init__(self):
            super().__init__()
            self.element_stack = []
    
        @property
        def current_element(self):
            return self.element_stack[-1]
    
        # ...
    ```

    When the SAX parser finds a new element, you can immediately capture its tag name and attributes while making placeholders for children elements and the value, both of which are optional. For now, you can store every element as a `dict` object. Replace your existing `.startElement()` method with a new implementation:

    ```python
    # svg_handler.py

    # ...
    
    class SVGHandler(ContentHandler):
    
        # ...
    
        def startElement(self, name, attrs):
            self.element_stack.append({
                "name": name,
                "attributes": dict(attrs),
                "children": [],
                "value": ""
            })
    ```

    The SAX parser gives you attributes as a [mapping](https://docs.python.org/3/glossary.html#term-mapping) that you can convert to a plain [Python dictionary](https://realpython.com/python-dicts/) with a call to the dict() function. The element value is often spread over multiple pieces that you can concatenate using the plus operator (+) or a corresponding augmented assignment statement:

    ```python
    # svg_handler.py

    # ...
    
    class SVGHandler(ContentHandler):
    
        # ...
    
        def characters(self, content):
            self.current_element["value"] += content
    ```

    Aggregating text in such a way will ensure that multiline content ends up in the current element. For example, the `<script>` tag in the sample SVG file contains six lines of JavaScript code, which trigger separate calls to the `characters()` callback.

    Finally, once the parser stumbles on a closing tag, you can pop the current element from the stack and append it to its parent’s children. If there’s only one element left, then it will be your document’s root that you should keep for later. Other than that, you might want to clean the current element by removing keys with empty values:

    ```python
    # svg_handler.py

    # ...
    
    class SVGHandler(ContentHandler):
    
        # ...
    
        def endElement(self, name):
            clean(self.current_element)
            if len(self.element_stack) > 1:
                child = self.element_stack.pop()
                self.current_element["children"].append(child)
    
    def clean(element):
        element["value"] = element["value"].strip()
        for key in ("attributes", "children", "value"):
            if not element[key]:
                del element[key]
    ```

    Note that `clean()` is a function defined outside of the class body. Cleaning must be done at the end since there’s no way of knowing up front how many text pieces to concatenate there might be. You can expand the collapsible section below for a complete content handler’s code.
    
    **SAX Hanlder for SVG to JSON Convert**

    ```python 
    # svg_handler.py

    from xml.sax.handler import ContentHandler
    
    class SVGHandler(ContentHandler):
    
        def __init__(self):
            super().__init__()
            self.element_stack = []
    
        @property
        def current_element(self):
            return self.element_stack[-1]
    
        def startElement(self, name, attrs):
            self.element_stack.append({
                "name": name,
                "attributes": dict(attrs),
                "children": [],
                "value": ""
            })
    
        def endElement(self, name):
            clean(self.current_element)
            if len(self.element_stack) > 1:
                child = self.element_stack.pop()
                self.current_element["children"].append(child)
    
        def characters(self, content):
            self.current_element["value"] += content
    
    def clean(element):
        element["value"] = element["value"].strip()
        for key in ("attributes", "children", "value"):
            if not element[key]:
                del element[key]
    ```

    Now, it’s time to put everything to the test by parsing the XML, extracting the root element from your content handler, and dumping it to a JSON string:

    ```python
    >>> from xml.sax import parse
    >>> from svg_handler import SVGHandler
    >>> handler = SVGHandler()
    >>> parse("smiley.svg", handler)
    >>> root = handler.current_element
    
    >>> import json
    >>> print(json.dumps(root, indent=4))
    {
        "name": "svg",
        "attributes": {
            "xmlns": "http://www.w3.org/2000/svg",
            "xmlns:inkscape": "http://www.inkscape.org/namespaces/inkscape",
            "viewBox": "-105 -100 210 270",
            "width": "210",
            "height": "270"
        },
        "children": [
            {
                "name": "inkscape:custom",
                "attributes": {
                    "x": "42",
                    "inkscape:z": "555"
                },
                "value": "Some value"
            },
    ⋮
    ```

    It’s worth noting that this implementation has no memory gain over DOM because it builds an abstract representation of the whole document just as before. The difference is that you’ve made a custom dictionary representation instead of the standard DOM tree. However, you could imagine writing directly to a file or a database instead of memory while receiving SAX events. That would effectively lift your computer memory limit.
    
    If you want to parse XML namespaces, then you’ll need to create and configure the SAX parser yourself with a bit of boilerplate code and also implement slightly different callbacks:

    ```python
    # svg_handler.py

    from xml.sax.handler import ContentHandler
    
    class SVGHandler(ContentHandler):
    
        def startPrefixMapping(self, prefix, uri):
            print(f"startPrefixMapping: {prefix=}, {uri=}")
    
        def endPrefixMapping(self, prefix):
            print(f"endPrefixMapping: {prefix=}")
    
        def startElementNS(self, name, qname, attrs):
            print(f"startElementNS: {name=}")
    
        def endElementNS(self, name, qname):
            print(f"endElementNS: {name=}")
    ```

    These callbacks receive additional parameters about the element’s namespace. To make the SAX parser actually trigger those callbacks instead of some of the earlier ones, you must explicitly enable **XML namespace** support:

    ```python
    >>> from xml.sax import make_parser
    >>> from xml.sax.handler import feature_namespaces
    >>> from svg_handler import SVGHandler
    
    >>> parser = make_parser()
    >>> parser.setFeature(feature_namespaces, True)
    >>> parser.setContentHandler(SVGHandler())
    
    >>> parser.parse("smiley.svg")
    startPrefixMapping: prefix=None, uri='http://www.w3.org/2000/svg'
    startPrefixMapping: prefix='inkscape', uri='http://www.inkscape.org/namespaces/inkscape'
    startElementNS: name=('http://www.w3.org/2000/svg', 'svg')
    ⋮
    endElementNS: name=('http://www.w3.org/2000/svg', 'svg')
    endPrefixMapping: prefix='inkscape'
    endPrefixMapping: prefix=None
    ```

    Setting this feature turns the element name into a tuple comprised of the namespace’s domain name and the tag name.

    The xml.sax package offers a decent event-based XML parser interface modeled after the original Java API. It’s somewhat limited compared to the DOM but should be enough to implement a basic XML streaming push parser without resorting to third-party libraries. With this in mind, there’s a less verbose pull parser available in Python, which you’ll explore next.

### xml.dom.pulldom: 流式拉取解析器

=== "中文"

    Python 标准库中的解析器经常一起工作。 例如，`xml.dom.pulldom` 模块包装了 `xml.sax` 中的解析器，以利用缓冲并以块的形式读取文档。 同时，它使用 `xml.dom.minidom` 中的默认 DOM 实现来表示文档元素。 然而，这些元素一次处理一个，不具有任何关系，直到您明确要求为止。

    !!! info "Note"

        **XML 命名空间支持在** xml.dom.pulldom 中默认启用。

    虽然 SAX 模型遵循[观察者模式](https://en.wikipedia.org/wiki/Observer_pattern)，但您可以将 StAX 视为[迭代器设计](https://sourcemaking.com/design_patterns/iterator) 模式，它允许您循环事件的**平坦流**。 再次，您可以调用从模块导入的熟悉的 `parse()` 或`parseString()`函数来解析 SVG 图像：

    ```python
    >>> from xml.dom.pulldom import parse
    >>> event_stream = parse("smiley.svg")
    >>> for event, node in event_stream:
    ...     print(event, node)
    ...
    START_DOCUMENT <xml.dom.minidom.Document object at 0x7f74f9283e80>
    START_ELEMENT <DOM Element: svg at 0x7f74fde18040>
    CHARACTERS <DOM Text node "'\n'">
    ⋮
    END_ELEMENT <DOM Element: script at 0x7f74f92b3c10>
    CHARACTERS <DOM Text node "'\n'">
    END_ELEMENT <DOM Element: svg at 0x7f74fde18040>
    ```

    只需几行代码即可解析文档。 `xml.sax` 和 `xml.dom`.pulldom 之间最显着的区别是缺少回调，因为您驱动整个过程。 您在构建代码时有更多的自由，如果您不想，则无需使用[类](https://realpython.com/python3-object-orient-programming/)。

    请注意，从流中提取的 XML 节点具有在`xml.dom.minidom`中定义的类型。 但如果你去检查他们的父级、兄弟节点和子节点，你会发现他们彼此一无所知：

    ```python
    >>> from xml.dom.pulldom import parse, START_ELEMENT
    >>> event_stream = parse("smiley.svg")
    >>> for event, node in event_stream:
    ...     if event == START_ELEMENT:
    ...         print(node.parentNode, node.previousSibling, node.childNodes)
    <xml.dom.minidom.Document object at 0x7f90864f6e80> None []
    None None []
    None None []
    None None []
    ⋮
    ```

    相关属性为空。 无论如何，pull 解析器可以帮助采用混合方法快速查找某些父元素并仅为以它为根的分支构建 DOM 树：

    ```python
    from xml.dom.pulldom import parse, START_ELEMENT

    def process_group(parent):
        left_eye, right_eye = parent.getElementsByTagName("ellipse")
        # ...
    
    event_stream = parse("smiley.svg")
    for event, node in event_stream:
        if event == START_ELEMENT:
            if node.tagName == "g":
                event_stream.expandNode(node)
                process_group(node)
    ```

    通过在事件流上调用 `.expandNode()` ，您实际上可以向前移动迭代器并递归地解析 XML 节点，直到找到父元素的匹配结束标记。 生成的节点将具有具有正确初始化属性的子节点。 此外，您将能够对它们使用 DOM 方法。

    Pull 解析器结合了 DOM 和 SAX 的优点，提供了一个有趣的替代方案。 它高效、灵活且易于使用，从而使代码更加紧凑和可读。 您还可以使用它更轻松地同时处理多个 XML 文件。 也就是说，到目前为止提到的 XML 解析器都无法与 Python 标准库中最后一个解析器的优雅、简单和完整性相媲美。

=== "原文"

    The parsers in the Python standard library often work together. For example, the `xml.dom.pulldom` module wraps the parser from `xml.sax` to take advantage of buffering and read the document in chunks. At the same time, it uses the default DOM implementation from `xml.dom.minidom` for representing document elements. However, those elements are processed one at a time without bearing any relationship until you ask for it explicitly.

    !!! info "Note"

        The **XML namespace support is enabled by default in** xml.dom.pulldom.

    While the SAX model follows the [observer pattern](https://en.wikipedia.org/wiki/Observer_pattern), you can think of StAX as the [iterator design](https://sourcemaking.com/design_patterns/iterator) pattern, which lets you loop over a **flat stream** of events. Once again, you can call the familiar `parse()` or `parseString()` functions imported from the module to parse the SVG image:

    ```python
    >>> from xml.dom.pulldom import parse
    >>> event_stream = parse("smiley.svg")
    >>> for event, node in event_stream:
    ...     print(event, node)
    ...
    START_DOCUMENT <xml.dom.minidom.Document object at 0x7f74f9283e80>
    START_ELEMENT <DOM Element: svg at 0x7f74fde18040>
    CHARACTERS <DOM Text node "'\n'">
    ⋮
    END_ELEMENT <DOM Element: script at 0x7f74f92b3c10>
    CHARACTERS <DOM Text node "'\n'">
    END_ELEMENT <DOM Element: svg at 0x7f74fde18040>
    ```

    It takes only a few lines of code to parse the document. The most striking difference between `xml.sax` and `xml.dom`.pulldom is the lack of callbacks since you drive the whole process. You have a lot more freedom in structuring your code, and you don’t need to use [classes](https://realpython.com/python3-object-oriented-programming/) if you don’t want to.

    Notice that the XML nodes pulled from the stream have types defined in `xml.dom.minidom`. But if you were to check their parents, siblings, and children, then you’d find out they know nothing about each other:

    ```python
    >>> from xml.dom.pulldom import parse, START_ELEMENT
    >>> event_stream = parse("smiley.svg")
    >>> for event, node in event_stream:
    ...     if event == START_ELEMENT:
    ...         print(node.parentNode, node.previousSibling, node.childNodes)
    <xml.dom.minidom.Document object at 0x7f90864f6e80> None []
    None None []
    None None []
    None None []
    ⋮
    ```

    The relevant attributes are empty. Anyway, the pull parser can help in a hybrid approach to quickly look up some parent element and build a DOM tree only for the branch rooted in it:

    ```python
    from xml.dom.pulldom import parse, START_ELEMENT

    def process_group(parent):
        left_eye, right_eye = parent.getElementsByTagName("ellipse")
        # ...
    
    event_stream = parse("smiley.svg")
    for event, node in event_stream:
        if event == START_ELEMENT:
            if node.tagName == "g":
                event_stream.expandNode(node)
                process_group(node)
    ```

    By calling `.expandNode()` on the event stream, you essentially move the iterator forward and parse XML nodes recursively until finding the matching closing tag of the parent element. The resulting node will have children with properly initialized attributes. Moreover, you’ll be able to use the DOM methods on them.

    The pull parser offers an interesting alternative to DOM and SAX by combining the best of both worlds. It’s efficient, flexible, and straightforward to use, leading to more compact and readable code. You could also use it to process multiple XML files at the same time more easily. That said, none of the XML parsers mentioned so far can match the elegance, simplicity, and completeness of the last one to arrive in Python’s standard library.

### xml.etree.ElementTree: 一个轻量级、更 Pythonic 的替代方案

=== "中文"

    到目前为止您所了解的 XML 解析器可以完成这项工作。 然而，它们不太符合 Python 的哲学，这并非偶然。 虽然 DOM 遵循 W3C 规范，而 SAX 是根据 Java API 建模的，但两者都不给人一种 Pythonic 的感觉。

    更糟糕的是，DOM 和 SAX 解析器都感觉过时，因为它们在 [CPython](https://realpython.com/cpython-source-code-guide/) 解释器中的一些代码已经二十多年没有改变了！ 在撰写本文时，它们的实现仍然不完整，并且[缺少 typeshed 存根](https://github.com/python/typeshed/issues/3787)，这会破坏[代码编辑器](https://github.com/python/typeshed/issues/3787)中的代码完成 /realpython.com/python-ides-code-editors-guide/）。

    同时，Python 2.5 带来了解析和编写 XML 文档的全新视角 — **ElementTree API**。 它是一个轻量级、高效、优雅且功能丰富的界面，甚至一些第三方库也可以在其上构建。 要开始使用它，您必须导入 `xml.etree.ElementTree` 模块，这有点拗口。 因此，习惯上定义一个**别名**，如下所示：

    ```python
    import xml.etree.ElementTree as ET
    ```

    在稍旧的代码中，您可能已经看到导入了 `cElementTree` 模块。 它的实现速度比用 C 编写的相同接口快几倍。如今，常规模块尽可能使用快速实现，因此您无需再烦恼。
    
    您可以通过采用不同的解析策略来使用 ElementTree API：
    
    | - | 非增量式 | 增量式 (阻塞) | 增量式 (非阻塞) |
    | --| ----------------|-------------------------|------------------------------|
    | ET.parse() | ✔️ | | |
    | ET.fromstring() | ✔️ | | |
    | ET.iterparse() | | ✔️ | |
    | ET.XMLPullParser | | | ✔️ |

    非增量策略以**类似 DOM 的方式**将整个文档加载到内存中。 模块中有两个适当命名的函数，允许解析具有 XML 内容的文件或 Python 字符串：

    ```python
    >>> import xml.etree.ElementTree as ET

    >>> # 从文件名解析 XML
    >>> ET.parse("smiley.svg")
    <xml.etree.ElementTree.ElementTree object at 0x7fa4c980a6a0>
    
    >>> # 从文件对象中解析 XML
    >>> with open("smiley.svg") as file:
    ...     ET.parse(file)
    ...
    <xml.etree.ElementTree.ElementTree object at 0x7fa4c96df340>
    
    >>> # 从 Python 字符串解析 XML
    >>> ET.fromstring("""\
    ... <svg viewBox="-105 -100 210 270">
    ...   <!-- More content goes here... -->
    ... </svg>
    ... """)
    <Element 'svg' at 0x7fa4c987a1d0>
    ```

    使用 `parse()` 解析文件对象或文件名会返回 [ET.ElementTree](https://docs.python.org/3/library/xml.etree.elementtree.html#xml.etree.ElementTree. ElementTree) 类，代表整个元素层次结构。 另一方面，使用 `fromstring()` 解析字符串将返回特定的根[ET.Element](https://docs.python.org/3/library/xml.etree.elementtree.html#xml.etree.ElementTree.Element)。
    
    或者，您可以使用流式 **拉式解析器** 增量读取 XML 文档，这会生成一系列事件和元素：

    ```python
    >>> for event, element in ET.iterparse("smiley.svg"):
    ...     print(event, element.tag)
    ...
    end {<http://www.inkscape.org/namespaces/inkscape}custom>
    end {<http://www.w3.org/2000/svg}stop>
    end {<http://www.w3.org/2000/svg}stop>
    end {<http://www.w3.org/2000/svg}stop>
    end {<http://www.w3.org/2000/svg}linearGradient>
    ⋮
    ```

    默认情况下，`iterparse()`仅发出与结束 XML 标记关联的结束事件。 但是，您也可以订阅其他事件。 您可以使用字符串常量（例如 `comment` ）找到它们：

    ```python
    >>> import xml.etree.ElementTree as ET
    >>> for event, element in ET.iterparse("smiley.svg", ["comment"]):
    ...     print(element.text.strip())
    ...
    Head
    Eyes
    Mouth
    ```

    以下是所有可用事件类型的列表：

    - **start**: 元素的开始
    - **end**: 元素结束
    - **comment**: 注释元素
    - **pi**: 处理指令，如 [XSL](https://en.wikipedia.org/wiki/XSL) 中
    - **start-ns**: 命名空间的开始
    - **end-ns**: 命名空间的结尾

    `iterparse()` 的缺点是它使用**阻塞调用**来读取下一个数据块，这可能不适合[异步代码](https://realpython.com/python-async-features/)在单个执行线程上运行。 为了缓解这种情况，您可以查看[XMLPullParser](https://docs.python.org/3/library/xml.etree.elementtree.html#pull-api-for-non-blocking-parsing)，这一个更详细一点：

    ```python
    import xml.etree.ElementTree as ET

    async def receive_data(url):
        """从 URL 异步下载字节块。"""
        yield b"<svg "
        yield b"viewBox=\"-105 -100 210 270\""
        yield b"></svg>"
    
    async def parse(url, events=None):
        parser = ET.XMLPullParser(events)
        async for chunk in receive_data(url):
            parser.feed(chunk)
            for event, element in parser.read_events():
                yield event, element
    ```

    这个假设的示例向解析器提供 XML 块，这些 XML 块可能会相隔几秒钟到达。 一旦有足够的内容，您就可以迭代解析器缓冲的一系列事件和元素。 这种**非阻塞**增量解析策略允许在下载多个 XML 文档时真正并发解析它们。
    
    树中的元素是可变的、可迭代的和可索引的[序列](https://docs.python.org/3/glossary.html#term-sequence)。 它们的长度与其直系子代的数量相对应：

    ```python
    >>> import xml.etree.ElementTree as ET
    >>> tree = ET.parse("smiley.svg")
    >>> root = tree.getroot()
    
    >>> # 元素的长度等于其子元素的数量。
    >>> len(root)
    5
    
    >>> # 方括号允许您通过索引访问子项。
    >>> root[1]
    <Element '{http://www.w3.org/2000/svg}defs' at 0x7fe05d2e8860>
    >>> root[2]
    <Element '{http://www.w3.org/2000/svg}g' at 0x7fa4c9848400>
    
    >>> # 元素是可变的。 例如，你可以交换他们的子节点。
    >>> root[2], root[1] = root[1], root[2]
    
    >>> # 您可以迭代元素的子元素。
    >>> for child in root:
    ...     print(child.tag)
    ...
    {http://www.inkscape.org/namespaces/inkscape}custom
    {http://www.w3.org/2000/svg}g
    {http://www.w3.org/2000/svg}defs
    {http://www.w3.org/2000/svg}text
    {http://www.w3.org/2000/svg}script
    ```

    标记名称可能以可选的命名空间为前缀，该命名空间括在一对大括号 (`{}`) 中。 定义时，默认的 XML 命名空间也会出现在那里。 请注意突出显示的行中的交换分配如何使 `<g>` 元素出现在 `<defs>` 之前。 这显示了序列的可变性质。
    
    这里还有一些值得一提的元素属性和方法：

    ```python
    >>> element = root[0]

    >>> element.tag
    '{<http://www.inkscape.org/namespaces/inkscape}custom>'
    
    >>> element.text
    'Some value'
    
    >>> element.attrib
    {'x': '42', '{<http://www.inkscape.org/namespaces/inkscape}z>': '555'}
    
    >>> element.get("x")
    '42'
    ```

    该 API 的好处之一是它使用 Python 的原生数据类型。 上面，它使用 Python 字典作为元素的属性。 在之前的模块中，这些模块被封装在不太方便的适配器中。 与 DOM 不同，ElementTree API 不公开用于在任何方向上遍历树的方法或属性，但有一些更好的替代方案。
    
    正如您之前所见，Element 类的实例实现了 **序列协议**，让您可以使用循环迭代它们的直接子级：

    ```python
    >>> for child in root:
    ...     print(child.tag)
    ...
    {<http://www.inkscape.org/namespaces/inkscape}custom>
    {<http://www.w3.org/2000/svg}defs>
    {<http://www.w3.org/2000/svg}g>
    {<http://www.w3.org/2000/svg}text>
    {<http://www.w3.org/2000/svg}script>
    ```

    您将获得根的直接子级的序列。 然而，要深入了解嵌套后代，您必须在祖先元素上调用 `.iter()` 方法：

    ```python
    >>> for descendant in root.iter():
    ...     print(descendant.tag)
    ...
    {http://www.w3.org/2000/svg}svg
    {http://www.inkscape.org/namespaces/inkscape}custom
    {http://www.w3.org/2000/svg}defs
    {http://www.w3.org/2000/svg}linearGradient
    {http://www.w3.org/2000/svg}stop
    {http://www.w3.org/2000/svg}stop
    {http://www.w3.org/2000/svg}stop
    {http://www.w3.org/2000/svg}g
    {http://www.w3.org/2000/svg}circle
    {http://www.w3.org/2000/svg}ellipse
    {http://www.w3.org/2000/svg}ellipse
    {http://www.w3.org/2000/svg}path
    {http://www.w3.org/2000/svg}text
    {http://www.w3.org/2000/svg}script
    ```
    根元素只有五个子节点，但总共有十三个后代元素。 还可以通过使用可选标签参数仅**过滤**特定标签名称来缩小后代范围：

    ```python
    >>> tag_name = "{http://www.w3.org/2000/svg}ellipse"
    >>> for descendant in root.iter(tag_name):
    ...     print(descendant)
    ...
    <Element '{http://www.w3.org/2000/svg}ellipse' at 0x7f430baa03b0>
    <Element '{http://www.w3.org/2000/svg}ellipse' at 0x7f430baa0450>
    ```

    这次，您只有两个 `<ellipse>` 元素。 请记住在标签名称中包含 **XML 命名空间**，例如 {`http://www.w3.org/2000/svg`} - 只要它已定义即可。 否则，如果您仅提供标签名称而没有正确的命名空间，则最终可能会得到比最初预期更少或更多的后代元素。
    
    使用 `.iterfind()` 处理命名空间会更方便，它接受前缀到域名的可选映射。 要指示 **默认命名空间**，您可以将键留空或分配任意前缀，稍后必须在标签名称中使用该前缀：

    ```python
    >>> namespaces = {
    ...     "": "<http://www.w3.org/2000/svg>",
    ...     "custom": "<http://www.w3.org/2000/svg>"
    ... }
    
    >>> for descendant in root.iterfind("g", namespaces):
    ...     print(descendant)
    ...
    <Element '{<http://www.w3.org/2000/svg}g>' at 0x7f430baa0270>
    
    >>> for descendant in root.iterfind("custom:g", namespaces):
    ...     print(descendant)
    ...
    <Element '{<http://www.w3.org/2000/svg}g>' at 0x7f430baa0270>
    ```

    命名空间映射允许您使用不同的前缀引用相同的元素。 令人惊讶的是，如果您尝试像以前一样查找那些嵌套的 `<ellipse>` 元素，那么 `.iterfind()` 将不会返回任何内容，因为它需要一个**XPath表达式**而不是一个简单的标签名称：

    ```python
    >>> for descendant in root.iterfind("ellipse", namespaces):
    ...     print(descendant)
    ...
    
    >>> for descendant in root.iterfind("g/ellipse", namespaces):
    ...     print(descendant)
    ...
    <Element '{<http://www.w3.org/2000/svg}ellipse>' at 0x7f430baa03b0>
    <Element '{<http://www.w3.org/2000/svg}ellipse>' at 0x7f430baa0450>
    ```

    巧合的是，字符串`g`恰好是相对于当前根元素的有效路径，这就是函数之前返回非空结果的原因。 但是，要查找 XML 层次结构中更深一层嵌套的省略号，您需要更详细的路径表达式。

    ElementTree 对 [XPath 迷你语言](https://www.w3.org/TR/xpath/)有[有限的语法支持](https://docs.python.org/3/library/xml.etree.elementtree.html#supported-xpath-syntax)，您可以使用它来查询 XML 中的元素，类似于 HTML 中的 CSS 选择器。 还有其他接受此类表达式的方法：

    ```python
    >>> namespaces = {"": "http://www.w3.org/2000/svg"}

    >>> root.iterfind("defs", namespaces)
    <generator object prepare_child.<locals>.select at 0x7f430ba6d190>
    
    >>> root.findall("defs", namespaces)
    [<Element '{http://www.w3.org/2000/svg}defs' at 0x7f430ba09e00>]
    
    >>> root.find("defs", namespaces)
    <Element '{<http://www.w3.org/2000/svg}defs>' at 0x7f430ba09e00>
    ```

    虽然 `.iterfind()` 延迟地生成匹配元素，但 `.findall()` 返回一个列表，而 `.find()` 仅返回第一个匹配元素。 类似地，您可以使用`.findtext()` 提取元素的开始和结束标记之间的文本，或者使用`.itertext()`获取整个文档的内部文本：

    ```python
    >>> namespaces = {"i": "http://www.inkscape.org/namespaces/inkscape"}

    >>> root.findtext("i:custom", namespaces=namespaces)
    'Some value'
    
    >>> for text in root.itertext():
    ...     if text.strip() != "":
    ...         print(text.strip())
    ...
    Some value
    Hello <svg>!
    console.log("CDATA disables XML parsing: <svg>")
    ⋮
    ```

    您首先查找嵌入在特定 XML 元素中的文本，然后查找整个文档中的任何位置。 按文本搜索是 ElementTree API 的一项强大功能。 可以使用其他内置解析器来复制它，但代价是增加代码复杂性和降低便利性。
    
    ElementTree API 可能是其中最直观的一个。 它具有 Python 风格、高效、健壮且通用。 除非您有特定原因使用 DOM 或 SAX，否则这应该是您的默认选择。

=== "原文"

    The XML parsers you’ve come to know so far get the job done. However, they don’t fit Python’s philosophy very well, and that’s no accident. While DOM follows the W3C specification and SAX was modeled after a Java API, neither feels particularly Pythonic.

    Even worse, both DOM and SAX parsers feel antiquated as some of their code in the [CPython](https://realpython.com/cpython-source-code-guide/) interpreter hasn’t changed for more than two decades! At the time of writing this, their implementation is still incomplete and has [missing typeshed stubs](https://github.com/python/typeshed/issues/3787), which breaks code completion in [code editors](https://realpython.com/python-ides-code-editors-guide/).

    Meanwhile, Python 2.5 brought a fresh perspective on parsing and writing XML documents—the **ElementTree API**. It’s a lightweight, efficient, elegant, and feature-rich interface that even some third-party libraries build on. To get started with it, you must import the `xml.etree.ElementTree` module, which is a bit of a mouthful. Therefore, it’s customary to define an **alias** like this:

    ```python
    import xml.etree.ElementTree as ET
    ```

    In slightly older code, you may have seen the `cElementTree` module imported instead. It was an implementation several times faster than the same interface written in C. Today, the regular module uses the fast implementation whenever possible, so you don’t need to bother anymore.
    
    You can use the ElementTree API by employing different parsing strategies:
    
    | - | Non-incremental | Incremental (Blocking) | Incremental (Non-blocking) |
    | --| ----------------|-------------------------|------------------------------|
    | ET.parse() | ✔️ | | |
    | ET.fromstring() | ✔️ | | |
    | ET.iterparse() | | ✔️ | |
    | ET.XMLPullParser | | | ✔️ |

    The non-incremental strategy loads up the entire document into memory in a **DOM-like fashion**. There are two appropriately named functions in the module that allow for parsing a file or a Python string with XML content:

    ```python
    >>> import xml.etree.ElementTree as ET

    >>> # Parse XML from a filename
    >>> ET.parse("smiley.svg")
    <xml.etree.ElementTree.ElementTree object at 0x7fa4c980a6a0>
    
    >>> # Parse XML from a file object
    >>> with open("smiley.svg") as file:
    ...     ET.parse(file)
    ...
    <xml.etree.ElementTree.ElementTree object at 0x7fa4c96df340>
    
    >>> # Parse XML from a Python string
    >>> ET.fromstring("""\
    ... <svg viewBox="-105 -100 210 270">
    ...   <!-- More content goes here... -->
    ... </svg>
    ... """)
    <Element 'svg' at 0x7fa4c987a1d0>
    ```

    Parsing a file object or a filename with parse() returns an instance of the [ET.ElementTree](https://docs.python.org/3/library/xml.etree.elementtree.html#xml.etree.ElementTree.ElementTree) class, which represents the whole element hierarchy. On the other hand, parsing a string with `fromstring()` will return the specific root [ET.Element](https://docs.python.org/3/library/xml.etree.elementtree.html#xml.etree.ElementTree.Element).
    
    Alternatively, you can read the XML document incrementally with a streaming **pull parser**, which yields a sequence of events and elements:

    ```python
    >>> for event, element in ET.iterparse("smiley.svg"):
    ...     print(event, element.tag)
    ...
    end {<http://www.inkscape.org/namespaces/inkscape}custom>
    end {<http://www.w3.org/2000/svg}stop>
    end {<http://www.w3.org/2000/svg}stop>
    end {<http://www.w3.org/2000/svg}stop>
    end {<http://www.w3.org/2000/svg}linearGradient>
    ⋮
    ```

    By default, `iterparse()` emits only the end events associated with the closing XML tag. However, you can subscribe to other events as well. You can find them with string constants such as `"comment"`:

    ```python
    >>> import xml.etree.ElementTree as ET
    >>> for event, element in ET.iterparse("smiley.svg", ["comment"]):
    ...     print(element.text.strip())
    ...
    Head
    Eyes
    Mouth
    ```

    Here’s a list of all the available event types:

    - **start**: Start of an element
    - **end**: End of an element
    - **comment**: Comment element
    - **pi**: Processing instruction, as in XSL
    - **start-ns**: Start of a namespace
    - **end-ns**: End of a namespace

    The downside of `iterparse()` is that it uses **blocking calls** to read the next chunk of data, which might be unsuitable for [asynchronous code](https://realpython.com/python-async-features/) running on a single thread of execution. To alleviate that, you can look into [XMLPullParser](https://docs.python.org/3/library/xml.etree.elementtree.html#pull-api-for-non-blocking-parsing), which is a little bit more verbose:

    ```python
    import xml.etree.ElementTree as ET

    async def receive_data(url):
        """Download chunks of bytes from the URL asynchronously."""
        yield b"<svg "
        yield b"viewBox=\"-105 -100 210 270\""
        yield b"></svg>"
    
    async def parse(url, events=None):
        parser = ET.XMLPullParser(events)
        async for chunk in receive_data(url):
            parser.feed(chunk)
            for event, element in parser.read_events():
                yield event, element
    ```

    This hypothetical example feeds the parser with chunks of XML that can arrive a few seconds apart. Once there’s enough content, you can iterate over a sequence of events and elements buffered by the parser. This **non-blocking** incremental parsing strategy allows for a truly concurrent parsing of multiple XML documents on the fly while you download them.
    
    Elements in the tree are mutable, iterable, and indexable [sequences](https://docs.python.org/3/glossary.html#term-sequence). They have a length corresponding to the number of their immediate children:

    ```python
    >>> import xml.etree.ElementTree as ET
    >>> tree = ET.parse("smiley.svg")
    >>> root = tree.getroot()
    
    >>> # The length of an element equals the number of its children.
    >>> len(root)
    5
    
    >>> # The square brackets let you access a child by an index.
    >>> root[1]
    <Element '{http://www.w3.org/2000/svg}defs' at 0x7fe05d2e8860>
    >>> root[2]
    <Element '{http://www.w3.org/2000/svg}g' at 0x7fa4c9848400>
    
    >>> # Elements are mutable. For example, you can swap their children.
    >>> root[2], root[1] = root[1], root[2]
    
    >>> # You can iterate over an element's children.
    >>> for child in root:
    ...     print(child.tag)
    ...
    {http://www.inkscape.org/namespaces/inkscape}custom
    {http://www.w3.org/2000/svg}g
    {http://www.w3.org/2000/svg}defs
    {http://www.w3.org/2000/svg}text
    {http://www.w3.org/2000/svg}script
    ```

    Tag names might be prefixed with an optional namespace enclosed in a pair of curly braces (`{}`). The default XML namespace appears there, too, when defined. Notice how the swap assignment in the highlighted line made the `<g>` element come before `<defs>`. This shows the mutable nature of the sequence.
    
    Here are a few more element attributes and methods that are worth mentioning:

    ```python
    >>> element = root[0]

    >>> element.tag
    '{<http://www.inkscape.org/namespaces/inkscape}custom>'
    
    >>> element.text
    'Some value'
    
    >>> element.attrib
    {'x': '42', '{<http://www.inkscape.org/namespaces/inkscape}z>': '555'}
    
    >>> element.get("x")
    '42'
    ```

    One of the benefits of this API is how it uses Python’s native data types. Above, it uses a Python dictionary for the element’s attributes. In the previous modules, those were wrapped in less convenient adapters. Unlike the DOM, the ElementTree API doesn’t expose methods or properties for walking over the tree in any direction, but there are a couple of better alternatives.
    
    As you’ve seen before, instances of the Element class implement the **sequence protocol**, letting you iterate over their direct children with a loop:

    ```python
    >>> for child in root:
    ...     print(child.tag)
    ...
    {<http://www.inkscape.org/namespaces/inkscape}custom>
    {<http://www.w3.org/2000/svg}defs>
    {<http://www.w3.org/2000/svg}g>
    {<http://www.w3.org/2000/svg}text>
    {<http://www.w3.org/2000/svg}script>
    ```

    You get the sequence of the root’s immediate children. To go deeper into nested descendants, however, you’ll have to call the `.iter()` method on the ancestor element:

    ```python
    >>> for descendant in root.iter():
    ...     print(descendant.tag)
    ...
    {http://www.w3.org/2000/svg}svg
    {http://www.inkscape.org/namespaces/inkscape}custom
    {http://www.w3.org/2000/svg}defs
    {http://www.w3.org/2000/svg}linearGradient
    {http://www.w3.org/2000/svg}stop
    {http://www.w3.org/2000/svg}stop
    {http://www.w3.org/2000/svg}stop
    {http://www.w3.org/2000/svg}g
    {http://www.w3.org/2000/svg}circle
    {http://www.w3.org/2000/svg}ellipse
    {http://www.w3.org/2000/svg}ellipse
    {http://www.w3.org/2000/svg}path
    {http://www.w3.org/2000/svg}text
    {http://www.w3.org/2000/svg}script
    ```
    The root element has only five children but thirteen descendants in total. It’s also possible to narrow down the descendants by **filtering** only specific tag names using an optional tag argument:

    ```python
    >>> tag_name = "{http://www.w3.org/2000/svg}ellipse"
    >>> for descendant in root.iter(tag_name):
    ...     print(descendant)
    ...
    <Element '{http://www.w3.org/2000/svg}ellipse' at 0x7f430baa03b0>
    <Element '{http://www.w3.org/2000/svg}ellipse' at 0x7f430baa0450>
    ```

    This time, you only got two `<ellipse>` elements. Remember to include the **XML namespace**, such as {`http://www.w3.org/2000/svg`}, in your tag name—as long as it’s been defined. Otherwise, if you only provide the tag name without the right namespace, you could end up with fewer or more descendant elements than initially anticipated.
    
    Dealing with namespaces is more convenient when using `.iterfind()`, which accepts an optional mapping of prefixes to domain names. To indicate the **default namespace**, you can leave the key blank or assign an arbitrary prefix, which must be used in the tag name later:

    ```python
    >>> namespaces = {
    ...     "": "<http://www.w3.org/2000/svg>",
    ...     "custom": "<http://www.w3.org/2000/svg>"
    ... }
    
    >>> for descendant in root.iterfind("g", namespaces):
    ...     print(descendant)
    ...
    <Element '{<http://www.w3.org/2000/svg}g>' at 0x7f430baa0270>
    
    >>> for descendant in root.iterfind("custom:g", namespaces):
    ...     print(descendant)
    ...
    <Element '{<http://www.w3.org/2000/svg}g>' at 0x7f430baa0270>
    ```

    The namespace mapping lets you refer to the same element with different prefixes. Surprisingly, if you try to find those nested `<ellipse>` elements like before, then `.iterfind()` won’t return anything because it expects an **XPath expression** rather than a simple tag name:

    ```python
    >>> for descendant in root.iterfind("ellipse", namespaces):
    ...     print(descendant)
    ...
    
    >>> for descendant in root.iterfind("g/ellipse", namespaces):
    ...     print(descendant)
    ...
    <Element '{<http://www.w3.org/2000/svg}ellipse>' at 0x7f430baa03b0>
    <Element '{<http://www.w3.org/2000/svg}ellipse>' at 0x7f430baa0450>
    ```

    By coincidence, the string "g" happens to be a valid path relative to the current root element, which is why the function returned a non-empty result before. However, to find the ellipses nested one level deeper in the XML hierarchy, you need a more verbose path expression.

    ElementTree has [limited syntax support](https://docs.python.org/3/library/xml.etree.elementtree.html#supported-xpath-syntax) for the [XPath mini-language](https://www.w3.org/TR/xpath/), which you can use to query elements in XML, similar to CSS selectors in HTML. There are other methods that accept such an expression:

    ```python
    >>> namespaces = {"": "http://www.w3.org/2000/svg"}

    >>> root.iterfind("defs", namespaces)
    <generator object prepare_child.<locals>.select at 0x7f430ba6d190>
    
    >>> root.findall("defs", namespaces)
    [<Element '{http://www.w3.org/2000/svg}defs' at 0x7f430ba09e00>]
    
    >>> root.find("defs", namespaces)
    <Element '{<http://www.w3.org/2000/svg}defs>' at 0x7f430ba09e00>
    ```

    While `.iterfind()` yields matching elements lazily, `.findall()` returns a list, and `.find()` returns only the first matching element. Similarly, you can extract text enclosed between the opening and closing tags of elements using `.findtext()` or get the inner text of the entire document with `.itertext()`:

    ```python
    >>> namespaces = {"i": "http://www.inkscape.org/namespaces/inkscape"}

    >>> root.findtext("i:custom", namespaces=namespaces)
    'Some value'
    
    >>> for text in root.itertext():
    ...     if text.strip() != "":
    ...         print(text.strip())
    ...
    Some value
    Hello <svg>!
    console.log("CDATA disables XML parsing: <svg>")
    ⋮
    ```

    You look for text embedded in a specific XML element first, then everywhere in the whole document. Searching by text is a powerful feature of the ElementTree API. It’s possible to replicate it using other built-in parsers, but at the cost of increased code complexity and less convenience.
    
    The ElementTree API is probably the most intuitive one of them all. It’s Pythonic, efficient, robust, and universal. Unless you have a specific reason to use DOM or SAX, this should be your default choice.

## 探索第三方 XML 解析器库

=== "中文"

    有时，使用标准库中的 XML 解析器可能会让人感觉像是拿起大锤来破解坚果。 在其他时候，情况恰恰相反，您希望解析器可以做更多的事情。 例如，您可能希望根据架构验证 XML 或使用高级 XPath 表达式。 在这些情况下，最好查看 [PyPI](https://pypi.org/) 上可用的外部库。
    
    下面，您将找到一系列具有不同复杂程度和复杂程度的外部库。

=== "原文"

    Occasionally, reaching for the XML parsers in the standard library might feel like picking up a sledgehammer to crack a nut. At other times, it’s the opposite, and you wish for a parser that could do much more. For example, you might want to validate XML against a schema or use advanced XPath expressions. In those situations, it’s best to check out the external libraries available on [PyPI](https://pypi.org/).
    
    Below, you’ll find a selection of external libraries with varying degrees of complexity and sophistication.

### untangle: 将 XML 转换为 Python 对象

=== "中文"

    如果您正在寻找一种可以将 XML 文档转换为 Python 对象的单行代码，那么您就不用再犹豫了。 虽然 [untangle](https://pypi.org/project/untangle/) 库已经几年没有更新了，但它可能很快就会成为您在 Python 中解析 XML 的最喜欢的方式。 只需记住一个函数，它接受 URL、文件名、文件对象或 XML 字符串：

    ```python
    >>> import untangle

    >>> # Parse XML from a URL
    >>> untangle.parse("http://localhost:8000/smiley.svg")
    Element(name = None, attributes = None, cdata = )
    
    >>> # Parse XML from a filename
    >>> untangle.parse("smiley.svg")
    Element(name = None, attributes = None, cdata = )
    
    >>> # Parse XML from a file object
    >>> with open("smiley.svg") as file:
    ...     untangle.parse(file)
    ...
    Element(name = None, attributes = None, cdata = )
    
    >>> # Parse XML from a Python string
    >>> untangle.parse("""\
    ... <svg viewBox="-105 -100 210 270">
    ...   <!-- More content goes here... -->
    ... </svg>
    ... """)
    Element(name = None, attributes = None, cdata = )
    ```

    在每种情况下，它都会返回 `Element` 类的一个实例。 您可以使用 **点运算符** 来访问其子节点，并使用 **方括号** 语法来按索引获取 XML 属性或子节点之一。 例如，要获取文档的根元素，您可以像访问对象的属性一样访问它。 要获取元素的 XML 属性之一，您可以将其名称作为字典键传递：

    ```python
    >>> import untangle
    >>> document = untangle.parse("smiley.svg")
    
    >>> document.svg
    Element(name = svg, attributes = {'xmlns': ...}, ...)
    
    >>> document.svg["viewBox"]
    '-105 -100 210 270'
    ```

    没有需要记住的函数或方法名称。 相反，每个解析的对象都是唯一的，因此您确实需要了解底层 XML 文档的结构才能使用 `untangle` 遍历它。

    要找出根元素的名称，请在文档上调用 `dir()` ：

    ```python
    >>> dir(document)
    ['svg']
    ```

    这揭示了该元素的直接子元素的名称。 请注意，`untangle` 为其解析的文档重新定义了 `dir()` 的含义。 通常，您调用此内置函数来检查类或 Python 模块。 默认实现将返回属性名称列表，而不是 XML 文档的子元素。

    如果有多个子级具有给定的标签名称，那么您可以使用循环迭代它们或通过索引引用其中一个：

    ```python
    >>> dir(document.svg)
    ['defs', 'g', 'inkscape_custom', 'script', 'text']
    
    >>> dir(document.svg.defs.linearGradient)
    ['stop', 'stop', 'stop']
    
    >>> for stop in document.svg.defs.linearGradient.stop:
    ...     print(stop)
    ...
    Element <stop> with attributes {'offset': ...}, ...
    Element <stop> with attributes {'offset': ...}, ...
    Element <stop> with attributes {'offset': ...}, ...
    
    >>> document.svg.defs.linearGradient.stop[1]
    Element(name = stop, attributes = {'offset': ...}, ...)
    ```

    您可能已经注意到，`<inkscape:custom>` 元素已重命名为 `inkscape_custom`。 不幸的是，该库无法很好地处理 **XML 命名空间**，因此，如果您需要依赖它，那么您必须寻找其他地方。

    由于点表示法，XML 文档中的元素名称必须是有效的 [Python 标识符](https://docs.python.org/3/reference/lexical_analysis.html#identifiers)。 如果不是，那么 `untangle` 会自动重写它们的名称，用下划线替换禁止的字符：

    ```python
    >>> dir(untangle.parse("<com:company.web-app></com:company.web-app>"))
    ['com_company_web_app']
    ```

    子标签名称并不是您可以访问的唯一对象属性。 元素有一些预定义的对象属性，可以通过调用 `vars()` 来显示：

    ```python
    >>> element = document.svg.text

    >>> list(vars(element).keys())
    ['_name', '_attributes', 'children', 'is_root', 'cdata']
    
    >>> element._name
    'text'
    
    >>> element._attributes
    {'x': '-40', 'y': '75'}
    
    >>> element.children
    []
    
    >>> element.is_root
    False
    
    >>> element.cdata
    'Hello <svg>!'
    ```

    在幕后，`untangle` 使用内置的 SAX 解析器，但由于该库是用纯 Python 实现的，并创建了大量重量级对象，因此它的性能相当差。 虽然它旨在读取小型文档，但您仍然可以将其与另一种方法结合起来读取数 GB XML 文件。
    
    就是这样。 如果您前往[维基百科档案](https://dumps.wikimedia.org/enwiki/latest/)，您可以下载其中一个压缩的 XML 文件。 顶部的内容应包含文章摘要的快照：

    ```xml
    <feed>
      <doc>
        <title>Wikipedia: Anarchism</title>
        <url>https://en.wikipedia.org/wiki/Anarchism</url>
        <abstract>Anarchism is a political philosophy...</abstract>
        <links>
          <sublink linktype="nav">
            <anchor>Etymology, terminology and definition</anchor>
            <link>https://en.wikipedia.org/wiki/Anarchism#Etymology...</link>
          </sublink>
          <sublink linktype="nav">
            <anchor>History</anchor>
            <link>https://en.wikipedia.org/wiki/Anarchism#History</link>
          </sublink>
          ⋮
        </links>
      </doc>
      ⋮
    </feed>
    ```

    下载后大小超过 6 GB，非常适合本练习。 这个练习是扫描文件以查找连续的开始和结束 `<doc>` 标记，然后为了方便起见使用 `untangle` 解析它们之间的 XML 片段。
    
    内置的 [mmap](https://realpython.com/python-mmap/) 模块可让您创建文件内容的**虚拟视图**，即使它不适合可用内存。 这给人一种使用支持搜索和常规切片语法的巨大字节字符串的印象。 如果您对如何将此逻辑封装在 [Python 类](https://realpython.com/python-classes/) 中并利用 [生成器](https://realpython.com/introduction-to-python-generators/) 进行惰性评估，然后展开下面的可折叠部分。

    <!--这个语法在内容渲染时可以收缩起来-->
    ??? abstract "**A Hybrid Approach to Parsing XML**"

        Here’s the complete code of the XMLTagStream class:
    
        ```python
        import mmap
        import untangle
        
        class XMLTagStream:
            def __init__(self, path, tag_name, encoding="utf-8"):
                self.file = open(path)
                self.stream = mmap.mmap(
                    self.file.fileno(), 0, access=mmap.ACCESS_READ
                )
                self.tag_name = tag_name
                self.encoding = encoding
                self.start_tag = f"<{tag_name}>".encode(encoding)
                self.end_tag = f"</{tag_name}>".encode(encoding)
        
            def __enter__(self):
                return self
        
            def __exit__(self, *args, **kwargs):
                self.stream.close()
                self.file.close()
        
            def __iter__(self):
                end = 0
                while (begin := self.stream.find(self.start_tag, end)) != -1:
                    end = self.stream.find(self.end_tag, begin)
                    yield self.parse(self.stream[begin: end + len(self.end_tag)])
        
            def parse(self, chunk):
                document = untangle.parse(chunk.decode(self.encoding))
                return getattr(document, self.tag_name)
        ```
    
        它是一个自定义的[上下文管理器](https://realpython.com/python-with-statement/)，它使用定义为内联**生成器函数**的**迭代器协议**。 生成的生成器对象在 XML 文档上循环，就好像它是一长串字符一样。
        
        请注意，while 循环利用了相当新的 Python 语法，即 [walrus 运算符 (:=)](https://realpython.com/python-walrus-operator/) 来简化代码。 您可以在**赋值表达式**中使用此运算符，其中可以计算表达式并将其分配给变量。

    在不深入讨论具体细节的情况下，您可以使用以下自定义类快速浏览大型 XML 文件，同时通过 untangle 更彻底地检查特定元素：

    ```python
    >>> with XMLTagStream("abstract.xml", "doc") as stream:
    ...     for doc in stream:
    ...         print(doc.title.cdata.center(50, "="))
    ...         for sublink in doc.links.sublink:
    ...             print("-", sublink.anchor.cdata)
    ...         if "q" == input("Press [q] to exit or any key to continue..."):
    ...             break
    ...
    ===============Wikipedia: Anarchism===============
    - Etymology, terminology and definition
    - History
    - Pre-modern era
    ⋮
    Press [q] to exit or any key to continue...
    ================Wikipedia: Autism=================
    - Characteristics
    - Social development
    - Communication
    ⋮
    Press [q] to exit or any key to continue...
    ```

    首先，打开一个文件进行读取并指出要查找的标签名称。 然后，您迭代这些元素并接收 XML 文档的已解析片段。 这几乎就像透过一个小窗口在一张无限长的纸上移动一样。 这是一个相对浅显的示例，忽略了一些细节，但它应该让您大致了解如何使用这种混合解析策略。

=== "原文"

    If you’re looking for a one-liner that could turn your XML document into a Python object, then look no further. While it hasn’t been updated in a few years, the [untangle](https://pypi.org/project/untangle/) library might soon become your favorite way of parsing XML in Python. There’s only one function to remember, and it accepts a URL, a filename, a file object, or an XML string:

    ```python
    >>> import untangle

    >>> # Parse XML from a URL
    >>> untangle.parse("http://localhost:8000/smiley.svg")
    Element(name = None, attributes = None, cdata = )
    
    >>> # Parse XML from a filename
    >>> untangle.parse("smiley.svg")
    Element(name = None, attributes = None, cdata = )
    
    >>> # Parse XML from a file object
    >>> with open("smiley.svg") as file:
    ...     untangle.parse(file)
    ...
    Element(name = None, attributes = None, cdata = )
    
    >>> # Parse XML from a Python string
    >>> untangle.parse("""\
    ... <svg viewBox="-105 -100 210 270">
    ...   <!-- More content goes here... -->
    ... </svg>
    ... """)
    Element(name = None, attributes = None, cdata = )
    ```

    In each case, it returns an instance of the Element class. You can use the **dot operator** to access its children and the **square bracket** syntax to get XML attributes or one of the child nodes by index. To get the document’s root element, for example, you can access it as if it was the object’s property. To get one of the element’s XML attributes, you may pass its name as a dictionary key:

    ```python
    >>> import untangle
    >>> document = untangle.parse("smiley.svg")
    
    >>> document.svg
    Element(name = svg, attributes = {'xmlns': ...}, ...)
    
    >>> document.svg["viewBox"]
    '-105 -100 210 270'
    ```

    There are no function or method names to remember. Instead, each parsed object is unique, so you really need to know the underlying XML document’s structure to traverse it with untangle.

    To find out what the root element’s name is, call `dir()` on the document:

    ```python
    >>> dir(document)
    ['svg']
    ```

    This reveals the names of the element’s immediate children. Note that `untangle` redefines the meaning of `dir()` for its parsed documents. Usually, you call this built-in function to inspect a class or a Python module. The default implementation would return a list of attribute names rather than the child elements of an XML document.

    If there’s more than one child with the given tag name, then you can iterate over them with a loop or refer to one by index:

    ```python
    >>> dir(document.svg)
    ['defs', 'g', 'inkscape_custom', 'script', 'text']
    
    >>> dir(document.svg.defs.linearGradient)
    ['stop', 'stop', 'stop']
    
    >>> for stop in document.svg.defs.linearGradient.stop:
    ...     print(stop)
    ...
    Element <stop> with attributes {'offset': ...}, ...
    Element <stop> with attributes {'offset': ...}, ...
    Element <stop> with attributes {'offset': ...}, ...
    
    >>> document.svg.defs.linearGradient.stop[1]
    Element(name = stop, attributes = {'offset': ...}, ...)
    ```

    You might have noticed that the `<inkscape:custom>` element was renamed to `inkscape_custom`. Unfortunately, the library can’t handle **XML namespaces** well, so if that’s something you need to rely on, then you must look elsewhere.

    Because of the dot notation, element names in XML documents must be valid [Python identifiers](https://docs.python.org/3/reference/lexical_analysis.html#identifiers). If they’re not, then untangle will automatically rewrite their names by replacing forbidden characters with an underscore:

    ```python
    >>> dir(untangle.parse("<com:company.web-app></com:company.web-app>"))
    ['com_company_web_app']
    ```

    Children’s tag names aren’t the only object properties you can access. Elements have a few predefined object attributes that might be shown by calling `vars()`:

    ```python
    >>> element = document.svg.text

    >>> list(vars(element).keys())
    ['_name', '_attributes', 'children', 'is_root', 'cdata']
    
    >>> element._name
    'text'
    
    >>> element._attributes
    {'x': '-40', 'y': '75'}
    
    >>> element.children
    []
    
    >>> element.is_root
    False
    
    >>> element.cdata
    'Hello <svg>!'
    ```

    Behind the scenes, untangle uses the built-in SAX parser, but because the library is implemented in pure Python and creates lots of heavyweight objects, it has considerably **poor performance**. While it’s intended for reading tiny documents, you can still combine it with another approach to read multi-gigabyte XML files.
    
    Here’s how. If you head over to [Wikipedia archives](https://dumps.wikimedia.org/enwiki/latest/), you can download one of their compressed XML files. The one at the top should contain a snapshot of the articles’ abstracts:

    ```xml
    <feed>
      <doc>
        <title>Wikipedia: Anarchism</title>
        <url>https://en.wikipedia.org/wiki/Anarchism</url>
        <abstract>Anarchism is a political philosophy...</abstract>
        <links>
          <sublink linktype="nav">
            <anchor>Etymology, terminology and definition</anchor>
            <link>https://en.wikipedia.org/wiki/Anarchism#Etymology...</link>
          </sublink>
          <sublink linktype="nav">
            <anchor>History</anchor>
            <link>https://en.wikipedia.org/wiki/Anarchism#History</link>
          </sublink>
          ⋮
        </links>
      </doc>
      ⋮
    </feed>
    ```

    It’s over 6 GB in size after download, which is perfect for this exercise. The idea is to scan the file to find the consecutive opening and closing <doc> tags and then parse the XML fragment between them using untangle for convenience.
    
    The built-in [mmap](https://realpython.com/python-mmap/) module lets you create a **virtual view** of the file contents, even when it doesn’t fit the available memory. This gives an impression of working with a huge string of bytes that supports searching and the regular slicing syntax. If you’re interested in how to encapsulate this logic in a [Python class](https://realpython.com/python-classes/) and take advantage of a [generator](https://realpython.com/introduction-to-python-generators/) for lazy evaluation, then expand the collapsible section below.

    <!--这个语法在内容渲染时可以收缩起来-->
    ??? abstract "**A Hybrid Approach to Parsing XML**"

        Here’s the complete code of the XMLTagStream class:
    
        ```python
        import mmap
        import untangle
        
        class XMLTagStream:
            def __init__(self, path, tag_name, encoding="utf-8"):
                self.file = open(path)
                self.stream = mmap.mmap(
                    self.file.fileno(), 0, access=mmap.ACCESS_READ
                )
                self.tag_name = tag_name
                self.encoding = encoding
                self.start_tag = f"<{tag_name}>".encode(encoding)
                self.end_tag = f"</{tag_name}>".encode(encoding)
        
            def __enter__(self):
                return self
        
            def __exit__(self, *args, **kwargs):
                self.stream.close()
                self.file.close()
        
            def __iter__(self):
                end = 0
                while (begin := self.stream.find(self.start_tag, end)) != -1:
                    end = self.stream.find(self.end_tag, begin)
                    yield self.parse(self.stream[begin: end + len(self.end_tag)])
        
            def parse(self, chunk):
                document = untangle.parse(chunk.decode(self.encoding))
                return getattr(document, self.tag_name)
        ```
    
        It’s a custom [context manager](https://realpython.com/python-with-statement/), which uses the **iterator protocol** defined as an inline **generator function**. The resulting generator object loops over the XML document as if it was a long stream of characters.
        
        Note that the while loop takes advantage of fairly new Python syntax, the [walrus operator (:=)](https://realpython.com/python-walrus-operator/), to simplify the code. You can use this operator in **assignment expressions**, where an expression can be evaluated and assigned to a variable.

    Without getting into the nitty-gritty details, here’s how you can use this custom class to go through a big XML file quickly while inspecting specific elements more thoroughly with untangle:

    ```python
    >>> with XMLTagStream("abstract.xml", "doc") as stream:
    ...     for doc in stream:
    ...         print(doc.title.cdata.center(50, "="))
    ...         for sublink in doc.links.sublink:
    ...             print("-", sublink.anchor.cdata)
    ...         if "q" == input("Press [q] to exit or any key to continue..."):
    ...             break
    ...
    ===============Wikipedia: Anarchism===============
    - Etymology, terminology and definition
    - History
    - Pre-modern era
    ⋮
    Press [q] to exit or any key to continue...
    ================Wikipedia: Autism=================
    - Characteristics
    - Social development
    - Communication
    ⋮
    Press [q] to exit or any key to continue...
    ```

    First, you open a file for reading and indicate the tag name that you want to find. Then, you iterate over those elements and receive a parsed fragment of the XML document. It’s almost like looking through a tiny window moving over an infinitely long sheet of paper. That’s a relatively surface-level example that ignores a few details, but it should give you a general idea of how to use such a hybrid parsing strategy.

### xmltodict: 将 XML 转换为 Python 字典

=== "中文"

    如果您喜欢 JSON 但不喜欢 XML，那么请查看 [xmltodict](https://pypi.org/project/xmltodict/)，它试图弥合两种数据格式之间的差距。 顾名思义，该库可以解析 XML 文档并将其表示为 Python 字典，这也恰好是 Python 中 JSON 文档的目标数据类型。 这使得 **XML 和 JSON** 之间的转换成为可能。

    !!! info "Note"

        Note: 字典是由键值对组成的，而XML文档本质上是分层的，这可能会导致转换过程中丢失一些信息。 除此之外，XML 还具有属性、注释、处理指令以及字典中不可用的其他定义元数据的方法。

    与迄今为止的其他 XML 解析器不同，这个解析器需要一个 Python 字符串或一个类似文件的对象，以二进制模式打开以供读取：

    ```python
    >>> import xmltodict

    >>> xmltodict.parse("""\
    ... <svg viewBox="-105 -100 210 270">
    ...   <!-- More content goes here... -->
    ... </svg>
    ... """)
    OrderedDict([('svg', OrderedDict([('@viewBox', '-105 -100 210 270')]))])
    
    >>> with open("smiley.svg", "rb") as file:
    ...     xmltodict.parse(file)
    ...
    OrderedDict([('svg', ...)])
    ```

    默认情况下，该库返回 [OrderedDict](https://realpython.com/python-ordereddict/) 集合的实例以保留**元素顺序**。 然而，从Python 3.6开始，普通字典也保留插入顺序。 如果您想使用常规字典，请将 dict 作为 `dict_constructor` 参数传递给 `parse()` 函数：

    ```python
    >>> import xmltodict

    >>> with open("smiley.svg", "rb") as file:
    ...     xmltodict.parse(file, dict_constructor=dict)
    ...
    {'svg': ...}
    ```

    现在，`parse()` 返回一个普通的旧字典，具有熟悉的文本表示形式。
    
    为了避免 XML 元素及其属性之间的**名称冲突**，库会自动为后者添加 `@` 字符作为前缀。 您还可以通过适当设置 `xml_attribs` 标志来完全忽略属性：

    ```python
    >>> import xmltodict

    >>> # 默认重命名属性
    >>> with open("smiley.svg", "rb") as file:
    ...     document = xmltodict.parse(file)
    ...     print([x for x in document["svg"] if x.startswith("@")])
    ...
    ['@xmlns', '@xmlns:inkscape', '@viewBox', '@width', '@height']
    
    >>> # 当请求时忽略属性
    >>> with open("smiley.svg", "rb") as file:
    ...     document = xmltodict.parse(file, xml_attribs=False)
    ...     print([x for x in document["svg"] if x.startswith("@")])
    ...
    []
    ```

    默认情况下会忽略的另一条信息是 **XML 命名空间** 声明。 这些被视为常规属性，而相应的前缀则成为标签名称的一部分。 但是，如果您愿意，可以扩展、重命名或跳过某些命名空间：

    ```python
    >>> import xmltodict

    >>> # 默认忽略命名空间
    >>> with open("smiley.svg", "rb") as file:
    ...     document = xmltodict.parse(file)
    ...     print(document.keys())
    ...
    odict_keys(['svg'])
    
    >>> # 根据请求处理命名空间
    >>> with open("smiley.svg", "rb") as file:
    ...     document = xmltodict.parse(file, process_namespaces=True)
    ...     print(document.keys())
    ...
    odict_keys(['http://www.w3.org/2000/svg:svg'])
    
    >>> # 重命名并跳过一些命名空间
    >>> namespaces = {
    ...     "http://www.w3.org/2000/svg": "svg",
    ...     "http://www.inkscape.org/namespaces/inkscape": None,
    ... }
    >>> with open("smiley.svg", "rb") as file:
    ...     document = xmltodict.parse(
    ...         file, process_namespaces=True, namespaces=namespaces
    ...     )
    ...     print(document.keys())
    ...     print("custom" in document["svg:svg"])
    ...     print("inkscape:custom" in document["svg:svg"])
    ...
    odict_keys(['svg:svg'])
    True
    False
    ```

    在上面的第一个示例中，标签名称不包含 XML 命名空间前缀。 在第二个示例中，它们这样做是因为您请求处理它们。 最后，在第三个示例中，您将默认命名空间折叠为 svg，同时使用 None 抑制 Inkscape 的命名空间。
    
    Python 字典的默认字符串表示形式可能不够清晰。 要改进其呈现方式，您可以对其进行 [pretty-print](https://realpython.com/python-print/#pretty-printing-nested-data-structs) 或将其转换为其他格式，例如 **JSON** 或 **YAML**：

    ```python
    >>> import xmltodict
    >>> with open("smiley.svg", "rb") as file:
    ...     document = xmltodict.parse(file, dict_constructor=dict)
    ...
    
    >>> from pprint import pprint as pp
    >>> pp(document)
    {'svg': {'@height': '270',
             '@viewBox': '-105 -100 210 270',
             '@width': '210',
             '@xmlns': '<http://www.w3.org/2000/svg>',
             '@xmlns:inkscape': '<http://www.inkscape.org/namespaces/inkscape>',
             'defs': {'linearGradient': {'@id': 'skin',
             ⋮
    
    >>> import json
    >>> print(json.dumps(document, indent=4, sort_keys=True))
    {
        "svg": {
            "@height": "270",
            "@viewBox": "-105 -100 210 270",
            "@width": "210",
            "@xmlns": "<http://www.w3.org/2000/svg>",
            "@xmlns:inkscape": "<http://www.inkscape.org/namespaces/inkscape>",
            "defs": {
                "linearGradient": {
                 ⋮
    
    >>> import yaml  # Install first with 'pip install PyYAML'
    >>> print(yaml.dump(document))
    svg:
      '@height': '270'
      '@viewBox': -105 -100 210 270
      '@width': '210'
      '@xmlns': <http://www.w3.org/2000/svg>
      '@xmlns:inkscape': <http://www.inkscape.org/namespaces/inkscape>
      defs:
        linearGradient:
    ⋮
    ```

    `xmltodict` 库允许以相反的方式转换文档，即从 Python 字典转换回 XML 字符串：

    ```python
    >>> import xmltodict

    >>> with open("smiley.svg", "rb") as file:
    ...     document = xmltodict.parse(file, dict_constructor=dict)
    ...
    
    >>> xmltodict.unparse(document)
    '<?xml version="1.0" encoding="utf-8"?>\n<svg...'
    ```

    当需要将数据从 JSON 或 YAML 转换为 XML 时，字典可以作为中间格式派上用场。
    
    `xmltodict` 库中还有更多功能，例如流式传输，因此请随意自行探索它们。 然而，这个库也有点过时了。 此外，如果您确实正在寻求高级 XML 解析功能，那么它是您应该关注的下一个库。

=== "原文"

    If you like JSON but you’re not a fan of XML, then check out [xmltodict](https://pypi.org/project/xmltodict/), which tries to bridge the gap between both data formats. As the name implies, the library can parse an XML document and represent it as a Python dictionary, which also happens to be the target data type for JSON documents in Python. This makes **conversion between XML and JSON** possible.

    !!! info "Note"

        Note: Dictionaries are made up of key-value pairs, while XML documents are inherently hierarchical, which may lead to some information loss during the conversion. On top of that, XML has attributes, comments, processing instructions, and other ways of defining metadata that aren’t available in dictionaries.

    Unlike the rest of the XML parsers so far, this one expects either a Python string or a file-like object open for reading in binary mode:

    ```python
    >>> import xmltodict

    >>> xmltodict.parse("""\
    ... <svg viewBox="-105 -100 210 270">
    ...   <!-- More content goes here... -->
    ... </svg>
    ... """)
    OrderedDict([('svg', OrderedDict([('@viewBox', '-105 -100 210 270')]))])
    
    >>> with open("smiley.svg", "rb") as file:
    ...     xmltodict.parse(file)
    ...
    OrderedDict([('svg', ...)])
    ```

    By default, the library returns an instance of the [OrderedDict](https://realpython.com/python-ordereddict/) collection to retain **element order**. However, starting from Python 3.6, plain dictionaries also keep the insertion order. If you’d like to work with regular dictionaries instead, then pass dict as the `dict_constructor` argument to the `parse()` function:

    ```python
    >>> import xmltodict

    >>> with open("smiley.svg", "rb") as file:
    ...     xmltodict.parse(file, dict_constructor=dict)
    ...
    {'svg': ...}
    ```

    Now, `parse()` returns a plain old dictionary with a familiar textual representation.
    
    To avoid **name conflicts** between XML elements and their attributes, the library automatically prefixes the latter with an `@` character. You may also ignore attributes completely by setting the `xml_attribs` flag appropriately:

    ```python
    >>> import xmltodict

    >>> # Rename attributes by default
    >>> with open("smiley.svg", "rb") as file:
    ...     document = xmltodict.parse(file)
    ...     print([x for x in document["svg"] if x.startswith("@")])
    ...
    ['@xmlns', '@xmlns:inkscape', '@viewBox', '@width', '@height']
    
    >>> # Ignore attributes when requested
    >>> with open("smiley.svg", "rb") as file:
    ...     document = xmltodict.parse(file, xml_attribs=False)
    ...     print([x for x in document["svg"] if x.startswith("@")])
    ...
    []
    ```

    Yet another piece of information that gets ignored by default is the **XML namespace** declaration. These are treated like regular attributes, while the corresponding prefixes become part of the tag name. However, you can expand, rename, or skip some of the namespaces if you want to:

    ```python
    >>> import xmltodict

    >>> # Ignore namespaces by default
    >>> with open("smiley.svg", "rb") as file:
    ...     document = xmltodict.parse(file)
    ...     print(document.keys())
    ...
    odict_keys(['svg'])
    
    >>> # Process namespaces when requested
    >>> with open("smiley.svg", "rb") as file:
    ...     document = xmltodict.parse(file, process_namespaces=True)
    ...     print(document.keys())
    ...
    odict_keys(['http://www.w3.org/2000/svg:svg'])
    
    >>> # Rename and skip some namespaces
    >>> namespaces = {
    ...     "http://www.w3.org/2000/svg": "svg",
    ...     "http://www.inkscape.org/namespaces/inkscape": None,
    ... }
    >>> with open("smiley.svg", "rb") as file:
    ...     document = xmltodict.parse(
    ...         file, process_namespaces=True, namespaces=namespaces
    ...     )
    ...     print(document.keys())
    ...     print("custom" in document["svg:svg"])
    ...     print("inkscape:custom" in document["svg:svg"])
    ...
    odict_keys(['svg:svg'])
    True
    False
    ```

    In the first example above, tag names don’t include the XML namespace prefix. In the second example, they do because you requested to process them. Finally, in the third example, you collapsed the default namespace to svg while suppressing Inkscape’s namespace with None.
    
    The default string representation of a Python dictionary might not be legible enough. To improve its presentation, you can [pretty-print](https://realpython.com/python-print/#pretty-printing-nested-data-structures) it or convert it to another format such as **JSON** or **YAML**:

    ```python
    >>> import xmltodict
    >>> with open("smiley.svg", "rb") as file:
    ...     document = xmltodict.parse(file, dict_constructor=dict)
    ...
    
    >>> from pprint import pprint as pp
    >>> pp(document)
    {'svg': {'@height': '270',
             '@viewBox': '-105 -100 210 270',
             '@width': '210',
             '@xmlns': '<http://www.w3.org/2000/svg>',
             '@xmlns:inkscape': '<http://www.inkscape.org/namespaces/inkscape>',
             'defs': {'linearGradient': {'@id': 'skin',
             ⋮
    
    >>> import json
    >>> print(json.dumps(document, indent=4, sort_keys=True))
    {
        "svg": {
            "@height": "270",
            "@viewBox": "-105 -100 210 270",
            "@width": "210",
            "@xmlns": "<http://www.w3.org/2000/svg>",
            "@xmlns:inkscape": "<http://www.inkscape.org/namespaces/inkscape>",
            "defs": {
                "linearGradient": {
                 ⋮
    
    >>> import yaml  # Install first with 'pip install PyYAML'
    >>> print(yaml.dump(document))
    svg:
      '@height': '270'
      '@viewBox': -105 -100 210 270
      '@width': '210'
      '@xmlns': <http://www.w3.org/2000/svg>
      '@xmlns:inkscape': <http://www.inkscape.org/namespaces/inkscape>
      defs:
        linearGradient:
    ⋮
    ```

    The `xmltodict` library allows for converting the document the other way around—that is, from a Python dictionary back to an XML string:

    ```python
    >>> import xmltodict

    >>> with open("smiley.svg", "rb") as file:
    ...     document = xmltodict.parse(file, dict_constructor=dict)
    ...
    
    >>> xmltodict.unparse(document)
    '<?xml version="1.0" encoding="utf-8"?>\n<svg...'
    ```

    The dictionary may come in handy as an intermediate format when converting data from JSON or YAML to XML, should there be such a need.
    
    There are a bunch more features in the `xmltodict` library, such as streaming, so feel free to explore them on your own. However, this library is a bit dated too. Besides, it’s the next library that should be on your radar if you’re really seeking advanced XML parsing features.

### lxml: 使用经过扩充的 ElementTree

=== "中文"

    如果您想要将最佳性能、最广泛的功能和最熟悉的界面全部封装在一个包中，那么安装 [lxml](https://pypi.org/project/lxml/) 并忘记其余的解析库。 它是 C 库 [libxml2](https://en.wikipedia.org/wiki/Libxml2) 和 [libxslt](https://en.wikipedia.org/wiki/Libxslt) 的 **Python 绑定**， 它支持多种标准，包括 XPath、XML Schema 和 XSLT。
    
    该库与 Python 的 **ElementTree API** 兼容，您在本教程前面已经了解过它。 这意味着您可以通过仅替换单个导入语句来重用现有代码：

    ```python
    import lxml.etree as ET
    ```

    这将为您带来巨大的**性能提升**。 最重要的是，lxml 库附带了一组广泛的功能，并提供了不同的使用方式。 例如，它允许您针对多种模式语言**验证**您的 XML 文档，其中之一是 XML 模式定义：

    ```python
    >>> import lxml.etree as ET

    >>> xml_schema = ET.XMLSchema(
    ...     ET.fromstring("""\
    ...         <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    ...             <xsd:element name="parent"/>
    ...             <xsd:complexType name="SomeType">
    ...                 <xsd:sequence>
    ...                     <xsd:element name="child" type="xsd:string"/>
    ...                 </xsd:sequence>
    ...             </xsd:complexType>
    ...         </xsd:schema>"""))
    
    >>> valid = ET.fromstring("<parent><child></child></parent>")
    >>> invalid = ET.fromstring("<child><parent></parent></child>")
    
    >>> xml_schema.validate(valid)
    True
    
    >>> xml_schema.validate(invalid)
    False
    ```

    Python 标准库中的 XML 解析器都没有验证文档的能力。 同时，`lxml` 允许您定义 `XMLSchema` 对象并通过它运行文档，同时保持与 ElementTree API 的很大程度上兼容。

    除了 ElementTree API 之外，`lxml` 还支持替代的 `lxml.objectify` 接口，稍后您将在数据绑定部分中介绍该接口。

=== "原文"

    If you want the best performance, the broadest spectrum of functionality, and the most familiar interface all wrapped in one package, then install [lxml](https://pypi.org/project/lxml/) and forget about the rest of the libraries. It’s a **Python binding** for the C libraries [libxml2](https://en.wikipedia.org/wiki/Libxml2) and [libxslt](https://en.wikipedia.org/wiki/Libxslt), which support several standards, including XPath, XML Schema, and XSLT.
    
    The library is compatible with Python’s **ElementTree API**, which you learned about earlier in this tutorial. That means you can reuse your existing code by replacing only a single import statement:

    ```python
    import lxml.etree as ET
    ```

    This will give you a great **performance boost**. On top of that, the lxml library comes with an extensive set of features and provides different ways of using them. For example, it lets you **validate** your XML documents against several schema languages, one of which is the XML Schema Definition:

    ```python
    >>> import lxml.etree as ET

    >>> xml_schema = ET.XMLSchema(
    ...     ET.fromstring("""\
    ...         <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    ...             <xsd:element name="parent"/>
    ...             <xsd:complexType name="SomeType">
    ...                 <xsd:sequence>
    ...                     <xsd:element name="child" type="xsd:string"/>
    ...                 </xsd:sequence>
    ...             </xsd:complexType>
    ...         </xsd:schema>"""))
    
    >>> valid = ET.fromstring("<parent><child></child></parent>")
    >>> invalid = ET.fromstring("<child><parent></parent></child>")
    
    >>> xml_schema.validate(valid)
    True
    
    >>> xml_schema.validate(invalid)
    False
    ```

    None of the XML parsers in Python’s standard library have the capability to validate documents. Meanwhile, lxml lets you define an XMLSchema object and run documents through it while remaining largely compatible with the ElementTree API.
    
    Besides the ElementTree API, lxml supports an alternative lxml.objectify interface, which you’ll cover later in the data binding section.

### BeautifulSoup: 处理错误格式的 XML

=== "中文"

    您通常不会使用此比较中的最后一个库来解析 XML，因为您主要遇到的是 [网页抓取](https://realpython.com/beautiful-soup-web-scraper-python/) HTML 文档。 也就是说，它也能够解析 XML。 [BeautifulSoup](https://pypi.org/project/beautifulsoup4/) 附带一个 **可插拔架构**，可让您选择底层解析器。 前面介绍的 lxml 实际上是官方文档推荐的，也是目前该库支持的唯一 XML 解析器。
    
    根据您想要解析的文档类型、所需的效率和功能可用性，您可以选择以下解析器之一：

    | 文档名称 | 解析器 | Python 包 | 速度 |
    | -------| ------| ---------| -----|
    | HTML |"html.parser" | - | Moderate |
    | HTML | "html5lib" | html5lib | Slow |
    | HTML | "lxml" | lxml | Fast |
    | XML | "lxml-xml" or "xml" | lxml | Fast |

    除了速度之外，各个解析器之间还存在明显的差异。 例如，当涉及到格式错误的元素时，其中一些比其他更宽容，而另一些则更好地模拟网络浏览器。

    !!! info "有趣的事实"

        该库的名称指的是[tag soup](https://en.wikipedia.org/wiki/Tag_soup)，它描述了语法或结构上不正确的 HTML 代码。

    假设您已经将 `lxml` 和 `beautifulsoup4` 库安装到活动的[虚拟环境](https://realpython.com/python-virtual-environments-a-primer/)中，您可以立即开始解析 XML 文档。 你只需要导入 `BeautifulSoup`：

    ```python
    from bs4 import BeautifulSoup

    # Parse XML from a file object
    with open("smiley.svg") as file:
        soup = BeautifulSoup(file, features="lxml-xml")
    
    # Parse XML from a Python string
    soup = BeautifulSoup("""\
    <svg viewBox="-105 -100 210 270">
      <!-- More content goes here... -->
    </svg>
    """, features="lxml-xml")
    ```

    如果您不小心指定了不同的解析器（例如 `lxml`），那么该库会为您将缺少的 HTML 标记（例如 `<body>`）添加到解析的文档中。 在这种情况下，这可能不是您想要的，因此在指定解析器名称时要小心。

    BeautifulSoup 是一个用于解析 XML 文档的强大工具，因为它可以**处理无效内容**，并且它具有用于提取信息的**丰富的 API**。 看看它如何处理错误嵌套的标签、禁止的字符和错误放置的文本：

    ```python
    >>> from bs4 import BeautifulSoup

    >>> soup = BeautifulSoup("""\
    ... <parent>
    ...     <child>Forbidden < character </parent>
    ...     </child>
    ... ignored
    ... """, features="lxml-xml")
    
    >>> print(soup.prettify())
    <?xml version="1.0" encoding="utf-8"?>
    <parent>
     <child>
      Forbidden
     </child>
    </parent>
    ```

    不同的解析器会引发异常，并在检测到文档存在问题时立即放弃。 在这里，它不仅忽略了这些问题，而且还找到了解决其中一些问题的明智方法。 元素现在已正确嵌套并且没有无效内容。

    使用 BeautifulSoup 定位元素的方法有太多，这里无法一一涵盖。 通常，您会在 soup 元素上调用 `.find()` 或 `.findall()` 的变体：

    ```python
    >>> from bs4 import BeautifulSoup

    >>> with open("smiley.svg") as file:
    ...     soup = BeautifulSoup(file, features="lxml-xml")
    ...
    
    >>> soup.find_all("ellipse", limit=1)
    [<ellipse cx="-20" cy="-10" fill="black" rx="6" ry="8" stroke="none"/>]
    
    >>> soup.find(x=42)
    <inkscape:custom inkscape:z="555" x="42">Some value</inkscape:custom>
    
    >>> soup.find("stop", {"stop-color": "gold"})
    <stop offset="75%" stop-color="gold" stop-opacity="1.0"/>
    
    >>> soup.find(text=lambda x: "value" in x).parent
    <inkscape:custom inkscape:z="555" x="42">Some value</inkscape:custom>
    ```

    `limit` 参数类似于 MySQL 中的 LIMIT 子句，它可以让您决定最多希望接收多少个结果。 它将返回指定数量的结果或更少。 这并非巧合。 您可以将这些搜索方法视为具有强大过滤器的简单查询语言。

    搜索界面非常灵活，但超出了本教程的范围。 您可以查看[库的文档](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)了解更多详细信息，或阅读另一个关于Python中的涉及基于BeautifulSoup进行[网页抓取](https://realpython.com/python-web-scraping-practical-introduction/)的教程  

=== "原文"

    You won’t typically use the last library in this comparison for parsing XML since you mostly encounter it [web scraping](https://realpython.com/beautiful-soup-web-scraper-python/) HTML documents. That said, it’s capable of parsing XML just as well. [BeautifulSoup](https://pypi.org/project/beautifulsoup4/) comes with a **pluggable architecture** that lets you choose the underlying parser. The lxml one described earlier is actually recommended by the official documentation and is currently the only XML parser supported by the library.
    
    Depending on the kind of documents you’ll want to parse, the desired efficiency, and feature availability, you can select one of these parsers:

    | Document Type | Parser Name | Python  Library | Speed |
    | ----------| -------------| -------------| --------------|
    | HTML |"html.parser" | - | Moderate |
    | HTML | "html5lib" | html5lib | Slow |
    | HTML | "lxml" | lxml | Fast |
    | XML | "lxml-xml" or "xml" | lxml | Fast |

    Other than speed, there are noticeable differences between the individual parsers. For example, some of them are more forgiving than others when it comes to malformed elements, while others emulate web browsers better.

    !!! info "Fun Fact"

        The library’s name refers to the [tag soup](https://en.wikipedia.org/wiki/Tag_soup), which describes syntactically or structurally incorrect HTML code.

    Assuming you’ve already installed the lxml and beautifulsoup4 libraries into your active [virtual environment](https://realpython.com/python-virtual-environments-a-primer/), you can start parsing XML documents right away. You only need to import BeautifulSoup:

    ```python
    from bs4 import BeautifulSoup

    # Parse XML from a file object
    with open("smiley.svg") as file:
        soup = BeautifulSoup(file, features="lxml-xml")
    
    # Parse XML from a Python string
    soup = BeautifulSoup("""\
    <svg viewBox="-105 -100 210 270">
      <!-- More content goes here... -->
    </svg>
    """, features="lxml-xml")
    ```

    If you accidentally specified a different parser, say lxml, then the library would add missing HTML tags such as <body> to the parsed document for you. That probably isn’t what you intended in this case, so be careful when specifying the parser name.

    BeautifulSoup is a powerful tool for parsing XML documents because it can **handle invalid content** and it has a **rich API** for extracting information. Have a look at how it copes with incorrectly nested tags, forbidden characters, and badly placed text:

    ```python
    >>> from bs4 import BeautifulSoup

    >>> soup = BeautifulSoup("""\
    ... <parent>
    ...     <child>Forbidden < character </parent>
    ...     </child>
    ... ignored
    ... """, features="lxml-xml")
    
    >>> print(soup.prettify())
    <?xml version="1.0" encoding="utf-8"?>
    <parent>
     <child>
      Forbidden
     </child>
    </parent>
    ```

    A different parser would raise an [exception](https://realpython.com/python-exceptions/) and surrender as soon as it detected something wrong with the document. Here, not only did it ignore the problems, but it also figured out sensible ways to fix some of them. The elements are properly nested now and have no invalid content.

    There are way too many methods of locating elements with BeautifulSoup to cover them all here. Usually, you’ll call a variant of `.find()` or `.findall()` on the soup element:

    ```python
    >>> from bs4 import BeautifulSoup

    >>> with open("smiley.svg") as file:
    ...     soup = BeautifulSoup(file, features="lxml-xml")
    ...
    
    >>> soup.find_all("ellipse", limit=1)
    [<ellipse cx="-20" cy="-10" fill="black" rx="6" ry="8" stroke="none"/>]
    
    >>> soup.find(x=42)
    <inkscape:custom inkscape:z="555" x="42">Some value</inkscape:custom>
    
    >>> soup.find("stop", {"stop-color": "gold"})
    <stop offset="75%" stop-color="gold" stop-opacity="1.0"/>
    
    >>> soup.find(text=lambda x: "value" in x).parent
    <inkscape:custom inkscape:z="555" x="42">Some value</inkscape:custom>
    ```

    The `limit` parameter is similar to the LIMIT clause in MySQL, which lets you decide how many results you want to receive at most. It will return the specified number of results or fewer. That’s no coincidence. You can think of these search methods as being a simple query language with powerful filters.

    The search interface is very flexible but is outside the scope of this tutorial. You can check the [library’s documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) for more details or read yet another tutorial about [web scraping](https://realpython.com/python-web-scraping-practical-introduction/) in Python that touches on BeautifulSoup.

## 将 XML 数据绑定到 Python 对象

=== "中文"

    假设您想通过低延迟 [WebSocket](https://en.wikipedia.org/wiki/WebSocket) 连接使用实时数据源，并以 XML 格式交换消息。 出于本演示的目的，您将使用 Web 浏览器将鼠标和键盘事件广播到 Python 服务器。 您将构建一个**自定义协议**并使用**数据绑定**将 XML 转换为本机 Python 对象。

    数据绑定背后的想法是以声明方式定义数据模型，同时让程序弄清楚如何在运行时从 XML 中提取有价值的信息。 如果您曾经使用过 [Django 模型](https://docs.djangoproject.com/en/3.2/topics/db/models/)，那么这个概念听起来应该很熟悉。
    
    首先，从设计数据模型开始。 它将包含两种类型的事件：

    1. 键盘事件
    2. 鼠标事件
    
    每个都可以代表一些专门的子类型，例如键盘的按键或按键释放以及鼠标的单击或右键单击。 以下是响应按住“Shift+2”组合键而生成的示例 XML 消息：

    ```xml
    <KeyboardEvent>
        <Type>keydown</Type>
        <Timestamp>253459.17999999982</Timestamp>
        <Key>
            <Code>Digit2</Code>
            <Unicode>@</Unicode>
        </Key>
        <Modifiers>
            <Alt>false</Alt>
            <Ctrl>false</Ctrl>
            <Shift>true</Shift>
            <Meta>false</Meta>
        </Modifiers>
    </KeyboardEvent>
    ```

    该消息包含特定的键盘事件类型、时间戳、键码及其 [Unicode](https://realpython.com/python-encodings-guide/)，以及修饰键，例如“Alt”、“ Ctrl` 或 `Shift`。 [元键](https://en.wikipedia.org/wiki/Meta_key)通常是“Win”或“Cmd”键，具体取决于您的键盘布局。

    同样，鼠标事件可能如下所示：

    ```xml
    <MouseEvent>
        <Type>mousemove</Type>
        <Timestamp>52489.07000000145</Timestamp>
        <Cursor>
            <Delta x="-4" y="8"/>
            <Window x="171" y="480"/>
            <Screen x="586" y="690"/>
        </Cursor>
        <Buttons bitField="0"/>
        <Modifiers>
            <Alt>false</Alt>
            <Ctrl>true</Ctrl>
            <Shift>false</Shift>
            <Meta>false</Meta>
        </Modifiers>
    </MouseEvent>
    ```

    然而，不是按键，而是鼠标光标位置和编码[鼠标按钮](https://developer.mozilla.org/)的[位字段](https://en.wikipedia.org/wiki/Bit_field/en-US/docs/Web/API/MouseEvent/buttons#return_value) 在事件期间按下。 位字段为零表示没有按下任何按钮。

    一旦客户端建立连接，它将开始向服务器发送大量消息。 该协议不包含任何握手、心跳、正常关闭、主题订阅或控制消息。 您可以通过注册事件处理程序并用不到 50 行代码创建 WebSocket 对象，在 JavaScript 中进行编码。
    
    然而，实现客户端并不是本次练习的重点。 由于您不需要理解它，因此只需展开下面的可折叠部分即可显示带有嵌入式 JavaScript 的 HTML 代码，并将其保存在您喜欢的文件中。

    ??? abstract "JavaScript 和 HTML 中的 WebSocket 客户端"

        ```html
        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="utf-8">
            <title>Real-Time Data Feed</title>
        </head>
        <body>
            <script>
                const ws = new WebSocket("ws://localhost:8000")
                ws.onopen = event => {
                    ["keydown", "keyup"].forEach(name =>
                        window.addEventListener(name, event =>
                            ws.send(`\
        <KeyboardEvent>
            <Type>${event.type}</Type>
            <Timestamp>${event.timeStamp}</Timestamp>
            <Key>
                <Code>${event.code}</Code>
                <Unicode>${event.key}</Unicode>
            </Key>
            <Modifiers>
                <Alt>${event.altKey}</Alt>
                <Ctrl>${event.ctrlKey}</Ctrl>
                <Shift>${event.shiftKey}</Shift>
                <Meta>${event.metaKey}</Meta>
            </Modifiers>
        </KeyboardEvent>`))
                    );
                    ["mousedown", "mouseup", "mousemove"].forEach(name =>
                        window.addEventListener(name, event =>
                            ws.send(`\
        <MouseEvent>
            <Type>${event.type}</Type>
            <Timestamp>${event.timeStamp}</Timestamp>
            <Cursor>
                <Delta x="${event.movementX}" y="${event.movementY}"/>
                <Window x="${event.clientX}" y="${event.clientY}"/>
                <Screen x="${event.screenX}" y="${event.screenY}"/>
            </Cursor>
            <Buttons bitField="${event.buttons}"/>
            <Modifiers>
                <Alt>${event.altKey}</Alt>
                <Ctrl>${event.ctrlKey}</Ctrl>
                <Shift>${event.shiftKey}</Shift>
                <Meta>${event.metaKey}</Meta>
            </Modifiers>
        </MouseEvent>`))
                    )
                }
            </script>
        </body>
        </html>
        ```

    客户端连接到侦听端口 8000 的本地服务器。将 HTML 代码保存到文件中后，您就可以使用您喜欢的 Web 浏览器打开它。 但在此之前，您需要实现服务器。
    
    Python 不支持 WebSocket，但您可以将 [websockets](https://pypi.org/project/websockets/) 库安装到活动虚拟环境中。 稍后您还需要 lxml，因此现在是一次性安装这两个依赖项的好时机：

    ```shell  
    python -m pip install websockets lxml
    ```

    最后，您可以构建一个最小的异步 Web 服务器：

    ```python
    # server.py

    import asyncio
    import websockets
    
    async def handle_connection(websocket, path):
        async for message in websocket:
            print(message)
    
    if __name__ == "__main__":
        future = websockets.serve(handle_connection, "localhost", 8000)
        asyncio.get_event_loop().run_until_complete(future)
        asyncio.get_event_loop().run_forever()
    ```

    当您启动服务器并在 Web 浏览器中打开保存的 HTML 文件时，您应该会看到标准输出中出现 XML 消息，以响应您的鼠标移动和按键操作。 您可以在多个选项卡甚至多个浏览器中同时打开客户端！

=== "原文"

    Say you want to consume a real-time data feed over a low-latency [WebSocket](https://en.wikipedia.org/wiki/WebSocket) connection with messages exchanged in XML format. For the purposes of this presentation, you’re going to use a web browser to broadcast your mouse and keyboard events to the Python server. You’ll build a **custom protocol** and use **data binding** to translate XML into native Python objects.

    The idea behind data binding is to define a data model declaratively while letting the program figure out how to extract a valuable piece of information from the XML at runtime. If you’ve ever worked with [Django models](https://docs.djangoproject.com/en/3.2/topics/db/models/), then this concept should sound familiar.
    
    First, begin by designing your data model. It’s going to consist of two types of events:

    1. KeyboardEvent
    2. MouseEvent
    
    Each can represent a few specialized subtypes, like a keypress or key release for the keyboard and a click or right-click for the mouse. Here’s a sample XML message produced in response to holding down the `Shift+2` key combination:

    ```xml
    <KeyboardEvent>
        <Type>keydown</Type>
        <Timestamp>253459.17999999982</Timestamp>
        <Key>
            <Code>Digit2</Code>
            <Unicode>@</Unicode>
        </Key>
        <Modifiers>
            <Alt>false</Alt>
            <Ctrl>false</Ctrl>
            <Shift>true</Shift>
            <Meta>false</Meta>
        </Modifiers>
    </KeyboardEvent>
    ```

    This message contains a specific keyboard event type, a timestamp, the key code and its [Unicode](https://realpython.com/python-encodings-guide/), as well as the modifier keys such as `Alt`, `Ctrl`, or `Shift`. The [meta key](https://en.wikipedia.org/wiki/Meta_key) is usually the `Win` or `Cmd` key, depending on your keyboard layout.

    Similarly, a mouse event could look like this:

    ```xml
    <MouseEvent>
        <Type>mousemove</Type>
        <Timestamp>52489.07000000145</Timestamp>
        <Cursor>
            <Delta x="-4" y="8"/>
            <Window x="171" y="480"/>
            <Screen x="586" y="690"/>
        </Cursor>
        <Buttons bitField="0"/>
        <Modifiers>
            <Alt>false</Alt>
            <Ctrl>true</Ctrl>
            <Shift>false</Shift>
            <Meta>false</Meta>
        </Modifiers>
    </MouseEvent>
    ```

    Instead of the key, however, there’s the mouse cursor position and a [bit field](https://en.wikipedia.org/wiki/Bit_field) encoding the [mouse buttons](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/buttons#return_value) pressed during the event. A bit field of zero indicates that no button was pressed.

    As soon as a client makes the connection, it will start flooding the server with messages. The protocol won’t consist of any handshakes, heartbeats, graceful shutdowns, topic subscriptions, or control messages. You can code this in JavaScript by registering event handlers and creating a WebSocket object in less than fifty lines of code.
    
    However, implementing the client isn’t the point of this exercise. Since you don’t need to understand it, just expand the collapsible section below to reveal the HTML code with embedded JavaScript and save it in a file named whatever you like.

    ??? abstract "A WebSocket Client in JavaScript and HTML"

        ```html
        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="utf-8">
            <title>Real-Time Data Feed</title>
        </head>
        <body>
            <script>
                const ws = new WebSocket("ws://localhost:8000")
                ws.onopen = event => {
                    ["keydown", "keyup"].forEach(name =>
                        window.addEventListener(name, event =>
                            ws.send(`\
        <KeyboardEvent>
            <Type>${event.type}</Type>
            <Timestamp>${event.timeStamp}</Timestamp>
            <Key>
                <Code>${event.code}</Code>
                <Unicode>${event.key}</Unicode>
            </Key>
            <Modifiers>
                <Alt>${event.altKey}</Alt>
                <Ctrl>${event.ctrlKey}</Ctrl>
                <Shift>${event.shiftKey}</Shift>
                <Meta>${event.metaKey}</Meta>
            </Modifiers>
        </KeyboardEvent>`))
                    );
                    ["mousedown", "mouseup", "mousemove"].forEach(name =>
                        window.addEventListener(name, event =>
                            ws.send(`\
        <MouseEvent>
            <Type>${event.type}</Type>
            <Timestamp>${event.timeStamp}</Timestamp>
            <Cursor>
                <Delta x="${event.movementX}" y="${event.movementY}"/>
                <Window x="${event.clientX}" y="${event.clientY}"/>
                <Screen x="${event.screenX}" y="${event.screenY}"/>
            </Cursor>
            <Buttons bitField="${event.buttons}"/>
            <Modifiers>
                <Alt>${event.altKey}</Alt>
                <Ctrl>${event.ctrlKey}</Ctrl>
                <Shift>${event.shiftKey}</Shift>
                <Meta>${event.metaKey}</Meta>
            </Modifiers>
        </MouseEvent>`))
                    )
                }
            </script>
        </body>
        </html>
        ```

    The client connects to a local server listening on port 8000. Once you save the HTML code in a file, you’ll be able to open it with your favorite web browser. But before that, you’ll need to implement the server.
    
    Python doesn’t come with WebSocket support, but you can install the [websockets](https://pypi.org/project/websockets/) library into your active virtual environment. You’re also going to need lxml later, so it’s a good moment to install both dependencies in one go:

    ```shell  
    python -m pip install websockets lxml
    ```

    Finally, you can scaffold a minimal asynchronous web server:

    ```python
    # server.py

    import asyncio
    import websockets
    
    async def handle_connection(websocket, path):
        async for message in websocket:
            print(message)
    
    if __name__ == "__main__":
        future = websockets.serve(handle_connection, "localhost", 8000)
        asyncio.get_event_loop().run_until_complete(future)
        asyncio.get_event_loop().run_forever()
    ```

    When you start the server and open the saved HTML file in a web browser, you should see XML messages appear in the standard output in response to your mouse moves and key presses. You can open the client in multiple tabs or even multiple browsers simultaneously!

### 使用 XPath 表达式定义模型

=== "中文"

    现在，您的消息以纯字符串格式到达。 处理这种格式的消息不太方便。 幸运的是，您可以使用 `lxml.objectify` 模块通过一行代码将它们转换为复合 Python 对象：

    ```python
    # server.py

    import asyncio
    import websockets
    import lxml.objectify
    
    async def handle_connection(websocket, path):
        async for message in websocket:
            try:
                xml = lxml.objectify.fromstring(message)
            except SyntaxError:
                print("Malformed XML message:", repr(message))
            else:
                if xml.tag == "KeyboardEvent":
                    if xml.Type == "keyup":
                        print("Key:", xml.Key.Unicode)
                elif xml.tag == "MouseEvent":
                    screen = xml.Cursor.Screen
                    print("Mouse:", screen.get("x"), screen.get("y"))
                else:
                    print("Unrecognized event type")
    
    # ...
    ```

    只要XML解析成功，您就可以检查根元素的常用属性，例如标签名称、属性、内部文本等。 您将能够使用点运算符深入浏览元素树。 在大多数情况下，该库将识别合适的 Python 数据类型并为您转换该值。

    保存这些更改并重新启动服务器后，您需要在 Web 浏览器中重新加载页面以建立新的 WebSocket 连接。 以下是修改后的程序的示例输出：

    ```shell 
    $ python server.py
    Mouse: 820 121
    Mouse: 820 122
    Mouse: 820 123
    Mouse: 820 124
    Mouse: 820 125
    Key: a
    Mouse: 820 125
    Mouse: 820 125
    Key: a
    Key: A
    Key: Shift
    Mouse: 821 125
    Mouse: 821 125
    Mouse: 820 123
    ⋮
    ```

    有时，XML 可能包含不是有效 Python 标识符的标记名称，或者您可能希望调整消息结构以适应您的数据模型。 在这种情况下，一个有趣的选择是使用[描述符](https://realpython.com/python-descriptors/)定义自定义模型类，声明如何使用 XPath 表达式查找信息。 这部分开始类似于 Django 模型或 [Pydantic](https://github.com/samuelcolvin/pydantic) 模式定义。

    您将使用自定义 XPath 描述符和随附的 `Model` 类，它们为您的数据模型提供可重用的属性。 描述符需要一个 `XPath` 表达式来在接收到的消息中进行元素查找。 底层实现有点高级，因此请随意从下面的可折叠部分复制代码。

    ??? abstract "XPath 描述符和模型类"

        ```python
        import lxml.objectify

        class XPath:
            def __init__(self, expression, /, default=None, multiple=False):
                self.expression = expression
                self.default = default
                self.multiple = multiple
        
            def __set_name__(self, owner, name):
                self.attribute_name = name
                self.annotation = owner.__annotations__.get(name)
        
            def __get__(self, instance, owner):
                value = self.extract(instance.xml)
                instance.__dict__[self.attribute_name] = value
                return value
        
            def extract(self, xml):
                elements = xml.xpath(self.expression)
                if elements:
                    if self.multiple:
                        if self.annotation:
                            return [self.annotation(x) for x in elements]
                        else:
                            return elements
                    else:
                        first = elements[0]
                        if self.annotation:
                            return self.annotation(first)
                        else:
                            return first
                else:
                    return self.default
        
        class Model:
            """Abstract base class for your models."""
            def __init__(self, data):
                if isinstance(data, str):
                    self.xml = lxml.objectify.fromstring(data)
                elif isinstance(data, lxml.objectify.ObjectifiedElement):
                    self.xml = data
                else:
                    raise TypeError("Unsupported data type:", type(data))
        ```
    
    假设您的模块中已经有了所需的 `XPath` 描述符和 `Model` 抽象基类，您可以使用它们来定义 `KeyboardEvent` 和 `MouseEvent` 消息类型以及可重用的构建块以避免重复。 有无数种方法可以做到这一点，但这里有一个例子：

    ```python
    # ...
    
    class Event(Model):
        """具有公共元素的事件消息的基类。"""
        type_: str = XPath("./Type")
        timestamp: float = XPath("./Timestamp")
    
    class Modifiers(Model):
        alt: bool = XPath("./Alt")
        ctrl: bool = XPath("./Ctrl")
        shift: bool = XPath("./Shift")
        meta: bool = XPath("./Meta")
    
    class KeyboardEvent(Event):
        key: str = XPath("./Key/Code")
        modifiers: Modifiers = XPath("./Modifiers")
    
    class MouseEvent(Event):
        x: int = XPath("./Cursor/Screen/@x")
        y: int = XPath("./Cursor/Screen/@y")
        modifiers: Modifiers = XPath("./Modifiers")
    ```

    XPath 描述符允许**惰性求值**，以便仅在请求时才查找 XML 消息的元素。 更具体地说，只有当您访问事件对象的属性时才会查找它们。 此外，结果会被**缓存**以避免多次运行相同的 XPath 查询。 该描述符还尊重[类型注释](https://realpython.com/python-type-checking/)并自动将反序列化数据转换为正确的Python类型。

    使用这些事件对象与之前`lxml.objectify`自动生成的事件对象没有太大区别：

    ```python
    if xml.tag == "KeyboardEvent":
        event = KeyboardEvent(xml)
        if event.type_ == "keyup":
            print("Key:", event.key)
    elif xml.tag == "MouseEvent":
        event = MouseEvent(xml)
        print("Mouse:", event.x, event.y)
    else:
        print("Unrecognized event type")
    ```

    还有一个额外的步骤是创建特定事件类型的新对象。 但除此之外，它还为您提供了独立于 XML 协议构建模型的更大灵活性。 此外，还可以根据收到的消息中的属性派生新的模型属性，并在此基础上添加更多方法。

=== "原文"

    Right now, your messages arrive in plain string format. It’s not very convenient to work with the messages in this format. Fortunately, you can turn them into compound Python objects with a single line of code using the `lxml.objectify` module:

    ```python
    # server.py

    import asyncio
    import websockets
    import lxml.objectify
    
    async def handle_connection(websocket, path):
        async for message in websocket:
            try:
                xml = lxml.objectify.fromstring(message)
            except SyntaxError:
                print("Malformed XML message:", repr(message))
            else:
                if xml.tag == "KeyboardEvent":
                    if xml.Type == "keyup":
                        print("Key:", xml.Key.Unicode)
                elif xml.tag == "MouseEvent":
                    screen = xml.Cursor.Screen
                    print("Mouse:", screen.get("x"), screen.get("y"))
                else:
                    print("Unrecognized event type")
    
    # ...
    ```

    As long as the XML parsing is successful, you can inspect the root element’s usual properties, such as the tag name, attributes, inner text, and so on. You’ll be able to use the dot operator to navigate deep into the element tree. In most cases, the library will recognize a suitable Python data type and convert the value for you.

    After saving those changes and restarting the server, you’ll need to reload the page in your web browser to make a new WebSocket connection. Here’s a sample output of the modified program:

    ```shell 
    $ python server.py
    Mouse: 820 121
    Mouse: 820 122
    Mouse: 820 123
    Mouse: 820 124
    Mouse: 820 125
    Key: a
    Mouse: 820 125
    Mouse: 820 125
    Key: a
    Key: A
    Key: Shift
    Mouse: 821 125
    Mouse: 821 125
    Mouse: 820 123
    ⋮
    ```

    Sometimes, XML may contain tag names that aren’t valid Python identifiers, or you might want to adapt the message structure to fit your data model. In such a case, an interesting option would be defining custom **model classes** with [descriptors](https://realpython.com/python-descriptors/) that declare how to look up information using XPath expressions. That’s the part that starts to resemble Django models or [Pydantic](https://github.com/samuelcolvin/pydantic) schema definitions.

    You’re going to use a custom XPath descriptor and an accompanying Model class, which provide reusable properties for your data models. The descriptor expects an XPath expression for element lookup in the received message. The underlying implementation is a bit advanced, so feel free to copy the code from the collapsible section below.

    ??? abstract "XPath Descriptor and the Model Class"

        ```python
        import lxml.objectify

        class XPath:
            def __init__(self, expression, /, default=None, multiple=False):
                self.expression = expression
                self.default = default
                self.multiple = multiple
        
            def __set_name__(self, owner, name):
                self.attribute_name = name
                self.annotation = owner.__annotations__.get(name)
        
            def __get__(self, instance, owner):
                value = self.extract(instance.xml)
                instance.__dict__[self.attribute_name] = value
                return value
        
            def extract(self, xml):
                elements = xml.xpath(self.expression)
                if elements:
                    if self.multiple:
                        if self.annotation:
                            return [self.annotation(x) for x in elements]
                        else:
                            return elements
                    else:
                        first = elements[0]
                        if self.annotation:
                            return self.annotation(first)
                        else:
                            return first
                else:
                    return self.default
        
        class Model:
            """Abstract base class for your models."""
            def __init__(self, data):
                if isinstance(data, str):
                    self.xml = lxml.objectify.fromstring(data)
                elif isinstance(data, lxml.objectify.ObjectifiedElement):
                    self.xml = data
                else:
                    raise TypeError("Unsupported data type:", type(data))
        ```
    
    Assuming you already have the desired XPath descriptor and the Model abstract base class in your module, you might use them to define `KeyboardEvent` and `MouseEvent` message types along with reusable building blocks to avoid repetition. There are infinite ways to do so, but here’s one example:

    ```python
    # ...
    
    class Event(Model):
        """Base class for event messages with common elements."""
        type_: str = XPath("./Type")
        timestamp: float = XPath("./Timestamp")
    
    class Modifiers(Model):
        alt: bool = XPath("./Alt")
        ctrl: bool = XPath("./Ctrl")
        shift: bool = XPath("./Shift")
        meta: bool = XPath("./Meta")
    
    class KeyboardEvent(Event):
        key: str = XPath("./Key/Code")
        modifiers: Modifiers = XPath("./Modifiers")
    
    class MouseEvent(Event):
        x: int = XPath("./Cursor/Screen/@x")
        y: int = XPath("./Cursor/Screen/@y")
        modifiers: Modifiers = XPath("./Modifiers")
    ```

    The XPath descriptor allows for **lazy evaluation** so that elements of the XML messages are looked up only when requested. More specifically, they’re only looked up when you access a property on the event object. Moreover, the results are **cached** to avoid running the same XPath query more than once. The descriptor also respects [type annotations](https://realpython.com/python-type-checking/) and converts deserialized data to the right Python type automatically.

    Using those event objects isn’t much different from the ones auto-generated by `lxml.objectify` before:

    ```python
    if xml.tag == "KeyboardEvent":
        event = KeyboardEvent(xml)
        if event.type_ == "keyup":
            print("Key:", event.key)
    elif xml.tag == "MouseEvent":
        event = MouseEvent(xml)
        print("Mouse:", event.x, event.y)
    else:
        print("Unrecognized event type")
    ```

    There’s an additional step of creating new objects of the specific event type. But other than that, it gives you more flexibility in terms of structuring your model independently of the XML protocol. Additionally, it’s possible to derive new model attributes based on the ones in the received messages and add more methods on top of that.

### 从 XML 模式生成模型

=== "中文"

    实现模型类是一项乏味且容易出错的任务。 但是，只要您的模型反映了 XML 消息，您就可以利用自动化工具基于 XML 架构为您生成必要的代码。 此类代码的缺点是它的可读性通常不如手工编写的代码。
    
    最古老的第三方模块之一是 [PyXB](https://pypi.org/project/PyXB/)，它模仿 Java 流行的 [JAXB](https://www.baeldung.com/jaxb)库。 不幸的是，它的最后一次发布是在几年前，并且针对的是旧版 Python 版本。 您可以研究类似但积极维护的 [generateDS](https://pypi.org/project/generateDS/) 替代方案，它从 XML 模式生成数据结构。
    
    假设您有这个 models.xsd 架构文件来描述您的 `KeyboardEvent` 消息：

    ```XML
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">
        <xsd:element name="KeyboardEvent" type="KeyboardEventType"/>
        <xsd:complexType name="KeyboardEventType">
            <xsd:sequence>
                <xsd:element type="xsd:string" name="Type"/>
                <xsd:element type="xsd:float" name="Timestamp"/>
                <xsd:element type="KeyType" name="Key"/>
                <xsd:element type="ModifiersType" name="Modifiers"/>
            </xsd:sequence>
        </xsd:complexType>
        <xsd:complexType name="KeyType">
            <xsd:sequence>
                <xsd:element type="xsd:string" name="Code"/>
                <xsd:element type="xsd:string" name="Unicode"/>
            </xsd:sequence>
        </xsd:complexType>
        <xsd:complexType name="ModifiersType">
            <xsd:sequence>
                <xsd:element type="xsd:string" name="Alt"/>
                <xsd:element type="xsd:string" name="Ctrl"/>
                <xsd:element type="xsd:string" name="Shift"/>
                <xsd:element type="xsd:string" name="Meta"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:schema>
    ```

    模式告诉 XML 解析器需要哪些元素、它们的顺序以及它们在树中的级别。 它还限制 XML 属性的允许值。 这些声明与实际 XML 文档之间的任何差异都会使其无效并导致解析器拒绝该文档。
    
    此外，某些工具可以利用此信息生成一段代码，向您隐藏 XML 解析的详细信息。 安装库后，您应该能够在活动虚拟环境中运行 `generateDS` 命令：

    ```shell  
    $ generateDS -o models.py models.xsd
    ```

    它将在与生成的 Python 源代码相同的目录中创建一个名为 `models.py` 的新文件。 然后，您可以导入该模块并使用它来解析传入的消息：
    
    <!--python控制台语法-->
    ```pycon
    >>> from models import parseString

    >>> event = parseString("""\
    ... <KeyboardEvent>
    ...     <Type>keydown</Type>
    ...     <Timestamp>253459.17999999982</Timestamp>
    ...     <Key>
    ...         <Code>Digit2</Code>
    ...         <Unicode>@</Unicode>
    ...     </Key>
    ...     <Modifiers>
    ...         <Alt>false</Alt>
    ...         <Ctrl>false</Ctrl>
    ...         <Shift>true</Shift>
    ...         <Meta>false</Meta>
    ...     </Modifiers>
    ... </KeyboardEvent>""", silence=True)
    
    >>> event.Type, event.Key.Code
    ('keydown', 'Digit2')
    ```

    它看起来与前面显示的`lxml.objectify`示例类似。 不同之处在于，**使用数据绑定强制遵守架构**，而 `lxml.objectify` 会动态生成对象，无论它们在语义上是否正确。

=== "原文"

    Implementing model classes is a tedious and error-prone task. However, as long as your model mirrors the XML messages, you can take advantage of an automated tool to generate the necessary code for you based on XML Schema. The downside of such code is that it’s usually less readable than if written by hand.
    
    One of the oldest third-party modules to allow that was [PyXB](https://pypi.org/project/PyXB/), which mimics Java’s popular [JAXB](https://www.baeldung.com/jaxb) library. Unfortunately, it was last released several years ago and was targeting legacy Python versions. You can look into a similar yet actively maintained [generateDS](https://pypi.org/project/generateDS/) alternative, which generates data structures from XML Schema.
    
    Let’s say you have this models.xsd schema file describing your KeyboardEvent message:

    ```XML
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">
        <xsd:element name="KeyboardEvent" type="KeyboardEventType"/>
        <xsd:complexType name="KeyboardEventType">
            <xsd:sequence>
                <xsd:element type="xsd:string" name="Type"/>
                <xsd:element type="xsd:float" name="Timestamp"/>
                <xsd:element type="KeyType" name="Key"/>
                <xsd:element type="ModifiersType" name="Modifiers"/>
            </xsd:sequence>
        </xsd:complexType>
        <xsd:complexType name="KeyType">
            <xsd:sequence>
                <xsd:element type="xsd:string" name="Code"/>
                <xsd:element type="xsd:string" name="Unicode"/>
            </xsd:sequence>
        </xsd:complexType>
        <xsd:complexType name="ModifiersType">
            <xsd:sequence>
                <xsd:element type="xsd:string" name="Alt"/>
                <xsd:element type="xsd:string" name="Ctrl"/>
                <xsd:element type="xsd:string" name="Shift"/>
                <xsd:element type="xsd:string" name="Meta"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:schema>
    ```

    A schema tells the XML parser what elements to expect, their order, and their level in the tree. It also restricts the allowed values for the XML attributes. Any discrepancies between these declarations and an actual XML document should render it invalid and make the parser reject the document.
    
    Additionally, some tools can leverage this information to produce a piece of code that hides the details of XML parsing from you. After installing the library, you should be able to run the generateDS command in your active virtual environment:

    ```shell  
    $ generateDS -o models.py models.xsd
    ```

    It will create a new file named models.py in the same directory with the generated Python source code. You can then import that module and use it to parse the incoming messages:
    
    <!--python控制台语法-->
    ```pycon
    >>> from models import parseString

    >>> event = parseString("""\
    ... <KeyboardEvent>
    ...     <Type>keydown</Type>
    ...     <Timestamp>253459.17999999982</Timestamp>
    ...     <Key>
    ...         <Code>Digit2</Code>
    ...         <Unicode>@</Unicode>
    ...     </Key>
    ...     <Modifiers>
    ...         <Alt>false</Alt>
    ...         <Ctrl>false</Ctrl>
    ...         <Shift>true</Shift>
    ...         <Meta>false</Meta>
    ...     </Modifiers>
    ... </KeyboardEvent>""", silence=True)
    
    >>> event.Type, event.Key.Code
    ('keydown', 'Digit2')
    ```

    It looks similar to the `lxml.objectify` example shown earlier. The difference is that using data binding enforces compliance with the schema, whereas `lxml.objectify` produces objects dynamically no matter if they’re semantically correct.

## 使用安全解析器拆除 XML 炸弹

=== "中文"

    Python 标准库中的 XML 解析器容易受到许多安全威胁的影响，这些威胁充其量可能导致[拒绝服务 (DoS)]https://en.wikipedia.org/wiki/Denial-of-service_attack 或数据丢失。 公平地说，这不是他们的错。 它们只是遵循 XML 标准的规范，该标准比大多数人所知道的更加复杂和强大。

    !!! info "Note"

        请注意，您应该明智地使用您将要看到的信息。 您不想最终成为攻击者，让自己承担法律后果，或者面临终身禁止使用特定服务的情况。

    最常见的攻击之一是 **XML 炸弹**，也称为[十亿笑声攻击](https://en.wikipedia.org/wiki/Billion_laughs_attack)。 该攻击利用 DTD 中的**实体扩展**来炸毁内存并尽可能长时间地占用 CPU。 阻止未受保护的 Web 服务器接收新流量所需的只是以下几行 XML 代码：

    ```python
    import xml.etree.ElementTree as ET
    ET.fromstring("""\
    <?xml version="1.0"?>
    <!DOCTYPE lolz [
     <!ENTITY lol "lol">
     <!ELEMENT lolz (#PCDATA)>
     <!ENTITY lol1 "&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;">
     <!ENTITY lol2 "&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;">
     <!ENTITY lol3 "&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;">
     <!ENTITY lol4 "&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;">
     <!ENTITY lol5 "&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;">
     <!ENTITY lol6 "&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;">
     <!ENTITY lol7 "&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;">
     <!ENTITY lol8 "&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;">
     <!ENTITY lol9 "&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;">
    ]>
    <lolz>&lol9;</lolz>""")
    ```

    天真的解析器将尝试解析自定义实体 `&lol9`; 通过检查 DTD 放置在文档根目录中。 然而，该实体本身多次引用另一个实体，而另一个实体又引用另一个实体，依此类推。 当您[运行上面的脚本](https://realpython.com/run-python-scripts/)时，您会注意到您的内存和处理单元出现一些令人不安的情况：
    

    看看主内存和交换分区如何在短短几秒钟内耗尽，而其中一个 CPU 则以 100% 的容量运行。 当系统内存已满时，记录会突然停止，然后在 Python 进程被杀死后恢复。
    
    另一种流行的攻击类型称为 [XXE](https://en.wikipedia.org/wiki/XML_external_entity_attack)，它利用**一般外部实体**来读取本地文件并发出网络请求。 不过，从 Python 3.7.1 开始，为了提高安全性，该功能已默认禁用。 如果您信任您的数据，那么您可以告诉 SAX 解析器无论如何处理外部实体：

    ```pycon
    >>> from xml.sax import make_parser
    >>> from xml.sax.handler import feature_external_ges
    
    >>> parser = make_parser()
    >>> parser.setFeature(feature_external_ges, True)
    ```

    该解析器将能够读取您计算机上的本地文件。 它可能会在类 Unix 操作系统上提取用户名，例如：

    ```python
    >>> from xml.dom.minidom import parseString

    >>> xml = """\
    ... <?xml version="1.0" encoding="UTF-8"?>
    ... <!DOCTYPE root [
    ...     <!ENTITY usernames SYSTEM "/etc/passwd">
    ... ]>
    ... <root>&usernames;</root>"""
    
    >>> document = parseString(xml, parser)
    >>> print(document.documentElement.toxml())
    <root>root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    ⋮
    realpython:x:1001:1001:Real Python,,,:/home/realpython:/bin/bash
    </root>
    ```

    通过网络[发送该数据](https://www.youtube.com/watch?v=gjm6VHZa_8s)到远程服务器是完全可行的！

    现在，您如何保护自己免受此类攻击？ Python 官方文档明确警告您使用内置 XML 解析器的风险，并建议在关键任务应用程序中切换到外部包。 虽然未随 Python 一起分发，但 [defusedxml](https://pypi.org/project/defusedxml/) 是标准库中所有解析器的**直接替代品**。
    
    该库施加了严格的限制并禁用了许多危险的 XML 功能。 它应该可以阻止大多数众所周知的攻击，包括刚才描述的两种攻击。 要使用它，请从 PyPI 获取库并相应地替换导入语句：

    ```pycon
    >>> import defusedxml.ElementTree as ET
    >>> ET.parse("bomb.xml")
    Traceback (most recent call last):
      ...
        raise EntitiesForbidden(name, value, base, sysid, pubid, notation_name)
    defusedxml.common.EntitiesForbidden:
     EntitiesForbidden(name='lol', system_id=None, public_id=None)
    ```

    就是这样！ 禁止的功能将不再通过。

=== "原文"

    The XML parsers in Python’s standard library are vulnerable to a host of security threats that can lead to [denial-of-service (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) or data loss, at best. That isn’t their fault, to be fair. They just follow the specification of the XML standard, which is more complicated and powerful than most people know.

    !!! info "Note"

        Note: Please be advised that you should use the information you’re about to see wisely. You don’t want to wind up being the attacker, exposing yourself to legal consequences, or facing lifetime banishment from using a particular service.

    One of the most common attacks is the **XML Bomb**, also known as the [billion laughs attack](https://en.wikipedia.org/wiki/Billion_laughs_attack). The attack exploits **entity expansion** in DTD to blow up the memory and occupy the CPU for as long as possible. All you need to stop an unprotected web server from receiving new traffic are these few lines of XML code:

    ```python
    import xml.etree.ElementTree as ET
    ET.fromstring("""\
    <?xml version="1.0"?>
    <!DOCTYPE lolz [
     <!ENTITY lol "lol">
     <!ELEMENT lolz (#PCDATA)>
     <!ENTITY lol1 "&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;">
     <!ENTITY lol2 "&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;">
     <!ENTITY lol3 "&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;">
     <!ENTITY lol4 "&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;">
     <!ENTITY lol5 "&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;">
     <!ENTITY lol6 "&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;">
     <!ENTITY lol7 "&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;">
     <!ENTITY lol8 "&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;">
     <!ENTITY lol9 "&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;">
    ]>
    <lolz>&lol9;</lolz>""")
    ```

    A naive parser will try to resolve the custom entity &lol9; placed in the document root by inspecting the DTD. However, that entity itself refers to another entity several times, which refers to yet another entity, and so forth. When you [run the script](https://realpython.com/run-python-scripts/) above, you’ll notice something disturbing about your memory and the processing unit:
    

    Look how the main memory and the swap partition are exhausted in just a matter of seconds while one of the CPUs works at 100% of its capacity. The recording stops abruptly when the system memory becomes full and then resumes after the Python process gets killed.
    
    Another popular type of attack known as [XXE](https://en.wikipedia.org/wiki/XML_external_entity_attack) takes advantage of **general external entities** to read local files and make network requests. Nevertheless, starting from Python 3.7.1, this feature has been disabled by default to increase security. If you trust your data, then you can tell the SAX parser to process external entities anyway:

    ```pycon
    >>> from xml.sax import make_parser
    >>> from xml.sax.handler import feature_external_ges
    
    >>> parser = make_parser()
    >>> parser.setFeature(feature_external_ges, True)
    ```

    This parser will be able to read local files on your computer. It may pull usernames on a Unix-like operating system, for example:

    ```python
    >>> from xml.dom.minidom import parseString

    >>> xml = """\
    ... <?xml version="1.0" encoding="UTF-8"?>
    ... <!DOCTYPE root [
    ...     <!ENTITY usernames SYSTEM "/etc/passwd">
    ... ]>
    ... <root>&usernames;</root>"""
    
    >>> document = parseString(xml, parser)
    >>> print(document.documentElement.toxml())
    <root>root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    ⋮
    realpython:x:1001:1001:Real Python,,,:/home/realpython:/bin/bash
    </root>
    ```

    It’s perfectly feasible to [send that data](https://www.youtube.com/watch?v=gjm6VHZa_8s) over the network to a remote server!

    Now, how can you protect yourself from such attacks? The Python official documentation prominently warns you about the risks of using the built-in XML parsers and recommends switching to an external package in mission-critical applications. While not distributed with Python, [defusedxml](https://pypi.org/project/defusedxml/) is a **drop-in replacement** for all the parsers in the standard library.
    
    The library imposes strict limits and disables a lot of the dangerous XML features. It should stop most of the well-known attacks, including the two just described. To use it, grab the library from PyPI and replace your import statements accordingly:

    ```pycon
    >>> import defusedxml.ElementTree as ET
    >>> ET.parse("bomb.xml")
    Traceback (most recent call last):
      ...
        raise EntitiesForbidden(name, value, base, sysid, pubid, notation_name)
    defusedxml.common.EntitiesForbidden:
     EntitiesForbidden(name='lol', system_id=None, public_id=None)
    ```

    That’s it! Forbidden features won’t make it through anymore.

## 结语

=== "中文"

    XML 数据格式是一种成熟且强大的标准，至今仍在使用，特别是在企业环境中。 选择正确的 XML 解析器对于在性能、安全性、合规性和便利性之间找到**最佳平衡点**至关重要。

    本教程为您提供了详细的**路线图**，帮助您在 Python 中令人困惑的 XML 解析器迷宫中导航。 您知道在哪里走捷径以及如何避免死胡同，从而节省大量时间。
    
    **在本教程中，您学习了如何：**
    
    - 选择正确的 XML **解析模型**
    - 使用 **标准库** 中的 XML 解析器
    - 使用主要的 **XML 解析库**
    - 使用**数据绑定**以声明方式解析 XML 文档
    - 使用安全的 XML 解析器消除 **安全漏洞**
    
    现在，您了解了解析 XML 文档的不同策略及其优缺点。 有了这些知识，您就可以为您的特定用例选择最合适的 XML 解析器，甚至可以**组合**多个解析器来更快地读取数 GB XML 文件。

=== "原文"

    The XML data format is a mature and surprisingly powerful standard that is still in use today, especially in the enterprise setting. Choosing the right XML parser is crucial in finding the **sweet spot** between performance, security, compliance, and convenience.

    This tutorial puts a detailed **roadmap** in your hand to navigate the confusing maze of XML parsers in Python. You know where to take the shortcuts and how to avoid dead ends, saving you lots of time.
    
    **In this tutorial, you learned how to:**
    
    - Choose the right XML **parsing model**
    - Use the XML parsers in the **standard library**
    - Use major **XML parsing libraries**
    - Parse XML documents declaratively using **data binding**
    - Use safe XML parsers to eliminate **security vulnerabilities**
    
    Now, you understand the different strategies for parsing XML documents as well as their strengths and weaknesses. With this knowledge, you’re able to pick the most suitable XML parser for your specific use case and even **combine** more than one to read multi-gigabyte XML files faster.
