# Python Asyncio: The Complete Guide

=== "English"

    **Asyncio** allows us to use asynchronous programming with coroutine-based concurrency in Python.

    Although asyncio has been available in Python for many years now, it remains one of the most interesting and yet one of the most frustrating areas of Python.

    It is just plain hard to get started with asyncio for new developers.

    This guide provides a detailed and comprehensive(全面的) review of asyncio in Python, including how to define, create and run coroutines, what is asynchronous programming, what is non-blocking-io, concurrency primitives(原始并发) used with coroutines, common questions, and best practices.

    This is a massive 29,000+ word guide. You may want to bookmark it so you can refer to it as you develop your concurrent programs.

    Let’s dive in.

=== "Chinese"

    **Asyncio** 允许我们在 Python 中使用基于协程的并发的异步编程。

    尽管 asyncio 已经在 Python 中使用很多年了，但它仍然是 Python 中最有趣但也是最令人沮丧的领域之一。

    对于新开发人员来说，开始使用 asyncio 非常困难。

    本指南对 Python 中的 asyncio 进行了详细而全面的回顾，包括如何定义、创建和运行协程、什么是异步编程、什么是非阻塞 io、与协程一起使用的并发原语、 常见问题和最佳实践。

    这是一本超过 29,000 字的海量指南。 您可能需要为其添加书签，以便在开发并发程序时可以参考它。

    让我们深入了解一下。

## 1. What is Asynchronous Programming

=== "English"

=== "Chinese"

### 1.1 Asynchronous Tasks

=== "English"

=== "Chinese"

### 1.2 Asynchronous Programming

=== "English"

=== "Chinese"

### 1.3 Asynchronous Programming in Python

=== "English"

=== "Chinese"

## 2. What is Asyncio

=== "English"

=== "Chinese"

### 2.1 Changes to Python to add Support for Coroutines

=== "English"

=== "Chinese"

### 2.2 The asyncio Module

=== "English"

=== "Chinese"

## 3. When to Use Asyncio

=== "English"

=== "Chinese"

### 3.1 Reasons to Use Asyncio in Python

=== "English"

=== "Chinese"

### 3.2 Other Reasons to Use Asyncio

=== "English"

=== "Chinese"

### 3.3 When to Not Use Asyncio

=== "English"

=== "Chinese"

## 4. Coroutines in Python

=== "English"

=== "Chinese"

### 4.1 What is a Coroutine

=== "English"

=== "Chinese"

### 4.2 Coroutine vs Routine and Subroutine

=== "English"

=== "Chinese"

### 4.3 Coroutine vs Generator

=== "English"

=== "Chinese"

### 4.4 Coroutine vs Task

=== "English"

=== "Chinese"

### 4.5 Coroutine vs Thread

=== "English"

=== "Chinese"

### 4.6 Coroutine vs Process

=== "English"

=== "Chinese"

### 4.7 When Were Coroutines Added to Python

=== "English"

=== "Chinese"

## 5. Define, Create and Run Coroutines

=== "English"

=== "Chinese"

### 5.1 How to Define a Coroutine

=== "English"

=== "Chinese"

### 5.2 How to Create a Coroutine

=== "English"

=== "Chinese"

### 5.3 How to Run a Coroutine From Python

=== "English"

=== "Chinese"

## 6. What is the Event Loop

=== "English"

=== "Chinese"

### 6.1 What is the Asyncio Event Loop

=== "English"

=== "Chinese"

### 6.2 How To Start and Get An Event Loop

=== "English"

=== "Chinese"

### 6.3 What is an Event Loop Object

=== "English"

=== "Chinese"

### 6.4 Why Get Access to The Event Loop

=== "English"

=== "Chinese"

## 7. Create and Run Asyncio Tasks

=== "English"

=== "Chinese"

### 7.1 What is an Asyncio Task

=== "English"

=== "Chinese"

### 7.2 How to Create a Task

=== "English"

=== "Chinese"

### 7.3 When Does a Task Run?

=== "English"

=== "Chinese"

## 8. Work With and Query Tasks

=== "English"

=== "Chinese"

### 8.1 Task Life-Cycle

=== "English"

=== "Chinese"

### 8.2 How to Check Task Status

=== "English"

=== "Chinese"

### 8.3 How to Get Task Result

=== "English"

=== "Chinese"

### 8.4 How to Get Task Exception

=== "English"

=== "Chinese"

### 8.5 How to Cancel a Task

=== "English"

=== "Chinese"

### 8.6 How to Use Callback With a Task

=== "English"

=== "Chinese"

### 8.7 How to Set the Task Name

=== "English"

=== "Chinese"

## 9. Current and Running Tasks

=== "English"

=== "Chinese"

### 9.1 How to Get the Current Task

=== "English"

=== "Chinese"

### 9.2 How to Get All Tasks

=== "English"

=== "Chinese"

## 10. Run Many Coroutines Concurrently

=== "English"

=== "Chinese"

### 10.1 What is Asyncio gather()

=== "English"

=== "Chinese"

### 10.2 How to use Asyncio gather()

=== "English"

=== "Chinese"

### 10.3 Example of gather() For Many Coroutines in a List

=== "English"

=== "Chinese"

## 11. Wait for A Collection of Tasks

=== "English"

=== "Chinese"

### 11.1 What is asyncio.wait()

=== "English"

=== "Chinese"

### 11.2 How to Use asyncio.wait()

=== "English"

=== "Chinese"

### 11.3 Example of Waiting for All Tasks

=== "English"

=== "Chinese"

## 12. Wait for a Coroutine with a Time Limit

=== "English"

=== "Chinese"

### 12.1 What is Asyncio wait_for()

=== "English"

=== "Chinese"

### 12.2 How to Use Asyncio wait_for()

=== "English"

=== "Chinese"

### 12.3 Example of Asyncio wait_for() With a Timeout

=== "English"

=== "Chinese"

## 13. Shield Tasks from Cancellation

=== "English"

=== "Chinese"

### 13.1 What is Asyncio shield()

=== "English"

=== "Chinese"

### 13.2 How to Use Asyncio shield()

=== "English"

=== "Chinese"

### 13.3 Example of Asyncio shield() for a Task

=== "English"

=== "Chinese"

## 14. Run a Blocking Task in Asyncio

=== "English"

=== "Chinese"

### 14.1 Need to Run Blocking Tasks in Asyncio

=== "English"

=== "Chinese"

### 14.2 How to Run Blocking Tasks

=== "English"

=== "Chinese"

### 14.3 Example of Running I/O-Bound Task in Asyncio with to_thread()

=== "English"

=== "Chinese"

## 15. Asynchronous Iterators

=== "English"

=== "Chinese"

### 15.1 What Are Asynchronous Iterators

=== "English"

=== "Chinese"

### 15.2 What is the “async for” loop?

=== "English"

=== "Chinese"

### 15.3 How to Use Asynchronous Iterators

=== "English"

=== "Chinese"

### 15.4 Example of an Asynchronous Iterator

=== "English"

=== "Chinese"

## 16. Asynchronous Generators

=== "English"

=== "Chinese"

### 16.1 What Are Asynchronous Generators

=== "English"

=== "Chinese"

### 16.2 How to Use an Asynchronous Generator

=== "English"

=== "Chinese"

### 16.3 Example of an Asynchronous Generator

=== "English"

=== "Chinese"

## 17. Asynchronous Context Managers

=== "English"

=== "Chinese"

### 17.1 What is an Asynchronous Context Manager

=== "English"

=== "Chinese"

### 17.2 How to Use Asynchronous Context Managers

=== "English"

=== "Chinese"

### 17.3 Example of an Asynchronous Context Manager and “async with”

=== "English"

=== "Chinese"

## 18. Asynchronous Comprehensions

=== "English"

=== "Chinese"

### 18.1 What are Asynchronous Comprehensions

=== "English"

=== "Chinese"

### 18.2 Comprehensions

=== "English"

=== "Chinese"

### 18.3 Asynchronous Comprehensions

=== "English"

=== "Chinese"

### 18.4 Await Comprehensions

=== "English"

=== "Chinese"

## 19. Run Commands in Non-Blocking Subprocesses

=== "English"

=== "Chinese"

### 19.1 What is asyncio.subprocess.Process

=== "English"

=== "Chinese"

### 19.2 How to Run a Command Directly

=== "English"

=== "Chinese"

### 19.3 How to Run a Command Via the Shell

=== "English"

=== "Chinese"

## 20. Non-Blocking Streams

=== "English"

=== "Chinese"

### 20.1 Asyncio Streams

=== "English"

=== "Chinese"

### 20.2 How to Open a Connection

=== "English"

=== "Chinese"

### 20.3 How to Start a Server

=== "English"

=== "Chinese"

### 20.4 How to Write Data with the StreamWriter

=== "English"

=== "Chinese"

### 20.5 How to Read Data with the StreamReader

=== "English"

=== "Chinese"

### 20.6 How to Close Connection

=== "English"

=== "Chinese"

## 21. Example of Checking Website Status

=== "English"

    We can query the HTTP status of websites using asyncio by opening a stream and writing and reading HTTP requests and responses.
    
    We can then use asyncio to query the status of many websites concurrently, and even report the results dynamically.
    
    Let’s get started.

=== "Chinese"

    我们可以使用 asyncio 通过打开流并写入和读取 HTTP 请求和响应来查询网站的 HTTP 状态。

    然后我们可以使用 asyncio 同时查询多个网站的状态，甚至动态报告结果。
    
    让我们开始吧。

### 21.1 How to Check HTTP Status with Asyncio

=== "English"

    The asyncio module provides support for opening socket connections and reading and writing data via streams.
    
    We can use this capability to check the status of web pages.
    
    This involves perhaps four steps, they are:
    
    1. Open a connection
    2. Write a request
    3. Read a response
    4. Close the connection
    
    Let’s take a closer look at each part in turn.

=== "Chinese"

    asyncio 模块提供对打开套接字连接以及通过流读写数据的支持。
    
    我们可以使用此功能来检查网页的状态。
    
    这可能涉及四个步骤，它们是：
    
    1. 打开连接
    2. 写一个请求
    3. 读一个响应
    4. 关闭连接
    
    让我们依次仔细看看每个部分。

### 21.2 Open HTTP Connection

=== "English"

    A connection can be opened in asyncio using the [asyncio.open_connection()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.open_connection) function.
    
    Among many arguments, the function takes the string hostname and integer port number
    
    This is a coroutine that must be awaited and returns a StreamReader and a StreamWriter for reading and writing with the socket.
    
    This can be used to open an HTTP connection on port 80.
    
    For example:
    
    ```python
    ...
    # open a socket connection
    reader, writer = await asyncio.open_connection('www.google.com', 80)
    ```
    
    We can also open an SSL connection using the **ssl=True** argument. This can be used to open an HTTPS connection on port 443.
    
    For example:
    
    ```python
    ...
    # open a socket connection
    reader, writer = await asyncio.open_connection('www.google.com', 443, ssl=True)
    ```

=== "Chinese"

    可以使用 [asyncio.open_connection()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.open_connection) 函数在 asyncio 中打开连接。
    
    在许多参数中，该函数采用字符串主机名和整数端口号
    
    这是一个必须等待的协程，并返回一个 **StreamReader** 和一个 **StreamWriter**，用于使用套接字进行读写。
    
    这可用于在端口 80 上打开 HTTP 连接。
    
    例如:
    
    ```python
    ...
    # 打开套接字连接
    reader, writer = await asyncio.open_connection('www.google.com', 80)
    ```
    
    我们还可以使用 **ssl=True** 参数打开 SSL 连接。 这可用于在端口 443 上打开 HTTPS 连接。
    
    例如:
    
    ```python
    ...
    # 打开套接字连接
    reader, writer = await asyncio.open_connection('www.google.com', 443, ssl=True)
    ```

### 21.3 Write HTTP Request

