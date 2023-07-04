# 如何使用async.Runner执行多个协程

转自: <https://superfastpython.com/asyncio-runner/>

=== "中文"

    您可以使用高级 asyncio API 中的 asyncio.Runner 类在同一事件循环中执行多个协程。
    
    这是Python 3.11中提供的新功能。 在添加 **asyncio.Runner** 类之前，我们必须在单独的事件循环中执行每个协程，重组我们的程序以使用包装器协程或深入研究低级 asyncio API。

    在本教程中，您将了解如何使用 **asyncio.Runner** 类在 Python 的同一事件循环中执行多个协程。

    让我们开始吧。

=== "原文"

    **How to Execute Multiple Coroutines with asyncio.Runner**

    You can execute multiple coroutines in the same event loop using the asyncio.Runner class in the high-level asyncio API.
    
    This is a new feature provided in Python 3.11. Before the addition of the **asyncio.Runner** class, we would have to execute each coroutine in a separate event loop, restructure our program to use a wrapper coroutine or delve into the low-level asyncio API.
    
    In this tutorial, you will discover how to execute multiple coroutines in the same event loop from Python using the **asyncio.Runner** class.
    
    Let’s get started.

## 需要在 Python 中运行多个协程

=== "中文"

    运行 asyncio 程序的典型方法是调用 asyncio.run() 并传入一个协程对象来执行。

    例如：
    
    ```python
    ...
    asyncio.run(coro())
    ```
    
    提供给 asyncio.run() 的单个协程表示 asyncio 事件循环的入口点。
    
    在某些情况下，我们可能希望从 Python 程序中执行多个单独的协程。

    这可以通过多次调用 asyncio.run() 来实现，但这将为每个调用启动并运行一个新的 asyncio 事件循环。

    例如：
    
    ```python
    ...
    asyncio.run(coro1())
    asyncio.run(coro2())
    ```
    
    另一种解决方案是重构程序并创建一个新的入口点协程来为我们调用多个协程。

    例如：
    
    ```python
    async def main():
        await coro1()
        await coro2()
     
    asyncio.run(main())
    ```
    
    我们是否可以在同一个事件循环中运行多个协程，而无需通过高级 asyncio API 重构我们的程序？

    使用所有 CPU 运行循环，[下载我的免费书](https://superfastpython.com/plip-incontent) 以了解如何操作。

=== "原文"

    **Need to Run Multiple Coroutines from Python**

    The typical way to run an asyncio program is to call asyncio.run() and pass in a coroutine object to execute.
    
    For example:
    
    ```python
    ...
    asyncio.run(coro())
    ```
    
    The single coroutine is provided to the asyncio.run() represents the entry point into the asyncio event loop.
    
    There may be cases where we want to execute multiple separate coroutines from our Python program.
    
    This could be achieved with multiple calls to asyncio.run(), but this would start and run a new asyncio event loop for each call.
    
    For example:
    
    ```python
    ...
    asyncio.run(coro1())
    asyncio.run(coro2())
    ```
    
    Another solution is to restructure the program and create a new entry point coroutine that calls the multiple coroutines for us.
    
    For example:
    
    ```python
    async def main():
        await coro1()
        await coro2()
     
    asyncio.run(main())
    ```
    
    Can we run multiple coroutines in the same event loop without having to restructure our program via the high-level asyncio API?
    
    Run your loops using all CPUs, [download my FREE book](https://superfastpython.com/plip-incontent) to learn how.

## 如何使用 asyncio.Runner 运行多个协程

=== "中文"

    Python 3.11 引入了一项新功能，允许直接从 Python 程序运行多个协程。

    此功能是通过高级 asyncio API 中提供的 [asyncio.Runner](https://docs.python.org/3/library/asyncio-runner.html#runner-context-manager) 类提供的。
    
    !!! info ""
    
        添加了 Runner 类，它公开了 run() 使用的机制。
        
        — [Python 3.11 的新功能](https://docs.python.org/3/whatsnew/3.11.html)
    
    可以创建 **asyncio.Runner** 并用于在同一事件循环中执行多个协程。

    可以通过 **run()** 方法创建该类的实例并直接执行协程。 完成后，可以调用 **close()** 方法。

    例如：

    ```python
    ...
    # 创建异步运行器
    runner = asyncio.Runner()
    # 执行第一个协程
    runner.run(coro1())
    # 执行第二个协程
    runner.run(coro2())
    # close runner
    runner.close()
    ```
    
    或者，我们可以使用类上的上下文管理器接口，当我们完成它时，它将为我们关闭事件循环。

    例如：

    ```python
    ...
    # 创建异步运行器
    with asyncio.Runner() as runner:
        # 执行第一个协程
        runner.run(coro1())
        # 执行第二个协程
        runner.run(coro2())
    ```
    
    现在我们知道如何在 Python 程序的同一事件循环中执行多个协程，让我们看一些有效的示例。

=== "原文"

    **How to Run Multiple Coroutines with asyncio.Runner**

    Python 3.11 introduced a new feature to allow multiple coroutines to be run directly from a Python program.
    
    This capability is provided via the [asyncio.Runner](https://docs.python.org/3/library/asyncio-runner.html#runner-context-manager) class, provided in the high-level asyncio API.
    
    !!! info ""
    
        Added the Runner class, which exposes the machinery used by run().
        
        — [WHAT’S NEW IN PYTHON 3.11](https://docs.python.org/3/whatsnew/3.11.html)
    
    The **asyncio.Runner** can be created and used to execute multiple coroutines within the same event loop.
    
    An instance of the class can be created and coroutines can be executed directly via the **run()** method. Once finished, the **close()** method can be called.
    
    For example:
    
    ```python
    ...
    # create asyncio runner
    runner = asyncio.Runner()
    # execute first coroutine
    runner.run(coro1())
    # execute second coroutine
    runner.run(coro2())
    # close runner
    runner.close()
    ```
    
    Alternatively, we can use the context manager interface on the class which will close the event loop for us when we’re finished with it.
    
    For example:
    
    ```python
    ...
    # create asyncio runner
    with asyncio.Runner() as runner:
        # execute first coroutine
        runner.run(coro1())
        # execute second coroutine
        runner.run(coro2())
    ```
    
    Now that we know how to execute multiple coroutines in the same event loop from our Python program, let’s look at some worked examples.

## 使用 asyncio.run() 运行多个协程的示例

=== "中文"

    在查看使用 **asyncio.Runner** 的示例之前，让我们看看在提供 **asyncio.Runner** 之前如何运行多个协程，例如 Python 3.10 及更低版本。

    考虑这样的情况：我们的 Python 程序有两个协程运行。
    
    ```python
    # 协程示例
    async def task_coro1():
        print('Hello from first coro')
        await asyncio.sleep(1)
     
    # 另一个协程示例
    async def task_coro2():
        print('Hello from second coro')
        await asyncio.sleep(1)
    ```
    
    我们可以使用两种方法：
    
    1. 单独调用 asyncio.run()
    2. 创建包装协程

=== "原文"

    **Example of Running Multiple Coroutines with asyncio.run()**
    
    Before we look at an example of using **asyncio.Runner**, let’s look at how we would run multiple coroutines before the **asyncio.Runner** was provided, e.g. Python 3.10 and lower.
    
    Consider the case where we have two coroutines to run from our Python program.
    
    ```python
    # example coroutine
    async def task_coro1():
        print('Hello from first coro')
        await asyncio.sleep(1)
     
    # another example coroutine
    async def task_coro2():
        print('Hello from second coro')
        await asyncio.sleep(1)
    ```
    
    There are two methods we could use:
    
    1. Separate calls to asyncio.run()
    2. Create a wrapper coroutine

### 方法 1：单独调用 asyncio.run()

=== "中文"

    一种方法是单独运行每个协程。

    例如：
    
    ```python
    # run first coroutine
    asyncio.run(task_coro1())
    # run second coroutine
    asyncio.run(task_coro2())
    ```
    
    下面列出了使用此方法的完整示例。
    
    ```python
    # 使用单独的 asyncio.run() 调用的示例
    import asyncio
     
    # 协程示例
    async def task_coro1():
        print('Hello from first coro')
        await asyncio.sleep(1)
     
    # 另一个协程示例
    async def task_coro2():
        print('Hello from second coro')
        await asyncio.sleep(1)
     
    # 运行第一个协程
    asyncio.run(task_coro1())
    # 运行第二个协程
    asyncio.run(task_coro2())
    ```
    
    运行该示例首先创建一个 asyncio 事件循环，并使用该循环执行第一个协程。
    
    然后事件循环被关闭并清理。

    创建第二个异步事件循环，并用于在关闭和清理之前执行第二个协程。
    
    ```text
    Hello from first coro
    Hello from second coro
    ```
    
    这种方法的成本很高，因为必须为每个协程创建并关闭新的事件循环。 如果我们有数千或数百万个协程要从 Python 执行，这可能会成为一个问题。

=== "原文"

    **Method 1: Separate Calls to asyncio.run()**

    One approach would be to run each coroutine separately.
    
    For example:
    
    ```python
    # run first coroutine
    asyncio.run(task_coro1())
    # run second coroutine
    asyncio.run(task_coro2())
    ```
    
    A complete example using this method is listed below.
    
    ```python
    # example of using separate asyncio.run() calls
    import asyncio
     
    # example coroutine
    async def task_coro1():
        print('Hello from first coro')
        await asyncio.sleep(1)
     
    # another example coroutine
    async def task_coro2():
        print('Hello from second coro')
        await asyncio.sleep(1)
     
    # run first coroutine
    asyncio.run(task_coro1())
    # run second coroutine
    asyncio.run(task_coro2())
    ```
    
    Running the example first creates an asyncio event loop and uses the loop to execute the first coroutine.
    
    The event loop is then closed and cleaned up.
    
    A second asyncio event loop is created and used to execute the second coroutine before being closed and cleaned up.
    
    ```text
    Hello from first coro
    Hello from second coro
    ```
    
    This approach is expensive as a new event loop must be created and closed for each coroutine. This can become a problem if we have thousands or millions of coroutines to execute from Python.

### 方法2：包装为协程

=== "中文"

    另一种方法是定义一个新的包装协程，该协程反过来使用单个事件循环为我们调用协程。

    例如：

    ```python
    # 异步入口点
    async def main():
        # 执行第一个协程
        await task_coro1()
        # 执行第二个协程
        await task_coro2()
    ```
    
    下面列出了此更改的完整示例。

    ```python
    # 将 asyncio.run() 与包装协程一起使用的示例
    from asyncio import run
    from asyncio import sleep
     
    # 协程示例
    async def task_coro1():
        print('Hello from first coro')
        await sleep(1)
     
    # 另一个协程示例
    async def task_coro2():
        print('Hello from second coro')
        await sleep(1)
     
    # 异步入口点
    async def main():
        # 执行第一个协程
        await task_coro1()
        # 执行第二个协程
        await task_coro2()
     
    # 程序的入口点
    run(main())
    ```
    
    运行该示例将创建一个异步事件循环并执行 **main()** 包装协程。

    然后，**main()** 协程依次执行并等待每个任务协程，然后关闭。
    
    ```text
    Hello from first coro
    Hello from second coro
    ```
    
    这种方法的问题在于，必须使用新的包装协程作为执行所有协程的入口点来重构程序。
    
    这可能意味着在执行包装器协程之前必须收集和设置所需的所有数据和协程。 它还不允许 Python 程序根据先前协程的结果有条件地创建和启动协程。

    接下来，让我们看看使用 asyncio.Runner 类的替代方案。

=== "原文"

    **Method 2: Wrapper Coroutine**
    
    Another approach is to define a new wrapper coroutine that in turn calls the coroutines for us using the single event loop.
    
    For example:
    
    ```python
    # asyncio entry point
    async def main():
        # execute the first coroutine
        await task_coro1()
        # execute the second coroutine
        await task_coro2()
    ```
    
    The complete example of this change is listed below.
    
    ```python
    
    # example of using asyncio.run() with wrapper coroutine
    from asyncio import run
    from asyncio import sleep
     
    # example coroutine
    async def task_coro1():
        print('Hello from first coro')
        await sleep(1)
     
    # another example coroutine
    async def task_coro2():
        print('Hello from second coro')
        await sleep(1)
     
    # asyncio entry point
    async def main():
        # execute the first coroutine
        await task_coro1()
        # execute the second coroutine
        await task_coro2()
     
    # entry point of the program
    run(main())
    ```
    
    Running the example creates a single asyncio event loop and executes the **main()** wrapper coroutine.
    
    The **main()** coroutine then executes and awaits each task coroutine in turn before closing.
    
    ```text
    Hello from first coro
    Hello from second coro
    ```
    
    The problem with this approach is that the program must be restructured with a new wrapper coroutine used as the entry point for executing all coroutines.
    
    This may mean that all data and coroutines needed must be collected and set up before executing the wrapper coroutine. It also does not allow the Python program to conditionally create and start coroutines based on the results of prior coroutines.
    
    Next, let’s look at an alternative using the asyncio.Runner class.

## 使用 asyncio.Runner 运行多个协程的示例

=== "中文"

    我们可以开发一个使用 **asyncio.Runner** 类从 Python 程序执行多个协程的示例。
    
    这可以使用 **asyncio.Runner** 的上下文管理器接口来实现，并调用每个协程的 **run()** 方法来执行。

    例如：
    
    ```python
    # 程序的入口点
    with Runner() as runner:
        # execute the first coroutine
        runner.run(task_coro1())
        # execute the second coroutine
        runner.run(task_coro2())
    ```

    下面列出了这种方法的完整示例。

    ```python
    # 使用 asyncio.Runner 的示例
    from asyncio import Runner
    from asyncio import sleep
     
    # 协程示例
    async def task_coro1():
        print('Hello from first coro')
        await sleep(1)
     
    # 另一个协程示例
    async def task_coro2():
        print('Hello from second coro')
        await sleep(1)
     
    # 程序的入口点
    with Runner() as runner:
        # 执行第一个协程
        runner.run(task_coro1())
        # 执行第二个协程
        runner.run(task_coro2())
    ```
    
    运行该示例使用上下文管理器接口创建 asyncio.Runner。
    
    第一个协程运行，然后是第二个。

    每个协程都在同一个事件循环中执行，并且直到第一次调用 run() 才会创建所使用的循环。
    
    ```text
    Hello from first coro
    Hello from second coro
    ```
    
    这种方法为 Python 程序提供了灵活性，允许它在同一事件循环中以临时甚至有条件的方式执行协程。

=== "原文"

    **Example of Running Multiple Coroutines with asyncio.Runner**

    We can develop an example of executing multiple coroutines from a Python program using the **asyncio.Runner** class.
    
    This can be achieved using the context manager interface for the **asyncio.Runner** and call the **run()** method for each coroutine to execute.
    
    For example:
    
    ```python
    # entry point of the program
    with Runner() as runner:
        # execute the first coroutine
        runner.run(task_coro1())
        # execute the second coroutine
        runner.run(task_coro2())
    ```
    
    A complete example of this approach is listed below.
    
    ```python
    # example of using asyncio.Runner
    from asyncio import Runner
    from asyncio import sleep
     
    # example coroutine
    async def task_coro1():
        print('Hello from first coro')
        await sleep(1)
     
    # another example coroutine
    async def task_coro2():
        print('Hello from second coro')
        await sleep(1)
     
    # entry point of the program
    with Runner() as runner:
        # execute the first coroutine
        runner.run(task_coro1())
        # execute the second coroutine
        runner.run(task_coro2())
    ```
    
    Running the example creates the asyncio.Runner using the context manager interface.
    
    The first coroutine is run, then the second.
    
    Each coroutine is executed in the same event loop and the loop used is not created until the first call to run().
    
    ```text
    Hello from first coro
    Hello from second coro
    ```
    
    This approach provides flexibility to the Python program, allowing it to execute coroutines in an ad hoc and even conditional manner within the same event loop.

## 进一步阅读

=== "中文"

    本节提供了您可能会觉得有用的其他资源。
    
    **书籍**
    
    - [Python Asyncio Jump-Start](https://amzn.to/3UDIf9K), Jason Brownlee, 2022 (my book).
    - [Python Asyncio Interview Questions](https://amzn.to/3XFEZgj)
    - [Asyncio Module API Cheat Sheet](https://superfastpython.gumroad.com/l/pacs)
    
    我还推荐以下书籍:
    
    - [Python Concurrency with asyncio](https://amzn.to/3LZvxNn), Matthew Fowler, 2022.
    - [Using Asyncio in Python](https://amzn.to/3lNp2ml), Caleb Hattingh, 2020.
    
    **指南**
    
    - [Python Asyncio: The Complete Guide](https://superfastpython.com/python-asyncio/)
    
    **APIs**
    
    - [asyncio — Asynchronous I/O](https://docs.python.org/3/library/asyncio.html)
    - [Asyncio Coroutines and Tasks](https://docs.python.org/3/library/asyncio-task.html)
    - [Asyncio Streams](https://docs.python.org/3/library/asyncio-stream.html)
    - [Asyncio Subprocesses](https://docs.python.org/3/library/asyncio-subprocess.html)
    - [Asyncio Queues](https://docs.python.org/3/library/asyncio-queue.html)
    - [Asyncio Synchronization Primitives](https://docs.python.org/3/library/asyncio-sync.html)
    
    **参考**
    
    - [Asynchronous I/O, Wikipedia.](https://en.wikipedia.org/wiki/Asynchronous_I/O)
    - [Coroutine, Wikipedia.](https://en.wikipedia.org/wiki/Coroutine)

=== "原文"

    **Further Reading**

    This section provides additional resources that you may find helpful.
    
    **Books**
    
    - [Python Asyncio Jump-Start](https://amzn.to/3UDIf9K), Jason Brownlee, 2022 (my book).
    - [Python Asyncio Interview Questions](https://amzn.to/3XFEZgj)
    - [Asyncio Module API Cheat Sheet](https://superfastpython.gumroad.com/l/pacs)
    
    I also recommend the following books:
    
    - [Python Concurrency with asyncio](https://amzn.to/3LZvxNn), Matthew Fowler, 2022.
    - [Using Asyncio in Python](https://amzn.to/3lNp2ml), Caleb Hattingh, 2020.
    
    **Guides**
    
    - [Python Asyncio: The Complete Guide](https://superfastpython.com/python-asyncio/)
    
    **APIs**
    
    - [asyncio — Asynchronous I/O](https://docs.python.org/3/library/asyncio.html)
    - [Asyncio Coroutines and Tasks](https://docs.python.org/3/library/asyncio-task.html)
    - [Asyncio Streams](https://docs.python.org/3/library/asyncio-stream.html)
    - [Asyncio Subprocesses](https://docs.python.org/3/library/asyncio-subprocess.html)
    - [Asyncio Queues](https://docs.python.org/3/library/asyncio-queue.html)
    - [Asyncio Synchronization Primitives](https://docs.python.org/3/library/asyncio-sync.html)
    
    **References**
    
    - [Asynchronous I/O, Wikipedia.](https://en.wikipedia.org/wiki/Asynchronous_I/O)
    - [Coroutine, Wikipedia.](https://en.wikipedia.org/wiki/Coroutine)