=== "English"

    Once open, we can write a query to the **StreamWriter** to make an HTTP request.
    
    For example, an [HTTP version 1.1 request](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) is in plain text. We can request the file path ‘/’, which may look as follows:
    
    ```python
    GET / HTTP/1.1
    Host: www.google.com
    ```
    
    Importantly, there must be a carriage return and a line feed (\r\n) at the end of each line, and an empty line at the end.
    
    As Python strings this may look as follows:
    
    ```python
    'GET / HTTP/1.1\r\n'
    'Host: www.google.com\r\n'
    '\r\n'
    ```
    
    You can learn more about HTTP v1.1 request messages here:
    
    - [HTTP/1.1 request messages](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#HTTP/1.1_request_messages)
    
    This string must be encoded as bytes before being written to the [StreamWriter](https://docs.python.org/3/library/asyncio-stream.html#streamwriter).
    
    This can be achieved using the [encode()](https://docs.python.org/3/library/stdtypes.html#str.encode) method on the string itself.
    
    The default ‘**utf-8**‘ encoding may be sufficient.
    
    For example:
    
    ```python
    ...
    # encode string as bytes
    byte_data = string.encode()
    ```
    
    You can see a listing of encodings here:
    
    - [Python Standard Encodings](https://docs.python.org/3/library/codecs.html#standard-encodings)
    
    The bytes can then be written to the socket via the **StreamWriter** via the [write()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.StreamWriter.write) method.
    
    For example:
    
    ```python
    ...
    # encode string as bytes
    byte_data = string.encode()
    ```
    
    You can see a listing of encodings here:
    
    - [Python Standard Encodings](https://docs.python.org/3/library/codecs.html#standard-encodings)
    
    The bytes can then be written to the socket via the **StreamWriter** via the [write()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.StreamWriter.write) method.
    
    For example:
    
    ```python
    ...
    # write query to socket
    writer.write(byte_data)
    ```
    
    After writing the request, it is a good idea to wait for the byte data to be sent and for the socket to be ready.
    
    This can be achieved by the **drain()** method.
    
    This is a coroutine that must be awaited.
    
    For example:
    
    ```python
    ...
    # wait for the socket to be ready.
    await writer.drain()
    ```

=== "Chinese"

    打开后，我们可以向 **StreamWriter** 写入查询以发出 HTTP 请求。
    
    例如，[HTTP 版本 1.1 请求](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) 是纯文本形式。 我们可以请求文件路径“/”，如下所示：
    
    ```python
    GET / HTTP/1.1
    Host: www.google.com
    ```
    
    重要的是，每行末尾必须有回车符和换行符（\r\n），并且末尾有一个空行。
    
    作为 Python 字符串，这可能如下所示：
    
    ```python
    'GET / HTTP/1.1\r\n'
    'Host: www.google.com\r\n'
    '\r\n'
    ```
    
    您可以在此处了解有关 HTTP v1.1 请求消息的更多信息：
    
    - [HTTP/1.1 请求消息](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#HTTP/1.1_request_messages)
    
    在写入 [StreamWriter](https://docs.python.org/3/library/asyncio-stream.html#streamwriter) 之前，必须将该字符串编码为字节。
    
    这可以通过对字符串本身使用 [encode()](https://docs.python.org/3/library/stdtypes.html#str.encode) 方法来实现。
    
    默认的“**utf-8**”编码可能就足够了。
    
    例如:
    
    ```python
    ...
    # 将字符串编码为字节
    byte_data = string.encode()
    ```
    
    您可以在此处查看编码列表：
    
    - [Python 标准编码](https://docs.python.org/3/library/codecs.html#standard-encodings)
    
    然后可以通过 **StreamWriter** 通过 [write()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.StreamWriter.write) 将字节写入套接字 方法。
    
    例如:
    
    ```python
    ...
    # 将查询写入套接字
    writer.write(byte_data)
    ```
    
    写入请求后，最好等待字节数据发送和套接字准备就绪。
    
    这可以通过 **drain()** 方法来实现。
    
    这是一个必须等待的协程。
    
    例如:
    
    ```python
    ...
    # 等待套接字准备好。
    await writer.drain()
    ```

### 21.4 Read HTTP Response

=== "English"

    Once the HTTP request has been made, we can read the response.
    
    This can be achieved via the [StreamReader](https://docs.python.org/3/library/asyncio-stream.html#streamreader) for the socket.
    
    The response can be read using the **read()** method which will read a chunk of bytes, or the **readline()** method which will read one line of bytes.
    
    We might prefer the **readline()** method because we are using the text-based HTTP protocol which sends HTML data one line at a time.
    
    The **readline()** method is a coroutine and must be awaited.
    
    For example:
    
    ```python
    ...
    # read one line of response
    line_bytes = await reader.readline()
    ```
    
    [HTTP 1.1 responses](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#HTTP/1.1_response_messages) are composed of two parts, a header separated by an empty line, then the body terminating with an empty line.
    
    The header has information about whether the request was successful and what type of file will be sent, and the body contains the content of the file, such as an HTML webpage.
    
    The first line of the HTTP header contains the HTTP status for the requested page on the server.
    
    You can learn more about HTTP v1.1 responses here:
    
    - [HTTP/1.1 response messages](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#HTTP/1.1_response_messages)
    
    Each line must be decoded from bytes into a string.
    
    This can be achieved using the [decode()](https://docs.python.org/3/library/stdtypes.html#bytes.decode) method on the byte data. Again, the default encoding is ‘**utf_8**‘.
    
    For example:
    
    ```python
    ...
    # decode bytes into a string
    line_data = line_bytes.decode()
    ```

=== "Chinese"

    一旦发出 HTTP 请求，我们就可以读取响应。
    
    这可以通过套接字的 [StreamReader](https://docs.python.org/3/library/asyncio-stream.html#streamreader) 来实现。
    
    可以使用 **read()** 方法读取响应，该方法将读取一大块字节，或者使用 **readline()** 方法读取一行字节。
    
    我们可能更喜欢 **readline()** 方法，因为我们使用基于文本的 HTTP 协议，它一次发送一行 HTML 数据。
    
    **readline()** 方法是一个协程，必须等待。
    
    例如:
    
    ```python
    ...
    # 读取一行响应
    line_bytes = await reader.readline()
    ```
    
    [HTTP 1.1 响应](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#HTTP/1.1_response_messages) 由两部分组成，一个由空行分隔的标头，然后是由空行终止的正文。
    
    header 包含有关请求是否成功以及将发送什么类型的文件的信息，body 包含文件的内容，例如 HTML 网页。
    
    HTTP 标头的第一行包含服务器上所请求页面的 HTTP 状态。
    
    您可以在此处了解有关 HTTP v1.1 响应的更多信息：
    
    - [HTTP/1.1 响应消息](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#HTTP/1.1_response_messages)
    
    每一行都必须从字节解码为字符串。
    
    这可以通过对字节数据使用 [decode()](https://docs.python.org/3/library/stdtypes.html#bytes.decode) 方法来实现。 同样，默认编码是“**utf_8**”。
    
    例如:
    
    ```python
    ...
    # 将字节解码为字符串
    line_data = line_bytes.decode()
    ```

### 21.5 Close HTTP Connection

=== "English"

    We can close the socket connection by closing the **StreamWriter**.
    
    This can be achieved by calling the [close()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.StreamWriter.close) method.
    
    For example:
    
    ```python
    ...
    # close the connection
    writer.close()
    ```
    
    This does not block and may not close the socket immediately.
    
    Now that we know how to make HTTP requests and read responses using **asyncio**, let’s look at some worked examples of checking web page statuses.

=== "Chinese"

    我们可以通过关闭 **StreamWriter** 来关闭套接字连接。
    
    这可以通过调用 [close()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.StreamWriter.close) 方法来实现。
    
    例如:
    
    ```python
    ...
    # 关闭连接
    writer.close()
    ```
    
    这不会阻塞并且可能不会立即关闭套接字。
    
    现在我们知道如何使用 **asyncio** 发出 HTTP 请求并读取响应，让我们看一些检查网页状态的示例。

### 21.6 Example of Checking HTTP Status Sequentially

=== "English"

    We can develop an example to check the HTTP status for multiple websites using asyncio.
    
    In this example, we will first develop a coroutine that will check the status of a given URL. We will then call this coroutine once for each of the top 10 websites.
    
    Firstly, we can define a coroutine that will take a URL string and return the HTTP status.
    
    ```python
    # get the HTTP/S status of a webpage
    async def get_status(url):
        # ...
    ```
    
    The URL must be parsed into its constituent components.
    
    We require the hostname and file path when making the HTTP request. We also need to know the URL scheme (HTTP or HTTPS) in order to determine whether SSL is required nor not.
    
    This can be achieved using the [urllib.parse.urlsplit()](https://docs.python.org/3/library/urllib.parse.html) function that takes a URL string and returns a named tuple of all the URL elements.
    
    ```python
    ...
    # split the url into components
    url_parsed = urlsplit(url)
    ```
    
    We can then open the HTTP connection based on the URL scheme and use the URL hostname.
    
    ```python
    ...
    # open the connection
    if url_parsed.scheme == 'https':
        reader, writer = await asyncio.open_connection(url_parsed.hostname, 443, ssl=True)
    else:
        reader, writer = await asyncio.open_connection(url_parsed.hostname, 80)
    ```
    
    Next, we can create the HTTP GET request using the hostname and file path and write the encoded bytes to the socket using the StreamWriter.
    
    ```python
    ...
    # send GET request
    query = f'GET {url_parsed.path} HTTP/1.1\r\nHost: {url_parsed.hostname}\r\n\r\n'
    # write query to socket
    writer.write(query.encode())
    # wait for the bytes to be written to the socket
    await writer.drain()
    ```
    
    Next, we can read the HTTP response.
    
    We only require the first line of the response that contains the HTTP status.
    
    ```python
    ...
    # read the single line response
    response = await reader.readline()
    ```
    
    The connection can then be closed.
    
    ```python
    ...
    # close the connection
    writer.close()
    ```
    
    Finally, we can decode the bytes read from the server, remote trailing white space, and return the HTTP status.
    
    ```python
    ...
    # decode and strip white space
    status = response.decode().strip()
    # return the response
    return status
    ```
    
    Tying this together, the complete get_status() coroutine is listed below.
    
    It does not have any error handling, such as the case where the host cannot be reached or is slow to respond.
    
    These additions would make a nice extension for the reader.
    
    ```python
    # get the HTTP/S status of a webpage
    async def get_status(url):
        # split the url into components
        url_parsed = urlsplit(url)
        # open the connection
        if url_parsed.scheme == 'https':
            reader, writer = await asyncio.open_connection(url_parsed.hostname, 443, ssl=True)
        else:
            reader, writer = await asyncio.open_connection(url_parsed.hostname, 80)
        # send GET request
        query = f'GET {url_parsed.path} HTTP/1.1\r\nHost: {url_parsed.hostname}\r\n\r\n'
        # write query to socket
        writer.write(query.encode())
        # wait for the bytes to be written to the socket
        await writer.drain()
        # read the single line response
        response = await reader.readline()
        # close the connection
        writer.close()
        # decode and strip white space
        status = response.decode().strip()
        # return the response
        return status
    ```
    
    Next, we can call the get_status() coroutine for multiple web pages or websites we want to check.
    
    In this case, we will define a list of the top 10 web pages in the world.
    
    ```python
    ...
    # list of top 10 websites to check
    sites = ['https://www.google.com/',
        'https://www.youtube.com/',
        'https://www.facebook.com/',
        'https://twitter.com/',
        'https://www.instagram.com/',
        'https://www.baidu.com/',
        'https://www.wikipedia.org/',
        'https://yandex.ru/',
        'https://yahoo.com/',
        'https://www.whatsapp.com/'
        ]
    ```
    
    We can then query each, in turn, using our get_status() coroutine.
    
    In this case, we will do so sequentially in a loop, and report the status of each in turn.
    
    ```python
    ...
    # check the status of all websites
    for url in sites:
        # get the status for the url
        status = await get_status(url)
        # report the url and its status
        print(f'{url:30}:\t{status}')
    ```
    
    We can do better than sequential when using asyncio, but this provides a good starting point that we can improve upon later.
    
    Tying this together, the main() coroutine queries the status of the top 10 websites.
    
    ```python
    # main coroutine
    async def main():
        # list of top 10 websites to check
        sites = ['https://www.google.com/',
            'https://www.youtube.com/',
            'https://www.facebook.com/',
            'https://twitter.com/',
            'https://www.instagram.com/',
            'https://www.baidu.com/',
            'https://www.wikipedia.org/',
            'https://yandex.ru/',
            'https://yahoo.com/',
            'https://www.whatsapp.com/'
            ]
        # check the status of all websites
        for url in sites:
            # get the status for the url
            status = await get_status(url)
            # report the url and its status
            print(f'{url:30}:\t{status}')
    ```
    
    Finally, we can create the main() coroutine and use it as the entry point to the asyncio program.
    
    ```python
    ...
    # run the asyncio program
    asyncio.run(main())
    ```
    
    Tying this together, the complete example is listed below.
    
    ```python
    # SuperFastPython.com
    # check the status of many webpages
    import asyncio
    from urllib.parse import urlsplit
     
    # get the HTTP/S status of a webpage
    async def get_status(url):
        # split the url into components
        url_parsed = urlsplit(url)
        # open the connection
        if url_parsed.scheme == 'https':
            reader, writer = await asyncio.open_connection(url_parsed.hostname, 443, ssl=True)
        else:
            reader, writer = await asyncio.open_connection(url_parsed.hostname, 80)
        # send GET request
        query = f'GET {url_parsed.path} HTTP/1.1\r\nHost: {url_parsed.hostname}\r\n\r\n'
        # write query to socket
        writer.write(query.encode())
        # wait for the bytes to be written to the socket
        await writer.drain()
        # read the single line response
        response = await reader.readline()
        # close the connection
        writer.close()
        # decode and strip white space
        status = response.decode().strip()
        # return the response
        return status
     
    # main coroutine
    async def main():
        # list of top 10 websites to check
        sites = ['https://www.google.com/',
            'https://www.youtube.com/',
            'https://www.facebook.com/',
            'https://twitter.com/',
            'https://www.instagram.com/',
            'https://www.baidu.com/',
            'https://www.wikipedia.org/',
            'https://yandex.ru/',
            'https://yahoo.com/',
            'https://www.whatsapp.com/'
            ]
        # check the status of all websites
        for url in sites:
            # get the status for the url
            status = await get_status(url)
            # report the url and its status
            print(f'{url:30}:\t{status}')
     
    # run the asyncio program
    asyncio.run(main())
    ```
    
    Running the example first creates the main() coroutine and uses it as the entry point into the program.
    
    The main() coroutine runs, defining a list of the top 10 websites.
    
    The list of websites is then traversed sequentially. The main() coroutine suspends and calls the get_status() coroutine to query the status of one website.
    
    The get_status() coroutine runs, parses the URL, and opens a connection. It constructs an HTTP GET query and writes it to the host. A response is read, decoded, and returned.
    
    The main() coroutine resumes and reports the HTTP status of the URL.
    
    This is repeated for each URL in the list.
    
    The program takes about 5.6 seconds to complete, or about half a second per URL on average.
    
    This highlights how we can use asyncio to query the HTTP status of webpages.
    
    Nevertheless, it does not take full advantage of the asyncio to execute tasks concurrently.
    
    ```text
    https://www.google.com/       : HTTP/1.1 200 OK
    https://www.youtube.com/      : HTTP/1.1 200 OK
    https://www.facebook.com/     : HTTP/1.1 302 Found
    https://twitter.com/          : HTTP/1.1 200 OK
    https://www.instagram.com/    : HTTP/1.1 200 OK
    https://www.baidu.com/        : HTTP/1.1 200 OK
    https://www.wikipedia.org/    : HTTP/1.1 200 OK
    https://yandex.ru/            : HTTP/1.1 302 Moved temporarily
    https://yahoo.com/            : HTTP/1.1 301 Moved Permanently
    https://www.whatsapp.com/     : HTTP/1.1 302 Found
    ```
    
    Next, let’s look at how we might update the example to execute the coroutines concurrently.

=== "Chinese"

    我们可以开发一个示例来使用 asyncio 检查多个网站的 HTTP 状态。
    
    在此示例中，我们将首先开发一个协程来检查给定 URL 的状态。 然后，我们将为前 10 个网站中的每个网站调用一次该协程。
    
    首先，我们可以定义一个协程，它将接受 URL 字符串并返回 HTTP 状态。
    
    ```python
    # 获取网页的 HTTP/S 状态
    async def get_status(url):
        # ...
    ```
    
    URL 必须被解析为其组成部分。
    
    发出 HTTP 请求时，我们需要主机名和文件路径。 我们还需要知道 URL 方案（HTTP 或 HTTPS），以便确定是否需要 SSL。
    
    这可以使用 [urllib.parse.urlsplit()](https://docs.python.org/3/library/urllib.parse.html) 函数来实现，该函数接受 URL 字符串并返回所有 URL 的命名元组。 网址元素。
    
    ```python
    ...
    # 将 url 拆分为多个部分
    url_parsed = urlsplit(url)
    ```
    
    然后我们可以根据 URL 方案打开 HTTP 连接并使用 URL 主机名。
    
    ```python
    ...
    # 打开连接
    if url_parsed.scheme == 'https':
        reader, writer = await asyncio.open_connection(url_parsed.hostname, 443, ssl=True)
    else:
        reader, writer = await asyncio.open_connection(url_parsed.hostname, 80)
    ```
    
    接下来，我们可以使用主机名和文件路径创建 HTTP GET 请求，并使用 **StreamWriter** 将编码字节写入套接字。
    
    ```python
    ...
    # send GET request
    query = f'GET {url_parsed.path} HTTP/1.1\r\nHost: {url_parsed.hostname}\r\n\r\n'
    # write query to socket
    writer.write(query.encode())
    # wait for the bytes to be written to the socket
    await writer.drain()
    ```
    
    接下来，我们可以读取 HTTP 响应。
    
    我们只需要包含 HTTP 状态的响应的第一行。
    
    ```python
    ...
    # 读取单行响应
    response = await reader.readline()
    ```
    
    然后可以关闭连接。
    
    ```python
    ...
    # 关闭连接
    writer.close()
    ```
    
    最后，我们可以解码从服务器读取的字节、远程尾随空格，并返回 HTTP 状态。
    
    ```python
    ...
    # 解码并去除空白
    status = response.decode().strip()
    # 返回响应
    return status
    ```
    
    将它们结合在一起，下面列出了完整的 **get_status()** 协程。
    
    它没有任何错误处理，例如无法到达主机或响应缓慢的情况。
    
    这些补充将为读者提供一个很好的扩展。
    
    ```python
    # 获取网页的 HTTP/S 状态
    async def get_status(url):
        # 将 url 拆分为多个组件
        url_parsed = urlsplit(url)
        # 打开连接
        if url_parsed.scheme == 'https':
            reader, writer = await asyncio.open_connection(url_parsed.hostname, 443, ssl=True)
        else:
            reader, writer = await asyncio.open_connection(url_parsed.hostname, 80)
        # 发送GET请求
        query = f'GET {url_parsed.path} HTTP/1.1\r\nHost: {url_parsed.hostname}\r\n\r\n'
        # 将查询写入套接字
        writer.write(query.encode())
        # 等待字节写入套接字
        await writer.drain()
        # 读取单行响应
        response = await reader.readline()
        # 关闭连接
        writer.close()
        # 解码并去除空白
        status = response.decode().strip()
        # 返回响应
        return status
    ```
    
    接下来，我们可以为我们想要检查的多个网页或网站调用 **get_status()** 协程。
    
    在本例中，我们将定义世界排名前 10 的网页列表。
    
    ```python
    ...
    # 要检查的前 10 个网站列表
    sites = ['https://www.google.com/',
        'https://www.youtube.com/',
        'https://www.facebook.com/',
        'https://twitter.com/',
        'https://www.instagram.com/',
        'https://www.baidu.com/',
        'https://www.wikipedia.org/',
        'https://yandex.ru/',
        'https://yahoo.com/',
        'https://www.whatsapp.com/'
        ]
    ```
    
    然后我们可以使用 get_status() 协程依次查询每个。
    
    在这种情况下，我们将在循环中按顺序执行此操作，并依次报告每个状态。
    
    ```python
    ...
    # 检查所有网站的状态
    for url in sites:
        # 获取 url 的状态
        status = await get_status(url)
        # 报告 url 及其状态
        print(f'{url:30}:\t{status}')
    ```
    
    使用 asyncio 时，我们可以比顺序做得更好，但这提供了一个很好的起点，我们可以在以后进行改进。
    
    将它们结合在一起，**main()** 协程查询前 10 个网站的状态。
    
    ```python
    # 主协程
    async def main():
        # 要检查的前 10 个网站列表
        sites = ['https://www.google.com/',
            'https://www.youtube.com/',
            'https://www.facebook.com/',
            'https://twitter.com/',
            'https://www.instagram.com/',
            'https://www.baidu.com/',
            'https://www.wikipedia.org/',
            'https://yandex.ru/',
            'https://yahoo.com/',
            'https://www.whatsapp.com/'
            ]
        # 检查所有网站的状态
        for url in sites:
            # 获取 url 的状态
            status = await get_status(url)
            # 报告 url 及其状态
            print(f'{url:30}:\t{status}')
    ```
    
    最后，我们可以创建 **main()** 协程并将其用作 **asyncio** 程序的入口点。
    
    ```python
    ...
    # 运行异步程序
    asyncio.run(main())
    ```
    
    将它们结合在一起，下面列出了完整的示例。
    
    ```python
    # SuperFastPython.com
    # 检查许多网页的状态
    import asyncio
    from urllib.parse import urlsplit
     
    # 获取网页的 HTTP/S 状态
    async def get_status(url):
        # 将 url 拆分为多个组件
        url_parsed = urlsplit(url)
        # 打开连接
        if url_parsed.scheme == 'https':
            reader, writer = await asyncio.open_connection(url_parsed.hostname, 443, ssl=True)
        else:
            reader, writer = await asyncio.open_connection(url_parsed.hostname, 80)
        # 发送 GET 请求
        query = f'GET {url_parsed.path} HTTP/1.1\r\nHost: {url_parsed.hostname}\r\n\r\n'
        # 将查询写入套接字
        writer.write(query.encode())
        # 等待字节写入套接字
        await writer.drain()
        # 读取单行响应
        response = await reader.readline()
        # 关闭连接
        writer.close()
        # 解码并去除空白
        status = response.decode().strip()
        # 返回响应
        return status
     
    # 主协程
    async def main():
        # 要检查的前 10 个网站列表
        sites = ['https://www.google.com/',
            'https://www.youtube.com/',
            'https://www.facebook.com/',
            'https://twitter.com/',
            'https://www.instagram.com/',
            'https://www.baidu.com/',
            'https://www.wikipedia.org/',
            'https://yandex.ru/',
            'https://yahoo.com/',
            'https://www.whatsapp.com/'
            ]
        # 检查所有网站的状态
        for url in sites:
            # 获取 url 的状态
            status = await get_status(url)
            # 报告 url 及其状态
            print(f'{url:30}:\t{status}')
     
    # 运行异步程序
    asyncio.run(main())
    ```
    
    运行该示例首先创建 **main()** 协程并将其用作程序的入口点。
    
    **main()** 协程运行，定义前 10 个网站的列表。
    
    然后按顺序遍历网站列表。 **main()** 协程挂起并调用 **get_status()** 协程来查询某个网站的状态。
    
    **get_status()** 协程运行、解析 URL 并打开连接。 它构造一个 HTTP GET 查询并将其写入主机。 响应被读取、解码并返回。
    
    **main()** 协程恢复并报告 URL 的 HTTP 状态。
    
    对列表中的每个 URL 重复此操作。
    
    该程序大约需要 5.6 秒才能完成，或者平均每个 URL 大约需要半秒。
    
    这突出显示了我们如何使用 asyncio 来查询网页的 HTTP 状态。
    
    尽管如此，它并没有充分利用 asyncio 来并发执行任务。
    
    ```text
    https://www.google.com/       : HTTP/1.1 200 OK
    https://www.youtube.com/      : HTTP/1.1 200 OK
    https://www.facebook.com/     : HTTP/1.1 302 Found
    https://twitter.com/          : HTTP/1.1 200 OK
    https://www.instagram.com/    : HTTP/1.1 200 OK
    https://www.baidu.com/        : HTTP/1.1 200 OK
    https://www.wikipedia.org/    : HTTP/1.1 200 OK
    https://yandex.ru/            : HTTP/1.1 302 Moved temporarily
    https://yahoo.com/            : HTTP/1.1 301 Moved Permanently
    https://www.whatsapp.com/     : HTTP/1.1 302 Found
    ```
    
    接下来，让我们看看如何更新示例以同时执行协程。

### 21.7 Example of Checking Website Status Concurrently

=== "English"

    A benefit of asyncio is that we can execute many coroutines concurrently.

    We can query the status of websites concurrently in asyncio using the [asyncio.gather()](https://docs.python.org/3/library/asyncio-task.html#asyncio.gather) function.

    This function takes one or more coroutines, suspends executing the provided coroutines, and returns the results from each as an iterable. We can then traverse the list of URLs and iterable of return values from the coroutines and report results.

    This may be a simpler approach than the above.

    First, we can create a list of coroutines.

    ```python
    ...
    # create all coroutine requests
    coros = [get_status(url) for url in sites]
    ```

    Next, we can execute the coroutines and get the iterable of results using asyncio.gather().

    Note that we cannot provide the list of coroutines directly, but instead must unpack the list into separate expressions that are provided as positional arguments to the function.

    ```python
    ...
    # execute all coroutines and wait
    results = await asyncio.gather(*coros)
    ```

    This will execute all of the coroutines concurrently and retrieve their results.

    We can then traverse the list of URLs and returned status and report each in turn.

    ```python
    ...
    # process all results
    for url, status in zip(sites, results):
        # report status
        print(f'{url:30}:\t{status}')
    ```

    Tying this together, the complete example is listed below.

    ```python
    # SuperFastPython.com
    # check the status of many webpages
    import asyncio
    from urllib.parse import urlsplit
    
    # get the HTTP/S status of a webpage
    async def get_status(url):
        # split the url into components
        url_parsed = urlsplit(url)
        # open the connection
        if url_parsed.scheme == 'https':
            reader, writer = await asyncio.open_connection(url_parsed.hostname, 443, ssl=True)
        else:
            reader, writer = await asyncio.open_connection(url_parsed.hostname, 80)
        # send GET request
        query = f'GET {url_parsed.path} HTTP/1.1\r\nHost: {url_parsed.hostname}\r\n\r\n'
        # write query to socket
        writer.write(query.encode())
        # wait for the bytes to be written to the socket
        await writer.drain()
        # read the single line response
        response = await reader.readline()
        # close the connection
        writer.close()
        # decode and strip white space
        status = response.decode().strip()
        # return the response
        return status
    
    # main coroutine
    async def main():
        # list of top 10 websites to check
        sites = ['https://www.google.com/',
            'https://www.youtube.com/',
            'https://www.facebook.com/',
            'https://twitter.com/',
            'https://www.instagram.com/',
            'https://www.baidu.com/',
            'https://www.wikipedia.org/',
            'https://yandex.ru/',
            'https://yahoo.com/',
            'https://www.whatsapp.com/'
            ]
        # create all coroutine requests
        coros = [get_status(url) for url in sites]
        # execute all coroutines and wait
        results = await asyncio.gather(*coros)
        # process all results
        for url, status in zip(sites, results):
            # report status
            print(f'{url:30}:\t{status}')
    
    # run the asyncio program
    asyncio.run(main())
    ```

    Running the example executes the main() coroutine as before.

    In this case, a list of coroutines is created in a list comprehension.

    The asyncio.gather() function is then called, passing the coroutines and suspending the main() coroutine until they are all complete.

    The coroutines execute, querying each website concurrently and returning their status.

    The main() coroutine resumes and receives an iterable of status values. This iterable along with the list of URLs is then traversed using the zip() built-in function and the statuses are reported.

    This highlights a simpler approach to executing the coroutines concurrently and reporting the results after all tasks are completed.

    It is also faster than the sequential version above, completing in about 1.4 seconds on my system.

    ```text
    https://www.google.com/       : HTTP/1.1 200 OK
    https://www.youtube.com/      : HTTP/1.1 200 OK
    https://www.facebook.com/     : HTTP/1.1 302 Found
    https://twitter.com/          : HTTP/1.1 200 OK
    https://www.instagram.com/    : HTTP/1.1 200 OK
    https://www.baidu.com/        : HTTP/1.1 200 OK
    https://www.wikipedia.org/    : HTTP/1.1 200 OK
    https://yandex.ru/            : HTTP/1.1 302 Moved temporarily
    https://yahoo.com/            : HTTP/1.1 301 Moved Permanently
    https://www.whatsapp.com/     : HTTP/1.1 302 Found
    ```

    Next, let’s explore common errors when getting started with asyncio.

=== "Chinese"

    asyncio 的一个好处是我们可以同时执行许多协程。

    我们可以使用 [asyncio.gather()](https://docs.python.org/3/library/asyncio-task.html#asyncio.gather) 函数在 asyncio 中同时查询网站的状态。

    该函数采用一个或多个协程，暂停执行所提供的协程，并将每个协程的结果作为可迭代对象返回。 然后我们可以遍历协程的 URL 列表和可迭代的返回值并报告结果。

    这可能是比上面更简单的方法。

    首先，我们可以创建一个协程列表。

    ```python
    ...
    # 创建所有协程请求
    coros = [get_status(url) for url in sites]
    ```

    接下来，我们可以执行协程并使用 **asyncio.gather()** 获取可迭代的结果。

    请注意，我们无法直接提供协程列表，而是必须将列表解压缩为单独的表达式，这些表达式作为函数的位置参数提供。

    ```python
    ...
    # 执行所有协程并等待
    results = await asyncio.gather(*coros)
    ```

    这将同时执行所有协程并检索它们的结果。

    然后我们可以遍历 URL 列表和返回的状态并依次报告。

    ```python
    ...
    # 处理所有结果
    for url, status in zip(sites, results):
        # 报告状态
        print(f'{url:30}:\t{status}')
    ```

    将它们结合在一起，下面列出了完整的示例。

    ```python
    # SuperFastPython.com
    # 检查许多网页的状态
    import asyncio
    from urllib.parse import urlsplit
    
    # 获取网页的 HTTP/S 状态
    async def get_status(url):
        # 将 url 拆分为多个组件
        url_parsed = urlsplit(url)
        # 打开连接
        if url_parsed.scheme == 'https':
            reader, writer = await asyncio.open_connection(url_parsed.hostname, 443, ssl=True)
        else:
            reader, writer = await asyncio.open_connection(url_parsed.hostname, 80)
        # 发送 GET 请求
        query = f'GET {url_parsed.path} HTTP/1.1\r\nHost: {url_parsed.hostname}\r\n\r\n'
        # 将查询写入套接字
        writer.write(query.encode())
        # 等待字节写入套接字
        await writer.drain()
        # 读取单行响应
        response = await reader.readline()
        # 关闭连接
        writer.close()
        # 解码并去除空白字符
        status = response.decode().strip()
        # 返回响应
        return status
    
    # 主协程
    async def main():
        # 要检查的前 10 个网站列表
        sites = ['https://www.google.com/',
            'https://www.youtube.com/',
            'https://www.facebook.com/',
            'https://twitter.com/',
            'https://www.instagram.com/',
            'https://www.baidu.com/',
            'https://www.wikipedia.org/',
            'https://yandex.ru/',
            'https://yahoo.com/',
            'https://www.whatsapp.com/'
            ]
        # 创建所有协程请求
        coros = [get_status(url) for url in sites]
        # 执行所有协程并等待
        results = await asyncio.gather(*coros)
        # 处理所有结果
        for url, status in zip(sites, results):
            # 报告状态
            print(f'{url:30}:\t{status}')
    
    # 运行异步程序
    asyncio.run(main())
    ```

    运行该示例会像以前一样执行 **main()** 协程。

    在这种情况下，协程列表是在列表推导式中创建的。

    然后调用 **asyncio.gather()** 函数，传递协程并挂起 **main()** 协程，直到它们全部完成。

    协程执行，同时查询每个网站并返回其状态。

    **main()** 协程恢复并接收可迭代的状态值。 然后使用 **zip()** 内置函数遍历该可迭代对象以及 URL 列表，并报告状态。

    这突出显示了一种**更简单的方法**来**同时执行协程**并在所有任务完成后报告结果。

    它也比上面的顺序版本更快，在我的系统上大约需要 1.4 秒即可完成。

    ```text
    https://www.google.com/       : HTTP/1.1 200 OK
    https://www.youtube.com/      : HTTP/1.1 200 OK
    https://www.facebook.com/     : HTTP/1.1 302 Found
    https://twitter.com/          : HTTP/1.1 200 OK
    https://www.instagram.com/    : HTTP/1.1 200 OK
    https://www.baidu.com/        : HTTP/1.1 200 OK
    https://www.wikipedia.org/    : HTTP/1.1 200 OK
    https://yandex.ru/            : HTTP/1.1 302 Moved temporarily
    https://yahoo.com/            : HTTP/1.1 301 Moved Permanently
    https://www.whatsapp.com/     : HTTP/1.1 302 Found
    ```

    接下来，让我们探讨一下 **asyncio** 入门时的常见错误。

## 22. Python Asyncio Common Errors

=== "English"

    This section gives examples of general errors encountered by developers when using asyncio in Python.

    The 5 most common asyncio errors are:

    1. Trying to run coroutines by calling them.
    2. Not letting coroutines run in the event loop.
    3. Using the asyncio low-level API.
    4. Exiting the main coroutine too early.
    5. Assuming race conditions and deadlocks are not possible.
    
    Let’s take a closer look at each in turn.

=== "Chinese"

    本节提供了开发人员在 Python 中使用 asyncio 时遇到的常见错误的示例。

    5 个最常见的异步错误是：

    1. 尝试通过调用协程来运行它们。
    2. 不让协程在事件循环中运行。
    3. 使用 asyncio 低级 API。
    4. 过早退出主协程。
    5. 假设竞争条件和死锁是不存在的。
    
    让我们依次仔细看看每一个问题。

### 22.1 Error 1: Trying to Run Coroutines by Calling Them

=== "English"

    The most common error encountered by beginners to asyncio is calling a coroutine like a function.

    For example, we can define a coroutine using the “async def” expression:

    ```python
    # custom coroutine
    async def custom_coro():
        print('hi there')
    ```

    The beginner will then attempt to call this coroutine like a function and expect the print message to be reported.

    For example:

    ```python
    ...
    # error attempt at calling a coroutine like a function
    custom_coro()
    ```
    
    Calling a coroutine like a function will not execute the body of the coroutine.

    Instead, it will create a coroutine object.

    This object can then be awaited within the asyncio runtime, e.g. the event loop.

    We can start the event loop to run the coroutine using the asyncio.run() function.

    For example:

    ```python
    ...
    # run a coroutine
    asyncio.run(custom_coro())
    ```

    Alternatively, we can suspend the current coroutine and schedule the other coroutine using the “await” expression.

    For example:

    ```python
    ...
    # schedule a coroutine
    await custom_coro()
    ```

    You can learn more about running coroutines in the tutorial:
    
    - [How to Run an Asyncio Coroutine in Python](https://superfastpython.com/asyncio-run-coroutine)

=== "Chinese"

    asyncio 初学者遇到的最常见错误是像函数一样调用协程。

    例如，我们可以使用“async def”表达式定义一个协程：

    ```python
    # 自定义协程
    async def custom_coro():
        print('hi there')
    ```

    然后，初学者将尝试像函数一样调用这个协程，并期望报告打印消息。

    例如：

    ```python
    ...
    # 尝试像函数一样调用协程时出错
    custom_coro()
    ```
    
    像函数一样调用协程不会执行协程主体。

    相反，它将创建一个协程对象。

    然后可以在 asyncio 运行时中等待该对象，例如：事件循环（Event Loop）。

    我们可以使用 `asyncio.run()` 函数启动事件循环来运行协程。

    例如：

    ```python
    ...
    # 运行协程
    asyncio.run(custom_coro())
    ```

    或者，我们可以挂起当前协程并使用“**await**”表达式调度另一个协程。

    例如：

    ```python
    ...
    # 调度一个协程
    await custom_coro()
    ```

    您可以在教程中了解有关运行协程的更多信息：
    
    - [如何在 Python 中运行 Asyncio 协程](https://superfastpython.com/asyncio-run-coroutine)

### 22.2 Error 2: Not Letting Coroutines Run in the Event Loop

=== "English"

    If a coroutine is not run, you will get a runtime warning as follows:

    ```text
    sys:1: RuntimeWarning: coroutine 'custom_coro' was never awaited
    ```

    This will happen if you create a coroutine object but do not schedule it for execution within the asyncio event loop.

    For example, you may attempt to call a coroutine from a regular Python program:

    ```python
    ...
    # attempt to call the coroutine
    custom_coro()
    ```

    This will not call the coroutine.

    Instead, it will create a coroutine object.

    For example:

    ```python
    ...
    # create a coroutine object
    coro = custom_coro()
    ```

    If you do not allow this coroutine to run, you will get a runtime error.

    You can let the coroutine run, as we saw in the previous section, by starting the asyncio event loop and passing it the coroutine object.

    For example:

    ```python
    ...
    # create a coroutine object
    coro = custom_coro()
    # run a coroutine
    asyncio.run(coro)
    ```

    Or, on one line in a compound statement:

    ```python
    ...
    # run a coroutine
    asyncio.run(custom_coro())
    ```

    You can learn more about running coroutines in the tutorial:

    - [How to Run an Asyncio Coroutine in Python](https://superfastpython.com/asyncio-run-coroutine)

    If you get this error within an asyncio program, it is because you have created a coroutine and have not scheduled it for execution.

    This can be achieved using the await expression.

    For example:

    ```python
    ...
    # create a coroutine object
    coro = custom_coro()
    # suspend and allow the other coroutine to run
    await coro
    ```

    Or, you can schedule it to run independently as a task.

    For example:

    ```python
    ...
    # create a coroutine object
    coro = custom_coro()
    # schedule the coro to run as a task interdependently
    task = asyncio.create_task(coro)
    ```

    You can learn more about creating tasks in the tutorial:

    - How to Create an Asyncio Task in Python

=== "Chinese"

    如果协程未运行，您将收到如下运行时警告：

    ```text
    sys:1: RuntimeWarning: coroutine 'custom_coro' was never awaited
    ```

    如果您创建一个协程对象但没有安排它在 asyncio 事件循环中执行，就会发生这种情况。

    例如，您可以尝试从常规 Python 程序调用协程：

    ```python
    ...
    # 尝试调用协程
    custom_coro()
    ```

    这不会调用协程。

    相反，它将创建一个协程对象。

    例如：

    ```python
    ...
    # 创建一个协程对象
    coro = custom_coro()
    ```

    如果您不允许该协程运行，您将收到运行时错误。

    正如我们在上一节中看到的，您可以通过启动 asyncio 事件循环并向其传递协程对象来让协程运行。

    例如：

    ```python
    ...
    # 创建一个协程对象
    coro = custom_coro()
    # 运行协程
    asyncio.run(coro)
    ```

    或者，在复合语句的一行中：

    ```python
    ...
    # 运行协程
    asyncio.run(custom_coro())
    ```

    您可以在教程中了解有关运行协程的更多信息：

    - [如何在 Python 中运行 Asyncio 协程](https://superfastpython.com/asyncio-run-coroutine)

    如果您在 asyncio 程序中收到此错误，那是因为您创建了一个协程但尚未安排其执行。

    这可以使用 **await** 表达式来实现。

    例如：

    ```python
    ...
    # 创建一个协程对象
    coro = custom_coro()
    # 挂起并允许其他协程运行
    await coro
    ```

    或者，您可以安排它作为任务独立运行。

    例如：

    ```python
    ...
    # 创建一个协程对象
    coro = custom_coro()
    # 安排 coro 作为任务相互依赖地运行
    task = asyncio.create_task(coro)
    ```

    您可以在教程中了解有关创建任务的更多信息：

    - [如何在 Python 中创建Asyncio任务](https://superfastpython.com/asyncio-create-task)

### 22.3 Error 3: Using the Low-Level Asyncio API

=== "English"

    A big problem with beginners is that they use the wrong asyncio API.

    This is common for a number of reasons.

    - The API has changed a lot with recent versions of Python.
    - The API docs page makes things confusing, showing both APIs.
    - Examples elsewhere on the web mix up using the different APIs.
    
    Using the wrong API makes things more verbose (e.g. more code), more difficult, and way less understandable.

    Asyncio offers [two APIs](https://docs.python.org/3/library/asyncio.html).

    1. High-level API for application developers (us)
    2. Low-level API for framework and library developers (not us)
    
    The lower-level API provides the foundation for the high-level API and includes the internals of the event loop, transport protocols, policies, and more.

    > … there are low-level APIs for library and framework developers
    >
    > — [ASYNCIO — ASYNCHRONOUS I/O](https://docs.python.org/3/library/asyncio.html)
    We should almost always stick to the high-level API.

    We absolutely must stick to the high-level API when getting started.

    We may dip into the low-level API to achieve specific outcomes on occasion.

    If you start getting a handle on the event loop or use a “loop” variable to do things, you are doing it wrong.

    I am not saying don’t learn the low-level API.

    Go for it. It’s great.

    Just don’t start there.

    Drive asyncio via the high-level API for a while. Develop some programs. Get comfortable with asynchronous programming and running coroutines at will.

    Then later, dip in and have a look around.

=== "Chinese"

    初学者的一个大问题是他们使用了错误的 asyncio API。

    由于多种原因，这种情况很常见。

    - API 在最新版本的 Python 中发生了很大变化。
    - API 文档页面显示了这两个 API，这让事情变得混乱。
    - 网络上其他地方的示例混合使用不同的 API。
    
    使用错误的 API 会使事情变得更加冗长（例如更多代码）、更加困难并且更难以理解。

    Asyncio 提供[两个 API](https://docs.python.org/3/library/asyncio.html).

    1. 面向应用程序开发人员（我们）的高级 API
    2. 面向框架和库开发人员（不是我们）的低级 API
    
    较低级别的 API 为高级 API 提供基础，包括事件循环、传输协议、策略等的内部结构。

    > … 有供库和框架开发人员使用的低级 API
    >
    > — [ASYNCIO — ASYNCHRONOUS I/O](https://docs.python.org/3/library/asyncio.html)
    
    我们几乎应该始终坚持使用高级 API。

    开始时我们绝对必须坚持使用高级 API。

    有时我们可能会利用低级 API 来实现特定的结果。

    如果您开始获取事件循环的句柄或使用“loop”变量来执行操作，那么您就做错了。

    我并不是说不要学习低级 API。

    大胆试试吧。 这很棒。

    只是不要从那里开始。

    通过高级 API 驱动 asyncio 一段时间。 开发一些程序。 熟悉异步编程并随意运行协程。

    然后，深入看看相关技术细节。

### 22.4 Error 4: Exiting the Main Coroutine Too Early

=== "English"

    A major point of confusion in asyncio programs is not giving tasks enough time to complete.

    We can schedule many coroutines to run independently within an asyncio program via the **asyncio.create_task()** method.

    The main coroutine, the entry point for the asyncio program, can then carry on with other activities.

    If the main coroutine exits, then the asyncio program will terminate.

    The program will terminate even if there are one or many coroutines running independently as tasks.

    This can catch you off guard.

    You may issue many tasks and then allow the main coroutine to resume, expecting all issued tasks to complete in their own time.

    Instead, if the main coroutine has nothing else to do, it should wait on the remaining tasks.

    This can be achieved by first getting a set of all running tasks via the **asyncio.all_tasks()** function, removing itself from this set, then waiting on the remaining tasks via the **asyncio.wait()** function.

    For example:

    ```python
    ...
    # get a set of all running tasks
    all_tasks = asyncio.all_tasks()
    # get the current tasks
    current_task = asyncio.current_task()
    # remove the current task from the list of all tasks
    all_tasks.remove(current_task)
    # suspend until all tasks are completed
    await asyncio.wait(all_tasks)
    ```

=== "Chinese"

    异步程序中的一个主要混乱点是没有给任务足够的时间来完成。

    我们可以通过 **asyncio.create_task()** 方法安排许多协程在 asyncio 程序中独立运行。

    主协程（asyncio 程序的入口点）可以继续执行其他活动。

    如果主协程退出，则 asyncio 程序将终止。

    即使有一个或多个协程作为任务独立运行，程序也会终止。

    这可能会让你措手不及。

    您可以发出许多任务，然后允许主协程恢复，并期望所有发出的任务都能在自己的时间内完成。

    相反，如果主协程没有其他事情可做，它应该等待剩余的任务。

    这可以通过首先通过 **asyncio.all_tasks()** 函数获取一组所有正在运行的任务，将其自身从该组中删除，然后通过 **asyncio.wait()** 函数等待剩余任务来实现。

    例如:

    ```python
    ...
    # 获取所有正在运行的任务的集合
    all_tasks = asyncio.all_tasks()
    # 获取当前任务
    current_task = asyncio.current_task()
    # 从所有任务列表中删除当前任务
    all_tasks.remove(current_task)
    # 暂停直到所有任务完成
    await asyncio.wait(all_tasks)
    ```

### 22.5 Error 5: Assuming Race Conditions and Deadlocks are Impossible

=== "English"

    Concurrent programming has the hazard of concurrency-specific failure modes.

    This includes problems such as race conditions and deadlocks.

    A race condition involves two or more units of concurrency executing the same critical section at the same time and leaving a resource or data in an inconsistent or unexpected state. This can lead to data corruption and data loss.

    A deadlock is when a unit of concurrency waits for a condition that can never occur, such as for a resource to become available.

    Many Python developers believe these problems are not possible with coroutines in asyncio.

    The reason being that only one coroutine can run within the event loop at any one time.

    It is true that only one coroutine can run at a time.

    The problem is, coroutines can suspend and resume and may do so while using a shared resource or shared variable.

    Without protecting critical sections, race conditions can occur in asyncio programs.

    Without careful management of synchronization primitives, deadlocks can occur

    As such, it is important that asyncio programs are created ensuring coroutine-safety, a concept similar to thread-safety and process-safety, applied to coroutines.

=== "Chinese"

    并发编程存在并发特定故障模式的危险。

    这包括竞争条件和死锁等问题。

    竞争条件涉及两个或多个并发单元同时执行同一关键部分，并使资源或数据处于不一致或意外状态。 这可能会导致数据损坏和数据丢失。

    死锁是指并发单元等待永远不会发生的条件，例如资源可用。

    许多 Python 开发人员认为 **asyncio** 中的协程不可能出现这些问题。

    原因是任一时间只有一个协程可以在事件循环内运行。

    确实，一次只能运行一个协程。

    问题是，协程可以挂起和恢复，并且可以在使用共享资源或共享变量时执行此操作。

    如果不保护关键部分，异步程序中可能会出现竞争条件。

    如果不仔细管理同步原语，可能会发生死锁

    因此，创建 **asyncio** 程序以确保协程安全（类似于线程安全和进程安全的概念）非常重要，适用于协程。

## 23. Python Asyncio Common Questions

=== "English"

    This section answers common questions asked by developers when using asyncio in Python.

    **Do you have a question about asyncio?**

    Ask your question in the comments below and I will do my best to answer it and perhaps add it to this list of questions.

=== "Chinese"

    本节回答开发人员在 Python 中使用 **asyncio** 时提出的常见问题。

    **您对 asyncio 有疑问吗?**

    在下面的评论中提出您的问题，我会尽力回答它，也许会将其添加到这个问题列表中。

### 23.1 How to Stop a Task?

=== "English"

    We can cancel a task via the **cancel()** method on an [asyncio.Task](https://docs.python.org/3/library/asyncio-task.html#asyncio.Task.cancel) object.

    The **cancel()** method returns True if the task was canceled, or False otherwise.

    For example:

    ```python
    ...
    # cancel the task
    was_cancelled = task.cancel()
    ```

    If the task is already done, it cannot be canceled and the cancel() method will return False and the task will not have the status of canceled.

    The next time the task is given an opportunity to run, it will raise a CancelledError exception.

    If the CancelledError exception is not handled within the wrapped coroutine, the task will be canceled.

    Otherwise, if the CancelledError exception is handled within the wrapped coroutine, the task will not be canceled.

    The cancel() method can also take a message argument which will be used in the content of the CancelledError.

    We can explore how to cancel a running task.

    In this example, we define a task coroutine that reports a message and then blocks for a moment.

    We then define the main coroutine that is used as the entry point into the asyncio program. It reports a message, creates and schedules the task, then waits a moment.

    The main coroutine then resumes and cancels the task while it is running. It waits a moment more to allow the task to respond to the request to cancel. The main coroutine then reports whether the request to cancel the task was successful.

    The task is canceled and is then done.

    The main coroutine then reports whether the status of the task is canceled before closing the program.

    The complete example is listed below.

    ```python
    # SuperFastPython.com
    # example of canceling a running task
    import asyncio
    
    # define a coroutine for a task
    async def task_coroutine():
        # report a message
        print('executing the task')
        # block for a moment
        await asyncio.sleep(1)
    
    # custom coroutine
    async def main():
        # report a message
        print('main coroutine started')
        # create and schedule the task
        task = asyncio.create_task(task_coroutine())
        # wait a moment
        await asyncio.sleep(0.1)
        # cancel the task
        was_cancelled = task.cancel()
        # report whether the cancel request was successful
        print(f'was canceled: {was_cancelled}')
        # wait a moment
        await asyncio.sleep(0.1)
        # check the status of the task
        print(f'canceled: {task.cancelled()}')
        # report a final message
        print('main coroutine done')
    
    # start the asyncio program
    asyncio.run(main())
    ```

    Running the example starts the asyncio event loop and executes the **main()** coroutine.

    The **main()** coroutine reports a message, then creates and schedules the task coroutine.

    It then suspends and awaits a moment to allow the task coroutine to begin running.

    The task runs, reports a message and sleeps for a while.

    The **main()** coroutine resumes and cancels the task. It reports that the request to cancel the task was successful.

    It then sleeps for a moment to allow the task to respond to the request to be canceled.

    The **task_coroutine()** resumes and a **CancelledError** exception is raised that causes the task to fail and be done.

    The **main()** coroutine resumes and reports whether the task has the status of canceled. In this case, it does.

    This example highlights the normal case of canceling a running task.

    ```text
    main coroutine started
    executing the task
    was canceled: True
    canceled: True
    main coroutine done
    ```

=== "Chinese"

    我们可以通过 [asyncio.Task](https://docs.python.org/3/library/asyncio-task.html#asyncio.Task.cancel) 对象上的 **cancel()** 方法取消任务。

    如果任务被取消，则 **cancel()** 方法返回 `True`，否则返回 `False`。

    例如:

    ```python
    ...
    # 取消任务
    was_cancelled = task.cancel()
    ```

    如果任务已经完成，则无法取消，**cancel()** 方法将返回 `False`，任务不会处于已取消状态。

    下次任务有机会运行时，它将引发 **CancelledError** 异常。

    如果未在包装的协程内处理 **CancelledError** 异常，则任务将被取消。

    否则，如果在包装的协程内处理 **CancelledError** 异常，则任务将不会被取消。

    **cancel()** 方法同样还可以传递一个消息参数，作为 **CancelledError** 异常的内容。

    我们可以探讨如何取消正在运行的任务。

    在这个例子中，我们定义了一个任务协程，它报告一条消息，然后阻塞一会儿。

    然后，我们定义用作 **asyncio** 程序入口点的主协程。 它报告一条消息，创建并安排任务，然后等待片刻。

    然后，主协程在任务运行时恢复并取消任务。 它会再等待一段时间，以便任务响应取消请求。 然后主协程报告取消任务的请求是否成功。

    任务被取消，然后完成。

    然后主协程在关闭程序之前报告任务的状态是否已取消。

    下面列出了完整的示例。

    ```python
    # SuperFastPython.com
    # 取消一个正在运行的任务的例子
    import asyncio
    
    # 为任务定义一个协程
    async def task_coroutine():
        # 报告一条消息
        print('executing the task')
        # 阻塞片刻
        await asyncio.sleep(1)
    
    # 自定义协程
    async def main():
        # 报告一条消息
        print('main coroutine started')
        # 创建并调度任务
        task = asyncio.create_task(task_coroutine())
        # 等待一下
        await asyncio.sleep(0.1)
        # 取消任务
        was_cancelled = task.cancel()
        # 报告取消请求是否成功
        print(f'was canceled: {was_cancelled}')
        # 等待一下
        await asyncio.sleep(0.1)
        # 检查任务的状态
        print(f'canceled: {task.cancelled()}')
        # 报告最后的消息
        print('main coroutine done')
    
    # 启动异步程序
    asyncio.run(main())
    ```

    运行该示例将启动 **asyncio** 事件循环并执行 **main()** 协程。

    **main()** 协程报告一条消息，然后创建并调度任务协程。

    然后它会挂起并等待一段时间以允许任务协程开始运行。

    该任务运行，报告消息并休眠一段时间。

    **main()** 协程恢复并取消任务。 它报告取消任务的请求已成功。

    然后它会休眠一会儿，以允许任务响应要取消的请求。

    **task_coroutine()** 恢复并引发 **CancelledError** 异常，导致任务失败并完成。

    **main()** 协程恢复并报告任务是否处于已取消状态。 在这种情况下，确实如此。

    此示例重点介绍了取消正在运行的任务的正常情况。

    ```text
    main coroutine started
    executing the task
    was canceled: True
    canceled: True
    main coroutine done
    ```

### 23.2 How to Wait for a Task To Finish?

=== "English"

    We can wait for a task to finish by awaiting the **asyncio.Task** object directly.

    For example:

    ```python
    ...
    # wait for the task to finish
    await task
    ```

    We may create and wait for the task in a single line.

    For example:

    ```python
    ...
    # create and wait for the task to finish
    await asyncio.create_task(custom_coro())
    ```

=== "Chinese"

    我们可以通过直接等待 **asyncio.Task** 对象来等待任务完成。

    例如:

    ```python
    ...
    # 等待任务完成
    await task
    ```

    我们可以在一行中创建并等待任务完成。

    例如:

    ```python
    ...
    # 创建并等待任务完成
    await asyncio.create_task(custom_coro())
    ```

### 23.3 How to Get a Return Value from a Task?

=== "English"

    We may need to return values from coroutines to the caller.
    
    We can retrieve a return value from a coroutine by awaiting it.
    
    It assumes that the other coroutine being awaited returns a value.
    
    For example:
    
    ```python
    # coroutine that returns a value
    async def other_coro():
        return 100
    ```
    
    Awaiting the other coroutine will suspend the calling coroutine and schedule the other coroutine for execution. Once the other coroutine has been completed, the calling coroutine will resume. The return value will be passed from the other coroutine to the caller.
    
    For example:
    
    ```python
    ...
    # execute coroutine and retrieve return value
    value = await other_coro()
    ```
    
    A coroutine can be wrapped in an **asyncio.Task** object.
    
    This is helpful for independently executing the coroutine without having the current coroutine await it.
    
    This can be achieved using the [asyncio.create_task()](https://docs.python.org/3/library/asyncio-task.html#asyncio.create_task) function.
    
    For example:
    
    ```python
    ...
    # wrap coroutine in a task and schedule it for execution
    task = asyncio.create_task(other_coro())
    ```
    
    You can learn more about how to create tasks in the tutorial:
    
    - [How to Create an Asyncio Task in Python](https://superfastpython.com/asyncio-create-task)
    
    There are two ways to retrieve the return value from an asyncio.Task, they are:
    
    1. Await the task.
    2. Call the result() method.
    
    We can await the task to retrieve the return value.
    
    If the task is scheduled or running, then the caller will suspend until the task is complete and the return value will be provided.
    
    If the task is completed, the return value will be provided immediately.
    
    For example:
    
    ```python
    ...
    # get the return value from a task
    value = await task
    ```
    
    Unlike a coroutine, we can await a task more than once without raising an error.
    
    For example:
    
    ```python
    ...
    # get the return value from a task
    value = await task
    # get the return value from a task
    value = await task
    ```
    
    We can also get the return value from the task by calling the [result()](https://docs.python.org/3/library/asyncio-task.html#asyncio.Task.result) method on the **asyncio.Task** object.

    For example:
    
    ```python
    ...
    # get the return value from a task
    value = task.result()
    ```
    
    This requires that the task is done. If not, an **InvalidStateError** exception will be raised.
    
    If the task was canceled a **CancelledError** exception will be raised.
    
    You can learn more about getting the result from tasks in the tutorial:
    
    - [How to Get Asyncio Task Results](https://superfastpython.com/asyncio-task-result)

=== "Chinese"

    我们可能需要将值从协程返回给调用者。
    
    我们可以通过等待协程来检索返回值。
    
    它假设正在等待的另一个协程返回一个值。
    
    例如:
    
    ```python
    # 有返回值的协程
    async def other_coro():
        return 100
    ```
    
    等待另一个协程将挂起**调用协程**并安排另一个协程执行。 一旦其他协程完成，调用协程将恢复。 返回值将从另一个协程传递给调用者。
    
    例如:
    
    ```python
    ...
    # 执行协程并获取返回值
    value = await other_coro()
    ```
    
    协程可以包装在 **asyncio.Task** 对象中。
    
    **这对于独立执行协程很有帮助，而无需当前协程等待它。**
    
    这可以使用 [asyncio.create_task()](https://docs.python.org/3/library/asyncio-task.html#asyncio.create_task) 函数来实现。
    
    例如:
    
    ```python
    ...
    # 将协程包装在任务中并安排其执行
    task = asyncio.create_task(other_coro())
    ```
    
    您可以在教程中了解有关如何创建任务的更多信息：
    
    - [在Python中如何创建一个Asyncio任务](https://superfastpython.com/asyncio-create-task)
    
    有两种方法可以从 **asyncio.Task** 中检索返回值，它们是：
    
    1. 等待任务.
    2. 调用 **result()** 方法.
    
    我们可以等待任务来检索返回值。
    
    如果任务已调度或正在运行，则调用者将挂起，直到任务完成并提供返回值。
    
    如果任务完成，将立即提供返回值。
    
    例如:
    
    ```python
    ...
    # 获取任务的返回值
    value = await task
    ```
    
    与协程不同，我们可以多次等待任务而不会引发错误。
    
    例如:
    
    ```python
    ...
    # 获取任务的返回值
    value = await task
    # 获取任务的返回值
    value = await task
    ```
    
    我们还可以通过调用 **asyncio.Task** 对象上的 [result()](https://docs.python.org/3/library/asyncio-task.html#asyncio.Task.result) 方法来获取任务的返回值。

    例如:
    
    ```python
    ...
    # 获取任务的返回值
    value = task.result()
    ```
    
    这就要求任务完成。 如果不是，将引发 **InvalidStateError** 异常。
    
    如果任务被取消，则会引发 **CancelledError** 异常。
    
    您可以了解有关从教程中的任务获取结果的更多信息：
    
    - [如何获取 Asyncio 任务结果](https://superfastpython.com/asyncio-task-result)

### 23.4 How to Run a Task in the Background?

=== "English"

    We can run a coroutine in the background by wrapping it in an **asyncio.Task** object.
    
    This can be achieved by calling the **asyncio.create_task()** function and passing it the coroutine.
    
    The coroutine will be wrapped in a Task object and will be scheduled for execution. The task object will be returned and the caller will not suspend.
    
    For example:
    
    ```python
    ...
    # schedule the task for execution
    task = asyncio.create_task(other_coroutine())
    ```
    
    The task will not begin executing until at least the current coroutine is suspended, for any reason.
    
    We can help things along by suspending for a moment to allow the task to start running.
    
    This can be achieved by sleeping for zero seconds.
    
    For example:
    
    ```python
    ...
    # suspend for a moment to allow the task to start running
    await asyncio.sleep(0)
    ```
    
    This will suspend the caller only for a brief moment and allow the ask an opportunity to run.
    
    This is not required as the caller may suspend at some future time or terminate as part of normal execution.
    
    We may also await the task directly once the caller has run out of things to do.
    
    For example:
    
    ```python
    ...
    # wait for the task to complete
    await task
    ```

=== "Chinese"

    我们可以通过将协程包装在 **asyncio.Task** 对象中来在后台运行协程。
    
    这可以通过调用 **asyncio.create_task()** 函数并向其传递协程来实现。
    
    协程将被包装在 Task 对象中并被安排执行。 任务对象将被返回，并且调用者不会挂起。
    
    例如:
    
    ```python
    ...
    # 调度任务执行
    task = asyncio.create_task(other_coroutine())
    ```
    
    至少在当前协程出于任何原因被挂起之前，该任务不会开始执行。
    
    我们可以通过暂停片刻以允许任务开始运行来帮助完成任务。
    
    这可以通过休眠零秒来实现。
    
    例如:
    
    ```python
    ...
    # 暂停片刻以允许任务开始运行
    await asyncio.sleep(0)
    ```
    
    这只会将调用者暂停一小会儿，并允许有机会运行。
    
    这不是必需的，因为调用者可能会在将来的某个时间挂起或作为正常执行的一部分终止。
    
    一旦调用者没有事情可做，我们也可以直接等待任务。
    
    例如:
    
    ```python
    ...
    # 等待任务完成
    await task
    ```

### 23.5 How to Wait for All Background Tasks?

=== "English"

    We can wait for all independent tasks in an asyncio program.
    
    This can be achieved by first getting a set of all currently running tasks via the **asyncio.all_tasks()** function.
    
    For example:
    
    ```python
    ...
    # get a set of all running tasks
    all_tasks = asyncio.all_tasks()
    ```
    
    This will return a set that contains one **asyncio.Task** object for each task that is currently running, including the **main()** coroutine.
    
    We cannot wait on this set directly, as it will block forever as it includes the task that is the current task.
    
    Therefore we can get the **asyncio.Task** object for the currently running task and remove it from the set.
    
    This can be achieved by first calling the **asyncio.current_task()** method to get the task for the current coroutine and then remove it from the set via the **remove()** method.
    
    For example:
    
    ```python
    ...
    # get the current tasks
    current_task = asyncio.current_task()
    # remove the current task from the list of all tasks
    all_tasks.remove(current_task)
    ```
    
    Finally, we can wait on the set of remaining tasks.
    
    This will suspend the caller until all tasks in the set are complete.
    
    For example:
    
    ```python
    ...
    # suspend until all tasks are completed
    await asyncio.wait(all_tasks)
    ```
    
    Tying this together, the snippet below added to the end of the **main()** coroutine will wait for all background tasks to complete.
    
    ```python
    ...
    # get a set of all running tasks
    all_tasks = asyncio.all_tasks()
    # get the current tasks
    current_task = asyncio.current_task()
    # remove the current task from the list of all tasks
    all_tasks.remove(current_task)
    # suspend until all tasks are completed
    await asyncio.wait(all_tasks)
    ```

=== "Chinese"

    我们可以等待 asyncio 程序中的所有独立任务。
    
    首先可以通过 **asyncio.all_tasks()** 函数获取一组所有当前正在运行的任务来实现。
    
    例如:
    
    ```python
    ...
    # 获取所有正在运行的任务的集合
    all_tasks = asyncio.all_tasks()
    ```
    
    这将返回一个集合，其中包含当前正在运行的每个任务的一个 **asyncio.Task** 对象，包括 **main()** 协程。
    
    我们不能直接等待这个集合，因为它会永远阻塞，因为它包含当前任务的任务。
    
    因此，我们可以获取当前正在运行的任务的 **asyncio.Task** 对象并将其从集合中删除。
    
    这首先可以通过调用 **asyncio.current_task()** 方法来获取当前协程的任务，然后通过 **remove()** 方法将其从集合中删除来实现。
    
    例如:
    
    ```python
    ...
    # 获取当前任务
    current_task = asyncio.current_task()
    # 从所有任务列表中删除当前任务
    all_tasks.remove(current_task)
    ```
    
    最后，我们可以等待剩余的任务集。
    
    这将挂起调用者，直到该组中的所有任务完成。
    
    例如:
    
    ```python
    ...
    # 挂起直到所有任务完成
    await asyncio.wait(all_tasks)
    ```
    
    将它们结合在一起，添加到 **main()** 协程末尾的下面的代码片段将等待所有后台任务完成。
    
    ```python
    ...
    # 获取所有正在运行的任务的集合
    all_tasks = asyncio.all_tasks()
    # 获取当前任务
    current_task = asyncio.current_task()
    # 从所有任务列表中删除当前任务
    all_tasks.remove(current_task)
    # 挂起直到所有任务完成
    await asyncio.wait(all_tasks)
    ```

### 23.6 Does a Running Task Stop the Event Loop from Exiting?

=== "English"

    No.
    
    A task that is scheduled and run independently will not stop the event loop from exiting.
    
    If your main coroutine has no other activities to complete and there are independent tasks running in the background, you should retrieve the running tasks and wait on them
    
    The previous question/answer shows exactly how to do this.

=== "Chinese"

    不。
    
    独立调度和运行的任务不会阻止事件循环退出。
    
    如果您的主协程没有其他活动需要完成，并且有独立任务在后台运行，您应该检索正在运行的任务并等待它们
    
    上一个问题/答案准确地展示了如何做到这一点。

### 23.7 How to Show Progress of Running Tasks?

=== "English"

    We can show progress using a done callback function on each task.
    
    A done callback is a function that we can register on an **asyncio.Task**.
    
    It is called once the task is done, either normally or if it fails.
    
    The done callback function is a regular function, not a coroutine, and takes the **asyncio.Task** that it is associated with as an argument.
    
    We can use the same callback function for all tasks and report progress in a general way, such as by reporting a message.
    
    For example:
    
    ```python
    # callback function to show progress of tasks
    def progress(task):
        # report progress of the task
        print('.', end='')
    ```
    
    We can register a callback function on each **asyncio.Task** that we issue.
    
    This can be achieved using the [add_done_callback()](https://docs.python.org/3/library/asyncio-task.html#asyncio.Task.add_done_callback) method on each task and passing it the name of the callback function.
    
    For example:
    
    ```python
    ...
    # add a done callback to a task
    task.add_done_callback(progress)
    ```

=== "Chinese"

    我们可以使用每个任务的回调函数来显示进度。

    执行完成后的回调函数是我们可以在 **asyncio.Task** 上注册的函数。
    
    一旦任务执行完，无论正常还是失败，都会调用它。
    
    done 回调函数是一个常规函数，而不是协程，并且将与其关联的 **asyncio.Task** 作为参数。
    
    我们可以对所有任务使用相同的回调函数，并以通用方式报告进度，例如报告消息。
    
    例如:
    
    ```python
    # 回调函数显示任务进度
    def progress(task):
        # 报告任务进度
        print('.', end='')
    ```
    
    我们可以在我们发出的每个 **asyncio.Task** 上注册一个回调函数。
    
    这可以通过在每个任务上使用 [add_done_callback()](https://docs.python.org/3/library/asyncio-task.html#asyncio.Task.add_done_callback) 方法并向其传递回调函数的名称来实现。
    
    例如:
    
    ```python
    ...
    # 向任务添加回调函数
    task.add_done_callback(progress)
    ```

### 23.8 How to Run a Task After a Delay?

=== "English"

    We can develop a custom wrapper coroutine to execute a target coroutine after a delay.
    
    The wrapper coroutine may take two arguments, a coroutine and a time in seconds.
    
    It will sleep for the given delay interval in seconds, then await the provided coroutine.
    
    The **delay()** coroutine below implements this.
    
    ```python
    # coroutine that will start another coroutine after a delay in seconds
    async def delay(coro, seconds):
        # suspend for a time limit in seconds
        await asyncio.sleep(seconds)
        # execute the other coroutine
        await coro
    ```
    
    To use the wrapper coroutine, a coroutine object can be created and either awaited directly or executed independently as a task.
    
    For example, the caller may suspend and schedule the delayed coroutine and wait for it to be done:
    
    ```python
    ...
    # execute a coroutine after a delay
    await delay(coro, 10)
    ```
    
    Alternatively, the caller may schedule the delayed coroutine to run independently:
    
    ```python
    ...
    # execute a coroutine after a delay independently
    _ = asyncio.create_task(delay(coro, 10))
    ```

=== "Chinese"

    我们可以开发一个自定义包装协程来在延迟后执行目标协程。
    
    包装协程可以采用两个参数，一个协程和一个以秒为单位的时间。
    
    它将休眠给定的延迟间隔（以秒为单位），然后等待提供的协程执行完毕。
    
    下面的 **delay()** 协程实现了这一点。
    
    ```python
    # 延迟几秒后启动另一个协程的协程
    async def delay(coro, seconds):
        # 暂停时间限制（以秒为单位）
        await asyncio.sleep(seconds)
        # 执行另一个协程
        await coro
    ```
    
    要使用包装协程，可以创建协程对象并直接等待或作为任务独立执行。
    
    例如，调用者可以挂起并调度延迟协程并等待其完成：
    
    ```python
    ...
    # 延迟后执行协程
    await delay(coro, 10)
    ```
    
    或者，调用者可以安排延迟协程独立运行：
    
    ```python
    ...
    # 在延迟后独立执行协程
    _ = asyncio.create_task(delay(coro, 10))
    ```

### 23.9 How to Run a Follow-Up Task?

=== "English"

    There are three main ways to issue follow-up tasks in asyncio.

    They are:
    
    1. Schedule the follow-up task from the completed task itself.
    2. Schedule the follow-up task from the caller.
    3. Schedule the follow-up task automatically using a done callback.

    Let’s take a closer look at each approach.
    
    The task that is completed can issue its own follow-up task.
    
    This may require checking some state in order to determine whether the follow-up task should be issued or not.
    
    The task can then be scheduled via a call to `asyncio.create_task()`.
    
    For example:

    ```python
    ...
    # schedule a follow-up task
    task = asyncio.create_task(followup_task())
    ```

    The task itself may choose to await the follow-up task or let it complete in the background independently.

    For example:

    ```python
    ...
    # wait for the follow-up task to complete
    await task
    ```

    The caller that issued the task can choose to issue a follow-up task.

    For example, when the caller issues the first task, it may keep the asyncio.Task object.
    
    It can then check the result of the task or whether the task was completed successfully or not.
    
    The caller can then decide to issue a follow-up task.
    
    It may or may not await the follow-up task directly.
    
    For example:

    ```python
    ...
    # issue and await the first task
    task = await asyncio.create_task(task())
    # check the result of the task
    if task.result():
        # issue the follow-up task
        followup = await asyncio.create_task(followup_task())
    ```

    We can execute a follow-up task automatically using a done callback function.

    For example, the caller that issues the task can register a done callback function on the task itself.
    
    The done callback function must take the asyncio.Task object as an argument and will be called only after the task is done. It can then choose to issue a follow-up task.
    
    The done callback function is a regular Python function, not a coroutine, so it cannot await the follow-up task
    
    For example, the callback function may look as follows:

    ```python
    # callback function
    def callback(task):
        # schedule and await the follow-up task
        _ = asyncio.create_task(followup())
    ```

    The caller can issue the first task and register the done callback function.

    For example:

    ```python
    ...
    # schedule and the task
    task = asyncio.create_task(work())
    # add the done callback function
    task.add_done_callback(callback)
    ```

=== "Chinese"

    **asyncio** 中发出**后续任务**(follow-up tasks)的方式主要有三种。

    他们是:
    
    1. 从已完成的任务本身调度后续任务。
    2. 从调用者调度后续任务。
    3. 使用回调函数自动调度后续任务。

    让我们仔细看看每种方法。
    
    完成的任务可以发出自己的后续任务。
    
    这可能需要检查某些状态以确定是否应该发出后续任务。
    
    然后可以通过调用 `asyncio.create_task()` 来安排任务 .
    
    例如:

    ```python
    ...
    # schedule a follow-up task
    task = asyncio.create_task(followup_task())
    ```

    任务本身可以**选择等待**后续任务或让它在后台独立完成。

    例如:

    ```python
    ...
    # 等待后续任务执行完毕
    await task
    ```

    下发任务的调用者可以选择下发后续任务。

    例如，当调用者发出第一个任务时，它可能会保留 `asyncio.Task` 对象。
    
    然后它可以检查任务的结果或任务是否成功完成。
    
    然后调用者可以决定是否发出后续任务。
    
    它也可以直接选择等待/不等待后续任务执行完毕。
    
    例如:

    ```python
    ...
    # 发出并等待第一个任务
    task = await asyncio.create_task(task())
    # 检查任务结果
    if task.result():
        # 下达后续任务
        followup = await asyncio.create_task(followup_task())
    ```
    
    我们可以使用回调函数自动执行后续任务。

    例如，发出任务的调用者可以在任务本身上注册执行完成后的回调函数。
    
    回调函数必须将 `asyncio.Task` 对象作为参数，并且只有在任务完成后才会被调用。 然后它可以选择是否发出后续任务。
    
    回调函数是一个常规的Python函数，而不是协程，因此它不能等待后续任务
    
    例如，回调函数可能如下所示：

    ```python
    # 回调函数
    def callback(task):
        # 调度并等待后续任务
        _ = asyncio.create_task(followup())
    ```
    
    调用者可以发出第一个任务并注册执行完成后的回调函数。

    例如:

    ```python
    ...
    # 调度任务
    task = asyncio.create_task(work())
    # 添加执行完后的回调函数
    task.add_done_callback(callback)
    ```

### 23.10 How to Execute a Blocking I/O or CPU-bound Function in Asyncio?

=== "English"

    The asyncio module provides two approaches for executing blocking calls in asyncio programs.
    
    The first is to use the [asyncio.to_thread()](https://docs.python.org/3/library/asyncio-task.html#asyncio.to_thread) function.
    
    This is in the high-level API and is intended for application developers.
    
    The [asyncio.to_thread()](https://docs.python.org/3/library/asyncio-task.html#asyncio.to_thread) function takes a function name to execute and any arguments.
    
    The function is executed in a separate thread. It returns a coroutine that can be awaited or scheduled as an independent task.
    
    For example:
    
    ```python
    ...
    # execute a function in a separate thread
    await asyncio.to_thread(task)
    ```
    
    The task will not begin executing until the returned coroutine is given an opportunity to run in the event loop.
    
    The asyncio.to_thread() function creates a **ThreadPoolExecutor** behind the scenes to execute blocking calls.
    
    As such, the `asyncio.to_thread()` function is only appropriate for IO-bound tasks.
    
    An alternative approach is to use the [loop.run_in_executor()](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.loop.run_in_executor) function.
    
    This is in the low-level asyncio API and first requires access to the event loop, such as via the [asyncio.get_running_loop()](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.get_running_loop) function.
    
    The `loop.run_in_executor()` function takes an executor and a function to execute.
    
    If None is provided for the executor, then the default executor is used, which is a ThreadPoolExecutor.
    
    The `loop.run_in_executor()` function returns an awaitable that can be awaited if needed. The task will begin executing immediately, so the returned awaitable does not need to be awaited or scheduled for the blocking call to start executing.
    
    For example:
    
    ```python
    ...
    # get the event loop
    loop = asyncio.get_running_loop()
    # execute a function in a separate thread
    await loop.run_in_executor(None, task)
    ```
    
    Alternatively, an executor can be created and passed to the loop.run_in_executor() function, which will execute the asynchronous call in the executor.
    
    The caller must manage the executor in this case, shutting it down once the caller is finished with it.
    
    For example:

    ```python
    ...
    # create a process pool
    with ProcessPoolExecutor as exe:
        # get the event loop
        loop = asyncio.get_running_loop()
        # execute a function in a separate thread
        await loop.run_in_executor(exe, task)
        # process pool is shutdown automatically...
    ```

    These two approaches allow a blocking call to be executed as an asynchronous task in an asyncio program.

=== "Chinese"

    asyncio 模块提供了两种在 asyncio 程序中执行阻塞调用的方法。
    
    第一种是使用 [asyncio.to_thread()](https://docs.python.org/3/library/asyncio-task.html#asyncio.to_thread) 函数。
    
    这是高级 API 中的内容，适用于应用程序开发人员。
    
    `asyncio.to_thread()` 函数接受要执行的函数名称和任何参数。
    
    该函数在单独的线程中执行。 它返回一个可以作为独立任务等待或调度的协程。
    
    例如：
    
    ```python
    ...
    # 在单独的线程中执行函数
    await asyncio.to_thread(task)
    ```
    
    任务一开始并不会执行。直到协程返回并且给个在事件循环中运行的机会时，才会运行。
    
    `asyncio.to_thread()` 函数在后台创建一个 **ThreadPoolExecutor** 来执行阻塞调用。
    
    因此，`asyncio.to_thread()` 函数仅适用于 IO 密集型任务。
    
    另一种方法是使用[loop.run_in_executor()](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.loop.run_in_executor)函数。
    
    这是在低级 asyncio API 中，首先需要访问事件循环，例如通过 [asyncio.get_running_loop()](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.get_running_loop) 函数。
    
    `Loop.run_in_executor()` 函数需要一个执行器和一个要执行的函数。
    
    如果没有为执行器提供参数，则默认值为 None，将使用默认执行器，即 **ThreadPoolExecutor**。
    
    `Loop.run_in_executor()` 函数返回一个可等待的对象，如果需要可以等待。 该任务将立即开始执行，因此不需要等待或安排返回的可等待对象来开始执行阻塞调用。
    
    例如：
    
    ```python
    ...
    # 获取事件循环
    loop = asyncio.get_running_loop()
    # 在单独的线程中执行函数
    await loop.run_in_executor(None, task)
    ```
    
    或者，可以创建一个执行器并将其传递给`loop.run_in_executor()`函数，该函数将在执行器中执行异步调用。
    
    在这种情况下，调用者必须管理执行器，在调用者完成后将其关闭。
    
    例如:
    
    ```python
    ...
    # 创建一个进程池
    with ProcessPoolExecutor as exe:
        # 获取事件循环
        loop = asyncio.get_running_loop()
        # 在单独的线程中执行函数
        # process pool is shutdown automatically...
        # 进程池自动关闭...
    ```
    
    这两种方法允许阻塞调用作为 asyncio 程序中的异步任务执行。

## 24. Common Objections to Using Asyncio

=== "English"

    Asyncio and coroutines may not be the best solution for all concurrency problems in your program.
    
    That being said, there may also be some misunderstandings that are preventing you from making full and best use of the capabilities of the asyncio in Python.
    
    In this section, we review some of the common objections seen by developers when considering using the asyncio.

=== "Chinese"

    异步和协程可能不是解决程序中所有并发问题的最佳解决方案。

    话虽如此，也可能存在一些误解，阻碍您充分、最佳地利用 Python 中 asyncio 的功能。

    在本节中，我们将回顾开发人员在考虑使用 asyncio 时遇到的一些常见反对意见。

### 24.1 What About the Global Interpreter Lock (GIL)?

The GIL protects the internals of the Python interpreter from concurrent access and modification from multiple threads.

- GIL 保护 Python 解释器的内部免受多个线程的并发访问和修改。

The asyncio event loop runs in one thread.

- asyncio 事件循环在一个线程中运行。

This means that all coroutines run in a single thread.

- 这意味着所有协程都在单个线程中运行。

As such the GIL is not an issue when using asyncio and coroutine.

- 因此，使用 asyncio 和协程时，GIL 不是问题。

### 24.2 Are Python Coroutines “Real“?

Coroutines are managed in software.

- 协程由软件管理。

Coroutines run and are managed (switched) within the asyncio event loop in the Python runtime.

- 协程在 Python 运行时的 **asyncio 事件循环**中运行和管理（切换）。

They are not a software representation of a capability provided by the underlying operating system, like threads and processes.

- 它们不是底层操作系统级别提供的功能以及软件表示，例如线程和进程。

In this sense, Python does not have support for “native coroutines”, but I’m not sure such things exist in modern operating systems.

- 从这个意义上说，Python 不支持“原生协程”，但我不确定现代操作系统中是否存在这样的东西。

### 24.3 Isn’t Python Concurrency Buggy?

No.

- 不🙅🏻‍♀️。

Python provides first-class concurrency with coroutines, threads, and processes.

- Python 通过协程、线程和进程提供一流的并发性。

It has for a long time now and it is widely used in open source and commercial projects.

- 它已经存在很长时间了，并且广泛应用于开源和商业项目中。

### 24.4 Isn’t Python a Bad Choice for Concurrency?

Developers love python for many reasons, most commonly because it is easy to use and fast for development.

- 开发人员喜爱 Python 的原因有很多，最常见的是因为它易于使用且开发速度快。

Python is commonly used for glue code, one-off scripts, but more and more for large-scale software systems.

- Python 通常用于粘合代码、一次性脚本，但越来越多地用于大型软件系统。

If you are using Python and then you need concurrency, then you work with what you have. The question is moot.

- 如果您使用 Python 并且需要并发性，那么您可以使用现有的东西。 这个问题毫无意义。

If you need concurrency and you have not chosen a language, perhaps another language would be more appropriate, or perhaps not. Consider the full scope of functional and non-functional requirements (or user needs, wants, and desires) for your project and the capabilities of different development platforms.

- 如果您需要并发性并且尚未选择一种语言，那么另一种语言可能更合适，也可能不合适。 需要考虑项目的全部功能和非功能需求（或用户的需求、想法和愿望）以及不同开发平台的功能。

### 25.5 Why Not Use Threads Instead?

You can use threads instead of asyncio.

- 您可以使用线程而不是异步。

Any program developed using threads can be rewritten to use asyncio and coroutines.

- 任何使用**线程**开发的程序都可以使用 **asyncio 和协程**重写。

Any program developed using coroutines and asyncio can be rewritten to use threads.

- 任何使用**协程和 asyncio** 开发的程序都可以使用**线程**重写。

Adopting asyncio in a project is a choice, the rationale is yours.

- 在项目中采用 asyncio 是一种选择，其理由由您决定。

For the most part, they are **functionally equivalent.**（功能等效）

- 在大多数情况下，它们在功能上是等效的。

Many use cases will execute faster using threads and may be more familiar(亲切) to a wider array of Python developers.

- 许多用例使用线程将执行得更快，并且可能为更广泛的 Python 开发人员所熟悉。

Some use cases in the areas of network programming and executing system commands may be simpler(最简单) (less code) when using asyncio, and significantly more scalable than using threads.

- 使用 asyncio 时，网络编程和执行系统命令领域的一些用例可能会更简单（最简单）（代码更少），并且比使用线程更具可扩展性。

## 25. Further Reading

This section lists helpful additional resources on the topic.

### 25.1 Python Asyncio Books

This section lists my books on Python asyncio, designed to help you get started and get good, super fast.

- [Python Asyncio Jump-Start](https://superfastpython.com/paj-further-reading), Jason Brownlee, 2022. (my book!)
- [Python Asyncio Interview Questions](https://amzn.to/3XFEZgj)
- [Asyncio Module API Cheat Sheet](https://superfastpython.gumroad.com/l/pacs)

Other books on asyncio include:

- [Python Concurrency with asyncio](https://amzn.to/3LZvxNn), Matthew Fowler, 2022.
- [Using Asyncio in Python](https://amzn.to/3lNp2ml), Caleb Hattingh, 2020.

### 25.2 APIs

- [asyncio — Asynchronous I/O](https://docs.python.org/3/library/asyncio.html)
- [Asyncio Coroutines and Tasks](https://docs.python.org/3/library/asyncio-task.html)
- [Asyncio Streams](https://docs.python.org/3/library/asyncio-stream.html)
- [Asyncio Subprocesses](https://docs.python.org/3/library/asyncio-subprocess.html)
- [Asyncio Queues](https://docs.python.org/3/library/asyncio-queue.html)
- [Asyncio Synchronization Primitives](https://docs.python.org/3/library/asyncio-sync.html)

### 25.3 References

- [Asynchronous I/O, Wikipedia.](https://en.wikipedia.org/wiki/Asynchronous_I/O)
- [Coroutine, Wikipedia.](https://en.wikipedia.org/wiki/Coroutine)

## 26. Conclusions

This is a large guide, and you have discovered in great detail how asyncio and coroutines work in Python and how to best use them in your project.

**Did you find this guide useful?**

I’d love to know, please share a kind word in the comments below.

**Have you used asyncio on a project?**

I’d love to hear about it, please let me know in the comments.

**Do you have any questions?**

Leave your question in a comment below and I will reply fast with my best advice.

Join the discussion on [reddit](https://www.reddit.com/r/Python/comments/yqrr94/python_asyncio_the_complete_guide/) and [hackernews](https://news.ycombinator.com/item?id=33547323).
