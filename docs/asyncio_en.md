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

## 16. 异步生成器

**16. Asynchronous Generators**

=== "English"

    Generators are a fundamental part of Python.
    
    A generator is a function that has at least one **“yield”** expression. They are functions that can be suspended and resumed, just like coroutines.
    
    In fact, Python coroutines are an extension of Python generators.
    
    Asyncio allows us to develop asynchronous generators.
    
    We can create an asynchronous generator by defining a coroutine that makes use of the “yield” expression.
    
    Let’s take a closer look.

=== "Chinese"

    生成器是 Python 的基本组成部分。
    
    生成器是一种至少具有一个“**yield**”表达式的函数。 它们是可以暂停和恢复的函数，就像协程一样。
    
    事实上，Python 协程是 Python 生成器的扩展。
    
    Asyncio 允许我们开发异步生成器。
    
    我们可以通过定义使用“**yield**”表达式的协程来创建异步生成器。
    
    让我们仔细看看。

### 16.1 什么是异步生成器

**16.1 What Are Asynchronous Generators**

=== "English"

    An asynchronous generator is a coroutine that uses the yield expression.
    
    Before we dive into the details of asynchronous generators, let’s first review classical Python generators.

=== "Chinese"

    异步生成器是使用yield 表达式的协程。
    
    在我们深入了解异步生成器的细节之前，让我们首先回顾一下经典的 Python 生成器。

#### 16.1.1 生成器

**16.1.1 Generators**

=== "English"

    A generator is a Python function that returns a value via a yield expression.
    
    For example:
    
    ```python
    # define a generator
    def generator():
        for i in range(10):
            yield i
    ```
    
    The generator is executed to the yield expression, after which a value is returned. This suspends the generator at that point. The next time the generator is executed it is resumed from the point it was resumed and runs until the next yield expression.
    
    > generator: A function which returns a generator iterator. It looks like a normal function except that it contains yield expressions for producing a series of values usable in a for-loop or that can be retrieved one at a time with the next() function.
    >
    > — [PYTHON GLOSSARY](https://docs.python.org/3/glossary.html)
    
    Technically, a generator function creates and returns a generator iterator. The generator iterator executes the content of the generator function, yielding and resuming as needed.
    
    > generator iterator: An object created by a generator function. Each yield temporarily suspends processing, remembering the location execution state […] When the generator iterator resumes, it picks up where it left off …
    >
    > — [PYTHON GLOSSARY](https://docs.python.org/3/glossary.html)
    
    A generator can be executed in steps by using the next() built-in function.
    
    For example:
    
    ```python
    ...
    # create the generator
    gen = generator()
    # step the generator
    result = next(gen)
    ```
    
    Although, it is more common to iterate the generator to completion, such as using a for-loop or a list comprehension.
    
    For example:
    
    ```python
    ...
    # traverse the generator and collect results
    results = [item for item in generator()]
    ```
    
    Next, let’s take a closer look at asynchronous generators.

=== "Chinese"

    生成器是一个Python函数，它通过yield表达式返回一个值。
    
    例如:
    
    ```python
    # 定义生成器
    def generator():
        for i in range(10):
            yield i
    ```
    
    生成器执行到yield表达式，然后返回一个值。 这会在此时暂停生成器。 下次执行生成器时，它将从恢复点恢复并运行到下一个 yield 表达式。
    
    > 生成器: 返回生成器迭代器的函数。 它看起来像一个普通函数，只不过它包含用于生成一系列可在 for 循环中使用的值的yield 表达式，或者可以使用 **next()** 函数一次检索一个值。
    >
    > — [PYTHON GLOSSARY](https://docs.python.org/3/glossary.html)
    
    从技术上讲，生成器函数创建并返回生成器迭代器。 生成器迭代器执行生成器函数的内容，根据需要产生并恢复。
    
    > 生成迭代器: 由生成器函数创建的对象。 每个yield都会暂时挂起处理，记住位置执行状态[...]当生成器迭代器恢复时，它会从上次停止的地方继续...
    >
    > — [PYTHON GLOSSARY](https://docs.python.org/3/glossary.html)
    
    可以使用 **next()** 内置函数逐步执行生成器。
    
    例如:
    
    ```python
    ...
    # 创建生成器
    gen = generator()
    # 生成器的下一步
    result = next(gen)
    ```
    
    尽管如此，更常见的是迭代生成器以完成，例如使用 for 循环或列表理解。
    
    例如:
    
    ```python
    ...
    # 遍历生成器并收集结果
    results = [item for item in generator()]
    ```
    
    接下来，让我们仔细看看异步生成器。

#### 16.1.2 异步生成器

**16.1.2 Asynchronous Generators**

=== "English"

    An asynchronous generator is a coroutine that uses the yield expression.
    
    Unlike a function generator, the coroutine can schedule and await other coroutines and tasks.
    
    > asynchronous generator: A function which returns an asynchronous generator iterator. It looks like a coroutine function defined with async def except that it contains yield expressions for producing a series of values usable in an async for loop.
    >
    > — [PYTHON GLOSSARY](https://docs.python.org/3/glossary.html)
    
    Like a classical generator, an asynchronous generator function can be used to create an asynchronous generator iterator that can be traversed using the built-in anext() function, instead of the next() function.
    
    > asynchronous generator iterator: An object created by a asynchronous generator function. This is an asynchronous iterator which when called using the __anext__() method returns an awaitable object which will execute the body of the asynchronous generator function until the next yield expression.
    >
    > — [PYTHON GLOSSARY](https://docs.python.org/3/glossary.html)
    
    This means that the asynchronous generator iterator implements the **\_\_anext\_\_()** method and can be used with the async for expression.
    
    This means that each iteration of the generator is scheduled and executed as awaitable. The “**async for**” expression will schedule and execute each iteration of the generator, suspending the calling coroutine and awaiting the result.
    
    You can learn more about the “**async for**” expression in the tutorial:
    
    - [Asyncio async for loop](https://superfastpython.com/asyncio-async-for/)

=== "Chinese"

    异步生成器是使用yield 表达式的协程。
    
    与函数生成器不同，协程可以调度和等待其他协程和任务。
    
    > 异步生成器: 返回异步生成器迭代器的函数。 它看起来像一个使用 **async def** 定义的协程函数，只不过它包含用于生成一系列可在 **async for** 循环中使用的值的 yield 表达式。
    >
    > — [PYTHON GLOSSARY](https://docs.python.org/3/glossary.html)
    
    与经典生成器一样，异步生成器函数可用于创建异步生成器迭代器，该迭代器可以使用内置 **anext()** 函数（而不是 **next()** 函数）进行遍历。
    
    > 基于异步生成器的迭代器：由异步生成器函数创建的对象。 这是一个异步迭代器，当使用 **\_\_anext\_\_()** 方法调用时，它会返回一个可等待对象，该对象将执行异步生成器函数的主体，直到下一个 yield 表达式。
    >
    > — [PYTHON GLOSSARY](https://docs.python.org/3/glossary.html)
    
    这意味着异步生成器迭代器实现了 **\_\_anext\_\_()** 方法，并且可以与 async for 表达式一起使用。
    
    这意味着生成器的每次迭代都被调度并作为可等待执行。 “**async for**”表达式将调度并执行生成器的每次迭代，挂起调用协程并等待结果。
    
    您可以在教程中了解有关“**async for**”表达式的更多信息：
    
    - [Asyncio 的异步 for 循环](https://superfastpython.com/asyncio-async-for/)

### 16.2 如何使用异步生成器

**16.2 How to Use an Asynchronous Generator**

=== "English"

    In this section, we will take a close look at how to define, create, step, and traverse an asynchronous generator in asyncio programs.

    Let’s start with how to define an asynchronous generator.

=== "Chinese"

    在本节中，我们将仔细研究如何在 asyncio 程序中定义、创建、单步执行和遍历异步生成器。
    
    让我们从如何定义异步生成器开始。

#### 16.2.1 定义异步生成器

**16.2.1 Define an Asynchronous Generator**

=== "English"

    We can define an asynchronous generator by defining a coroutine that has at least one yield expression.
    
    This means that the function is defined using the “**async def**” expression.
    
    For example:
    
    ```python
    # define an asynchronous generator
    async def async_generator():
        for i in range(10)
            yield i
    ```
    
    Because the asynchronous generator is a coroutine and each iterator returns an awaitable that is scheduled and executed in the asyncio event loop, we can execute and await awaitables within the body of the generator.
    
    For example:
    
    ```python
    # define an asynchronous generator that awaits
    async def async_generator():
        for i in range(10)
            # suspend and sleep a moment
            await asyncio.sleep(1)
            # yield a value to the caller
            yield i
    ```
    
    Next, let’s look at how we might use an asynchronous generator.

=== "Chinese"

    我们可以通过定义一个至少具有一个yield 表达式的协程来定义异步生成器。
    
    这意味着该函数是使用“**async def**”表达式定义的。
    
    例如:
    
    ```python
    # 定义一个异步生成器
    async def async_generator():
        for i in range(10)
            yield i
    ```
    
    因为异步生成器是一个协程，并且每个迭代器返回一个在 asyncio 事件循环中调度和执行的等待对象，所以我们可以在生成器的主体内执行和等待等待对象。
    
    例如:
    
    ```python
    # 定义一个等待的异步生成器
    async def async_generator():
        for i in range(10)
            # 暂停并睡眠一会儿
            await asyncio.sleep(1)
            # 向调用者产生一个值
            yield i
    ```
    
    接下来，让我们看看如何使用异步生成器。

#### 16.2.2 创建异步生成器

**16.2.2 Create Asynchronous Generator**

=== "English"

    To use an asynchronous generator we must create the generator.
    
    This looks like calling it, but instead creates and returns an iterator object.
    
    For example:
    
    ```python
    ...
    # create the iterator
    it = async_generator()
    ```
    
    This returns a type of asynchronous iterator called an asynchronous generator iterator.

=== "Chinese"

    要使用异步生成器，我们必须创建生成器。
    
    这看起来像是调用它，但实际上是创建并返回一个迭代器对象。
    
    例如：
    
    ```python
    ...
    # 创建迭代器
    it = async_generator()
    ```
    
    这会返回一种称为异步生成器的可迭代的异步迭代器。

#### 16.2.3 使用异步生成器

**16.2.3 Step an Asynchronous Generator**

=== "English"

    One step of the generator can be traversed using the [anext()](https://docs.python.org/3/library/functions.html#anext) built-in function, just like a classical generator using the **next()** function.
    
    The result is an awaitable that is awaited.
    
    For example:
    
    ```python
    ...
    # get an awaitable for one step of the generator
    awaitable = anext(gen)
    # execute the one step of the generator and get the result
    result = await awaitable
    ```
    
    This can be achieved in one step.
    
    For example:
    
    ```python
    ...
    # step the async generator
    result = await anext(gen)
    ```

=== "Chinese"

    可以使用 [anext()](https://docs.python.org/3/library/functions.html#anext) 内置函数遍历生成器的一步，就像使用 **next()** 函数的经典生成器一样 。
    
    结果是一个值得期待的结果。
    
    例如:
    
    ```python
    ...
    # 获取生成器一步的等待值
    awaitable = anext(gen)
    # 执行生成器的一步并得到结果
    result = await awaitable
    ```
    
    这可以一步实现。
    
    例如:
    
    ```python
    ...
    # 启动异步生成器
    result = await anext(gen)
    ```

#### 16.2.4 遍历异步生成器

**16.2.4 Traverse an Asynchronous Generator**

=== "English"

    The asynchronous generator can also be traversed in a loop using the “**async for**” expression that will await each iteration of the loop automatically.
    
    For example:
    
    ```text
    ...
    # traverse an asynchronous generator
    async for result in async_generator():
        print(result)
    ```
    
    You can learn more about the “async for” expression in the tutorial:
    
    We may also use an asynchronous list comprehension with the “async for” expression to collect the results of the generator.
    
    For example:
    
    ```text
    ...
    # async list comprehension with async generator
    results = [item async for item in async_generator()]
    ```

=== "Chinese"

    还可以使用“**async for**”表达式在循环中遍历异步生成器，该表达式将自动等待循环的每次迭代。
    
    例如：
    
    ```text
    ...
    # 遍历异步生成器
    async for result in async_generator():
        print(result)
    ```
    
    您可以在教程中了解有关“async for”表达式的更多信息：
    
    我们还可以使用异步列表理解和“async for”表达式来收集生成器的结果。
    
    例如：
    
    ```text
    ...
    # 使用异步生成器的异步列表推导式
    results = [item async for item in async_generator()]
    ```

### 16.3 异步生成器示例

**16.3 Example of an Asynchronous Generator**

=== "English"

    We can explore how to traverse an asynchronous generator using the “**async for**” expression.
    
    In this example, we will update the previous example to traverse the generator to completion using an “**async for**” loop.
    
    This loop will automatically await each awaitable returned from the generator, retrieve the yielded value, and make it available within the loop body so that in this case it can be reported.
    
    This is perhaps the most common usage pattern for asynchronous generators.
    
    The complete example is listed below.
    
    ```python
    # SuperFastPython.com
    # example of asynchronous generator with async for loop
    import asyncio
     
    # define an asynchronous generator
    async def async_generator():
        # normal loop
        for i in range(10):
            # block to simulate doing work
            await asyncio.sleep(1)
            # yield the result
            yield i
     
    # main coroutine
    async def main():
        # loop over async generator with async for loop
        async for item in async_generator():
            print(item)
     
    # execute the asyncio program
    asyncio.run(main())
    ```
    
    Running the example first creates the **main()** coroutine and uses it as the entry point into the asyncio program.
    
    The **main()** coroutine runs and starts the for loop.
    
    An instance of the asynchronous generator is created and the loop automatically steps it using the **anext()** function to return an awaitable. The loop then awaits the awaitable and retrieves a value which is made available to the body of the loop where it is reported.
    
    This process is then repeated, suspending the main() coroutine, executing an iteration of the generator, and suspending, and resuming the **main()** coroutine until the generator is exhausted.
    
    This highlights how an asynchronous generator can be traversed using an async for expression.
    
    ```text
    0
    1
    2
    3
    4
    5
    6
    7
    8
    9
    ```
    
    You can learn more about async generators in the tutorial:
    
    - [Asynchronous Generators in Python](https://superfastpython.com/asynchronous-generators-in-python/)
    
    Next, we will explore asynchronous context managers.

=== "Chinese"

    我们可以探索如何使用“**async for**”表达式遍历异步生成器。
    
    在此示例中，我们将更新前面的示例，以使用“**async for**”循环遍历生成器直至完成。
    
    该循环将自动等待从生成器返回的每个等待，检索生成的值，并使其在循环体内可用，以便在这种情况下可以报告它。
    
    这可能是异步生成器最常见的使用模式。
    
    下面列出了完整的示例。
    
    ```python
    # SuperFastPython.com
    # 带有 async for 循环的异步生成器示例
    import asyncio
     
    # define an asynchronous generator
    async def async_generator():
        # 正常循环
        for i in range(10):
            # 块来模拟做工作
            await asyncio.sleep(1)
            # 产生结果
            yield i
     
    # 主协程
    async def main():
        # 使用 async for 循环遍历异步生成器
        async for item in async_generator():
            print(item)
     
    # 执行异步程序
    asyncio.run(main())
    ```
    
    运行该示例首先创建 **main()** 协程，并将其用作 asyncio 程序的入口点。
    
    **main()** 协程运行并启动 for 循环。
    
    创建异步生成器的实例，循环使用 **anext()** 函数自动步进它以返回可等待的对象。 然后，循环等待可等待对象并检索一个值，该值可供报告该值的循环体使用。
    
    然后重复这个过程，挂起 **main()** 协程，执行生成器的迭代，挂起并恢复 **main()** 协程，直到生成器耗尽。
    
    这突出显示了如何使用 **async for** 表达式遍历异步生成器。
    
    ```text
    0
    1
    2
    3
    4
    5
    6
    7
    8
    9
    ```
    
    您可以在教程中了解有关异步生成器的更多信息：
    
    - [Python 中的异步生成器](https://superfastpython.com/asynchronous-generators-in-python/)
    
    接下来，我们将探讨异步上下文管理器。

## 17. 异步上下文管理器

**17. Asynchronous Context Managers**

=== "English"

    A context manager is a Python construct that provides a try-finally like environment with a consistent interface and handy syntax, e.g. via the “with” expression.
    
    It is commonly used with resources, ensuring the resource is always closed or released after we are finished with it, regardless of whether the usage of the resources was successful or failed with an exception.
    
    Asyncio allows us to develop asynchronous context managers.
    
    We can create and use asynchronous context managers in asyncio programs by defining an object that implements the **\_\_aenter\_\_()** and **\_\_aexit\_\_()** methods as coroutines.
    
    Let’s take a closer look.

=== "Chinese"

    上下文管理器是一个 Python 结构，它提供了一个类似 try-finally 的环境，具有一致的接口和方便的语法，例如 通过“with”表达。
    
    它通常与资源一起使用，确保资源在使用完毕后始终关闭或释放，无论资源的使用是否成功或因异常而失败。
    
    Asyncio 允许我们开发异步上下文管理器。
    
    我们可以通过定义一个实现 **\_\_aenter\_\_()** 和 **\_\_aexit\_\_()** 方法的对象作为协程来在 asyncio 程序中创建和使用异步上下文管理器。
    
    让我们仔细看看。

### 17.1 什么是异步上下文管理器

**17.1 What is an Asynchronous Context Manager**

=== "English"

    An asynchronous context manager is a Python object that implements the **\_\_aenter\_\_()** and **\_\_aexit\_\ _()** methods.
    
    Before we dive into the details of asynchronous context managers, let’s review classical context managers.

=== "Chinese"

    异步上下文管理器是一个实现 **\_\_aenter\_\_()** 和 **\_\_aexit\_\_()** 方法的 Python 对象。
    
    在我们深入了解异步上下文管理器的细节之前，让我们回顾一下经典的上下文管理器。

#### 17.1.1  上下文管理器

**17.1.1  Context Manager**

=== "English"

    A context manager is a Python object that implements the \_\_enter\_\_() and \_\_exit\_\_() methods.
    
    > A context manager is an object that defines the runtime context to be established when executing a with statement. The context manager handles the entry into, and the exit from, the desired runtime context for the execution of the block of code.
    >
    > — [WITH STATEMENT CONTEXT MANAGERS](https://docs.python.org/3/reference/datamodel.html#context-managers)
    
    - The \_\_enter\_\_() method defines what happens at the beginning of a block, such as opening or preparing resources, like a file, socket or thread pool.
    - The \_\_exit\_\_() method defines what happens when the block is exited, such as closing a prepared resource.
    
    > Typical uses of context managers include saving and restoring various kinds of global state, locking and unlocking resources, closing opened files, etc.
    >
    > — [WITH STATEMENT CONTEXT MANAGERS](https://docs.python.org/3/reference/datamodel.html#context-managers)
    
    A context manager is used via the “with” expression.
    
    Typically the context manager object is created in the beginning of the “with” expression and the \_\_enter\_\_() method is called automatically. The body of the content makes use of the resource via the named context manager object, then the \_\_aexit\_\_() method is called automatically when the block is exited, normally or via an exception.
    
    For example:
    
    ```python
    ...
    # open a context manager
    with ContextManager() as manager:
        # ...
    # closed automatically
    This mirrors a try-finally expression.
    ```
    
    For example:
    
    ```python
    ...
    # create the object
    manager = ContextManager()
    try:
        manager.__enter__()
        # ...
    finally:
        manager.__exit__()
    ```
    
    Next, let’s take a look at asynchronous context managers.

=== "Chinese"

    上下文管理器是一个实现 \_\_enter\_\_() 和 \_\_exit\_\_() 方法的 Python 对象。
    
    > 上下文管理器是一个对象，它定义执行 with 语句时要建立的运行时上下文。 上下文管理器处理执行代码块所需的运行时上下文的进入和退出。
    >
    > — [WITH STATEMENT CONTEXT MANAGERS](https://docs.python.org/3/reference/datamodel.html#context-managers)
    
    - \_\_enter\_\_() 方法定义在块开始时发生的情况，例如打开或准备资源，如文件、套接字或线程池。
    - \_\_exit\_\_() 方法定义退出块时会发生什么，例如关闭准备好的资源。
    
    > 上下文管理器的典型用途包括保存和恢复各种全局状态、锁定和解锁资源、关闭打开的文件等。
    >
    > — [WITH STATEMENT CONTEXT MANAGERS](https://docs.python.org/3/reference/datamodel.html#context-managers)
    
    上下文管理器通过“with”表达式使用。
    
    通常，上下文管理器对象是在“with”表达式的开头创建的，并且自动调用 \_\_enter\_\_() 方法。 内容的主体通过指定的上下文管理器对象使用资源，然后当块正常或通过异常退出时，自动调用 \_\_aexit\_\_() 方法。
    
    例如:
    
    ```python
    ...
    # 打开上下文管理器
    with ContextManager() as manager:
        # ...
    # 自动关闭
    This mirrors a try-finally expression.
    ```
    
    例如:
    
    ```python
    ...
    # 创建对象
    manager = ContextManager()
    try:
        manager.__enter__()
        # ...
    finally:
        manager.__exit__()
    ```
    
    接下来，让我们看一下异步上下文管理器。

#### 17.1.2 异步上下文管理器

**17.1.2  Asynchronous Context Manager**

=== "English"

    Asynchronous context managers were introduced in “[PEP 492 – Coroutines with async and await syntax](https://peps.python.org/pep-0492/)“.
    
    They provide a context manager that can be suspended when entering and exiting.
    
    > An asynchronous context manager is a context manager that is able to suspend execution in its \_\_aenter\_\_ and \_\_aexit\_\_ methods.
    >
    > — ASYNCHRONOUS CONTEXT MANAGERS
    
    The \_\_aenter\_\_ and \_\_aexit\_\_ methods are defined as coroutines and are awaited by the caller.
    
    This is achieved using the “**async with**” expression.
    
    You can learn more about the “**async with**” expression in the tutorial:
    
    - [What is Asyncio async with](https://superfastpython.com/asyncio-async-with/)
    
    As such, asynchronous context managers can only be used within asyncio programs, such as within calling coroutines.
    
    What is “async with”
    
    The “**async with**” expression is for creating and using asynchronous context managers.
    
    It is an extension of the “**with**” expression for use in coroutines within asyncio programs.
    
    The “**async with**” expression is just like the “**with**” expression used for context managers, except it allows asynchronous context managers to be used within coroutines.
    
    In order to better understand “**async with**“, let’s take a closer look at asynchronous context managers.
    
    The async with expression allows a coroutine to create and use an asynchronous version of a context manager.
    
    For example:
    
    ```python
    ...
    # create and use an asynchronous context manager
    async with AsyncContextManager() as manager:
        # ...
    ```
    
    This is equivalent to something like:
    
    ```python
    ...
    # create or enter the async context manager
    manager = await AsyncContextManager()
    try:
        # ...
    finally:
        # close or exit the context manager
        await manager.close()
    ```
    
    Notice that we are implementing much the same pattern as a traditional context manager, except that creating and closing the context manager involve awaiting coroutines.
    
    This suspends the execution of the current coroutine, schedules a new coroutine and waits for it to complete.
    
    As such an asynchronous context manager must implement the **\_\_aenter\_\_()** and **\_\_aexit\_\_()** methods that must be defined via the async def expression. This makes them coroutines themselves which may also await.

=== "Chinese"

    异步上下文管理器在“[PEP 492 – 具有异步和等待语法的协程](https://peps.python.org/pep-0492/)”中引入。
    
    它们提供了一个上下文管理器，可以在进入和退出时暂停。
    
    > 异步上下文管理器是能够在其 \_\_aenter\_\_ 和 \_\_aexit\_\_ 方法中暂停执行的上下文管理器。
    >
    > — [ASYNCHRONOUS CONTEXT MANAGERS](https://docs.python.org/3/reference/datamodel.html#asynchronous-context-managers)
    
    **\_\_aenter\_\_** 和 **\_\_aexit\_\_** 方法被定义为协程并由调用者等待。
    
    这是使用“**async with**”表达式来实现的。
    
    您可以在教程中了解有关“**async with**”表达式的更多信息：
    
    - [Asyncio 异步是什么](https://superfastpython.com/asyncio-async-with/)
    
    因此，异步上下文管理器只能在 asyncio 程序中使用，例如在调用协程中。
    
    什么是“异步”
    
    “**async with**”表达式用于创建和使用异步上下文管理器。
    
    它是“**with**”表达式的扩展，用于 asyncio 程序中的协程。
    
    “**async with**”表达式就像用于上下文管理器的“**with**”表达式一样，只不过它允许在协程中使用异步上下文管理器。
    
    为了更好地理解“**async with**”，让我们仔细看看异步上下文管理器。
    
    async with 表达式允许协程创建和使用上下文管理器的异步版本。
    
    例如:
    
    ```python
    ...
    # 创建并使用异步上下文管理器
    async with AsyncContextManager() as manager:
        # ...
    ```
    
    这相当于：
    
    ```python
    ...
    # 创建或进入异步上下文管理器
    manager = await AsyncContextManager()
    try:
        # ...
    finally:
        # 关闭或退出上下文管理器
        await manager.close()
    ```
    
    请注意，我们实现的模式与传统上下文管理器几乎相同，只是创建和关闭上下文管理器涉及等待协程。
    
    这会暂停当前协程的执行，安排一个新的协程并等待其完成。
    
    因此，异步上下文管理器必须实现 **\_\_aenter\_\_()** 和 **\_\_aexit\_\_()** 方法，这些方法必须通过 async def 表达式定义。 这使得它们本身成为协程，也可能等待。

### 17.2 如何使用异步上下文管理器

**17.2 How to Use Asynchronous Context Managers**

=== "English"

    In this section, we will explore how we can define, create, and use asynchronous context managers in our asyncio programs.

=== "Chinese"

    在本节中，我们将探讨如何在 asyncio 程序中定义、创建和使用异步上下文管理器。

#### 17.2.1 定义异步上下文管理器

**17.2.1 Define an Asynchronous Context Manager**

=== "English"

    We can define an asynchronous context manager as a Python object that implements the **__aenter__()** and **__aexit__()** methods.
    
    Importantly, both methods must be defined as coroutines using the “**async def**” and therefore must return awaitables.
    
    For example:

    ```python
    # define an asynchronous context manager
    class AsyncContextManager:
        # enter the async context manager
        async def __aenter__(self):
            # report a message
            print('>entering the context manager')
     
        # exit the async context manager
        async def __aexit__(self, exc_type, exc, tb):
            # report a message
            print('>exiting the context manager')
    ```

    Because each of the methods are coroutines, they may themselves await coroutines or tasks.
    
    For example:

    ```python
    # define an asynchronous context manager
    class AsyncContextManager:
        # enter the async context manager
        async def __aenter__(self):
            # report a message
            print('>entering the context manager')
            # block for a moment
            await asyncio.sleep(0.5)
     
        # exit the async context manager
        async def __aexit__(self, exc_type, exc, tb):
            # report a message
            print('>exiting the context manager')
            # block for a moment
            await asyncio.sleep(0.5)
    ```

=== "Chinese"

    我们可以将异步上下文管理器定义为实现 **__aenter__()** 和 **__aexit__()** 方法的 Python 对象。
    
    重要的是，这两种方法都必须使用“**async def**”定义为协程，因此必须返回可等待对象。
    
    例如:

    ```python
    # 定义异步上下文管理器
    class AsyncContextManager:
        # 进入异步上下文管理器
        async def __aenter__(self):
            # 报告消息
            print('>entering the context manager')
     
        # 退出异步上下文管理器
        async def __aexit__(self, exc_type, exc, tb):
            # 报告消息
            print('>exiting the context manager')
    ```

    因为每个方法都是协程，所以它们本身可能等待协程或任务。
    
    例如:

    ```python
    # 定义异步上下文管理器
    class AsyncContextManager:
        # 进入异步上下文管理器
        async def __aenter__(self):
            # 报告消息
            print('>entering the context manager')
            # 暂时阻塞
            await asyncio.sleep(0.5)
     
        # 退出异步上下文管理器
        async def __aexit__(self, exc_type, exc, tb):
            # 报告消息
            print('>exiting the context manager')
            # 暂时阻塞
            await asyncio.sleep(0.5)
    ```

#### 17.2.2 使用异步上下文管理器

**17.2.2 Use an Asynchronous Context Manager**

=== "English"

    An asynchronous context manager is used via the “**async with**” expression.
    
    This will automatically await the enter and exit coroutines, suspending the calling coroutine as needed.
    
    For example:
    
    ```python
    ...
    # use an asynchronous context manager
    async with AsyncContextManager() as manager:
        # ...
    ```

    As such, the “**async with**” expression and asynchronous context managers more generally can only be used within asyncio programs, such as within coroutines.
    
    Now that we know how to use asynchronous context managers, let’s look at a worked example.

=== "Chinese"

    异步上下文管理器通过“**async with**”表达式使用。
    
    这将自动等待进入和退出协程，并根据需要暂停调用协程。
    
    例如：
    
    ```python
    ...
    # 使用异步上下文管理器
    async with AsyncContextManager() as manager:
        # ...
    ```

    因此，“**async with**”表达式和异步上下文管理器更一般地只能在 asyncio 程序中使用，例如在协程中。
    
    现在我们知道如何使用异步上下文管理器，让我们看一个有效的示例。

### 17.3 异步上下文管理器和“async with”的示例

**17.3 Example of an Asynchronous Context Manager and “async with”**

=== "English"

    We can explore how to use an asynchronous context manager via the “**async with**” expression.
    
    In this example, we will update the above example to use the context manager in a normal manner.
    
    We will use an “**async with**” expression and on one line, create and enter the context manager. This will automatically await the enter method.
    
    We can then make use of the manager within the inner block. In this case, we will just report a message.
    
    Exiting the inner block will automatically await the exit method of the context manager.
    
    Contrasting this example with the previous example shows how much heavy lifting the “**async with**” expression does for us in an asyncio program.
    
    The complete example is listed below.
    
    ```python
    # SuperFastPython.com
    # example of an asynchronous context manager via async with
    import asyncio
     
    # define an asynchronous context manager
    class AsyncContextManager:
        # enter the async context manager
        async def __aenter__(self):
            # report a message
            print('>entering the context manager')
            # block for a moment
            await asyncio.sleep(0.5)
     
        # exit the async context manager
        async def __aexit__(self, exc_type, exc, tb):
            # report a message
            print('>exiting the context manager')
            # block for a moment
            await asyncio.sleep(0.5)
     
    # define a simple coroutine
    async def custom_coroutine():
        # create and use the asynchronous context manager
        async with AsyncContextManager() as manager:
            # report the result
            print(f'within the manager')
     
    # start the asyncio program
    asyncio.run(custom_coroutine())
    ```
    
    Running the example first creates the **main()** coroutine and uses it as the entry point into the asyncio program.
    
    The **main()** coroutine runs and creates an instance of our **AsyncContextManager** class in an “**async with**” expression.
    
    This expression automatically calls the enter method and awaits the coroutine. A message is reported and the coroutine blocks for a moment.
    
    The **main()** coroutine resumes and executes the body of the context manager, printing a message.
    
    The block is exited and the exit method of the context manager is awaited automatically, reporting a message and sleeping a moment.
    
    This highlights the normal usage pattern for an asynchronous context manager in an asyncio program.
    
    ```python
    >entering the context manager
    within the manager
    >exiting the context manager
    ```
    
    You can learn more about async context managers in the tutorial:
    
    - [Asynchronous Context Managers in Python](https://superfastpython.com/asynchronous-context-manager/)
    
    Next, we will explore asynchronous comprehensions.

=== "Chinese"

    我们可以通过“**async with**”表达式探索如何使用异步上下文管理器。
    
    在此示例中，我们将更新上面的示例以正常方式使用上下文管理器。
    
    我们将使用“**async with**”表达式，并在一行中创建并输入上下文管理器。 这将自动等待输入方法。
    
    然后我们可以在内部块中使用管理器。 在这种情况下，我们只会报告一条消息。
    
    退出内部块将自动等待上下文管理器的退出方法。
    
    将此示例与前面的示例进行对比，可以看出“**async with**”表达式在 asyncio 程序中为我们带来了多大的负担。
    
    下面列出了完整的示例。
    
    ```python
    # SuperFastPython.com
    # 通过 async with 实现异步上下文管理器的示例
    import asyncio
     
    # 定义异步上下文管理器
    class AsyncContextManager:
        # 进入异步上下文管理器
        async def __aenter__(self):
            # 报告消息
            print('>entering the context manager')
            # 暂时阻塞
            await asyncio.sleep(0.5)
     
        # 退出异步上下文管理器
        async def __aexit__(self, exc_type, exc, tb):
            # 报告消息
            print('>exiting the context manager')
            # 暂时阻塞
            await asyncio.sleep(0.5)
     
    # 定义一个简单的协程
    async def custom_coroutine():
        # 创建并使用异步上下文管理器
        async with AsyncContextManager() as manager:
            # 报告结果
            print(f'within the manager')
     
    # 启动异步程序
    asyncio.run(custom_coroutine())
    ```
    
    运行该示例首先创建 **main()** 协程，并将其用作 asyncio 程序的入口点。
    
    **main()** 协程运行并在“**async with**”表达式中创建 **AsyncContextManager** 类的实例。
    
    该表达式自动调用 Enter 方法并等待协程。 报告一条消息，协程阻塞片刻。
    
    **main()** 协程恢复并执行上下文管理器的主体，打印一条消息。
    
    该块退出并自动等待上下文管理器的退出方法，报告消息并休眠一会儿。
    
    这突出显示了 asyncio 程序中异步上下文管理器的正常使用模式。
    
    ```python
    >entering the context manager
    within the manager
    >exiting the context manager
    ```
    
    您可以在教程中了解有关异步上下文管理器的更多信息：
    
    - [Asynchronous Context Managers in Python](https://superfastpython.com/asynchronous-context-manager/)
    
    接下来，我们将探索异步推导式。

## 18. 异步推导式

**18. Asynchronous Comprehensions**

=== "English"

    Comprehensions, like list and dict comprehensions are one feature of Python when we think of “pythonic“.
    
    It is a way we do loops that is different to many other languages.
    
    Asyncio allows us to use asynchronous comprehensions.
    
    We can traverse an asynchronous generators and asynchronous iterators using an asynchronous comprehension via the “**async for**” expression.
    
    Let’s take a closer look.

=== "Chinese"

    当我们想到“Pythonic”时，推导式（例如列表推导式和字典推导式）是 Python 的特征之一。
    
    这是我们执行循环的一种与许多其他语言不同的方式。
    
    Asyncio 允许我们使用异步推导式。
    
    我们可以通过“**async for**”表达式使用异步推导式来遍历异步生成器和异步迭代器。
    
    让我们仔细看看。

### 18.1 什么是异步推导式

**18.1 What are Asynchronous Comprehensions**

=== "English"

    An async comprehension is an asynchronous version of a classical comprehension.
    
    Asyncio supports two types of asynchronous comprehensions, they are the “async for” comprehension and the “await” comprehension.
    
    > PEP 530 adds support for using async for in list, set, dict comprehensions and generator expressions
    >
    > — [PEP 530: ASYNCHRONOUS COMPREHENSIONS, WHAT’S NEW IN PYTHON 3.6.](https://docs.python.org/3/whatsnew/3.6.html#pep-530-asynchronous-comprehensions)
    
    Before we look at each, let’s first recall classical comprehensions.

=== "Chinese"

    异步理解是经典理解的异步版本。
    
    Asyncio 支持两种类型的异步理解，它们是“async for”推导和“await”推导。
    
    > PEP 530 添加了对在 列表、集合、字典理解和生成器表达式使用异步的支持
    >
    > — [PEP 530: ASYNCHRONOUS COMPREHENSIONS, WHAT’S NEW IN PYTHON 3.6.](https://docs.python.org/3/whatsnew/3.6.html#pep-530-asynchronous-comprehensions)
    
    在我们讨论每一个之前，让我们首先回顾一下经典的推导式。

### 18.2 推导式

**18.2 Comprehensions**

=== "English"

    Comprehensions allow data collections like lists, dicts, and sets to be created in a concise way.
    
    > List comprehensions provide a concise way to create lists.
    >
    > — [LIST COMPREHENSIONS](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions)
    
    A list comprehension allows a list to be created from a for expression within the new list expression.
    
    For example:
    
    ```python
    ...
    # create a list using a list comprehension
    result = [a*2 for a in range(100)]
    ```
    
    Comprehensions are also supported for creating dicts and sets.
    
    For example:
    
    ```python
    ...
    # create a dict using a comprehension
    result = {a:i for a,i in zip(['a','b','c'],range(3))}
    # create a set using a comprehension
    result = {a for a in [1, 2, 3, 2, 3, 1, 5, 4]}
    ```

=== "Chinese"

    推导式允许以简洁的方式创建列表、字典和集合等数据集合。
    
    > 列表推导式提供了一种创建列表的简洁方法。
    >
    > — [LIST COMPREHENSIONS](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions)
    
    列表推导式允许从新列表表达式中的 for 表达式创建列表。
    
    例如:
    
    ```python
    ...
    # 使用列表理解创建列表
    result = [a*2 for a in range(100)]
    ```
    
    还支持创建字典和集合的推导式。
    
    例如:
    
    ```python
    ...
    # 使用理解创建一个字典
    result = {a:i for a,i in zip(['a','b','c'],range(3))}
    # 使用推导式创建一个集合
    result = {a for a in [1, 2, 3, 2, 3, 1, 5, 4]}
    ```

### 18.3 异步推导式

**18.3 Asynchronous Comprehensions**

=== "English"

    An asynchronous comprehension allows a list, set, or dict to be created using the “async for” expression with an asynchronous iterable.
    
    > We propose to allow using async for inside list, set and dict comprehensions.
    >
    > — PEP 530 – ASYNCHRONOUS COMPREHENSIONS
    
    For example:
    
    ```python
    ...
    # async list comprehension with an async iterator
    result = [a async for a in aiterable]
    ```
    
    This will create and schedule coroutines or tasks as needed and yield their results into a list.
    
    Recall that the “[async for](https://superfastpython.com/asyncio-async-for/)” expression may only be used within coroutines and tasks.
    
    Also, recall that an asynchronous iterator is an iterator that yields awaitables.
    
    The “**async for**” expression allows the caller to traverse an asynchronous iterator of awaitables and retrieve the result from each.
    
    Internally, the async for loop will automatically resolve or await each awaitable, scheduling coroutines as needed.
    
    An async generator automatically implements the methods for the async iterator and may also be used in an asynchronous comprehension.
    
    For example:
    
    ```python
    ...
    # async list comprehension with an async generator
    result = [a async for a in agenerator]
    ```

=== "Chinese"

    异步理解允许使用带有异步迭代的“**async for**”表达式来创建列表、集合或字典。
    
    > 我们建议允许对内部列表、集合和字典理解使用异步。
    >
    > — PEP 530 – ASYNCHRONOUS COMPREHENSIONS
    
    例如:
    
    ```python
    ...
    # 使用异步迭代器的异步列表理解
    result = [a async for a in aiterable]
    ```
    
    这将根据需要创建和调度协程或任务，并将其结果生成到列表中。
    
    回想一下，“[async for](https://superfastpython.com/asyncio-async-for/)”表达式只能在协程和任务中使用。
    
    另外，请记住，异步迭代器是产生可等待项的迭代器。
    
    “**async for**”表达式允许调用者遍历可等待项的异步迭代器并从每个迭代器中检索结果。
    
    在内部，async for 循环将自动解析或等待每个可等待的、根据需要调度协程。
    
    异步生成器自动实现异步迭代器的方法，也可以在异步推导式中使用。
    
    例如:
    
    ```python
    ...
    # 使用异步生成器的异步列表推导式
    result = [a async for a in agenerator]
    ```

### 18.4 Await 推导式

**18.4 Await Comprehensions**

=== "English"

    The **“await”** expression may also be used within a list, set, or dict comprehension, referred to as an await comprehension.
    
    > We propose to allow the use of await expressions in both asynchronous and synchronous comprehensions
    >
    > — [PEP 530 – ASYNCHRONOUS COMPREHENSIONS](https://peps.python.org/pep-0530/)
    
    Like an async comprehension, it may only be used within an asyncio coroutine or task.
    
    This allows a data structure, like a list, to be created by suspending and awaiting a series of awaitables.
    
    For example:
    
    ```python
    ...
    # await list compression with a collection of awaitables
    results = [await a for a in awaitables]
    ```
    
    This will create a list of results by awaiting each awaitable in turn.
    
    The current coroutine will be suspended to execute awaitables sequentially, which is different and perhaps slower than executing them concurrently using **asyncio.gather()**.
    
    You can learn more about async comprehensions in the tutorial:
    
    - [Asynchronous Comprehensions in Python](https://superfastpython.com/asynchronous-comprehensions/)
    
    Next, we will explore how to run commands using subprocesses from asyncio.

=== "Chinese"

    **“await”** 表达式也可以在列表、集合或字典理解中使用，称为**await 推导**。
    
    > 我们建议在异步和同步中都使用**await推导**或**列表推导**
    >
    > — [PEP 530 – ASYNCHRONOUS COMPREHENSIONS](https://peps.python.org/pep-0530/)
    
    与异步理解一样，它只能在异步协程或任务中使用。
    
    这允许通过挂起和等待一系列可等待项来创建数据结构，例如列表。
    
    例如:
    
    ```python
    ...
    # 在可等待对象合集中使用await列表推导
    results = [await a for a in awaitables]
    ```
    
    这将通过依次等待每个可等待项来创建结果列表。
    
    当前协程将被挂起以顺序执行可等待项，这与使用 **asyncio.gather()** 并发执行它们不同，并且可能更慢。
    
    您可以在教程中了解有关异步推导的更多信息：
    
    - [Python 中的异步推导式](https://superfastpython.com/asynchronous-comprehensions/)
    
    接下来，我们将探索如何使用 asyncio 中的子进程来运行命令。

## 19. 在非阻塞子进程中运行命令

**19. Run Commands in Non-Blocking Subprocesses**

=== "English"

    We can execute commands from asyncio.
    
    The command will run in a subprocess that we can write to and read from using non-blocking I/O.
    
    Let’s take a closer look.

=== "Chinese"

    We can execute commands from asyncio.
    
    The command will run in a subprocess that we can write to and read from using non-blocking I/O.
    
    Let’s take a closer look.

### 19.1 什么是 asyncio.subprocess.Process

**19.1 What is asyncio.subprocess.Process**

=== "English"

    The [asyncio.subprocess.Process](https://docs.python.org/3/library/asyncio-subprocess.html#asyncio.subprocess.Process) class provides a representation of a subprocess run by asyncio.
    
    It provides a handle on a subprocess in asyncio programs, allowing actions to be performed on it, such as waiting and terminating it.
    
    > Process is a high-level wrapper that allows communicating with subprocesses and watching for their completion.
    >
    > — [INTERACTING WITH SUBPROCESSES](https://docs.python.org/3/library/asyncio-subprocess.html#interacting-with-subprocesses)
    
    The API is very similar to the [multiprocessing.Process](https://docs.python.org/3/library/subprocess.html#subprocess.Popen) class and perhaps more so with the [subprocess.Popen](https://docs.python.org/3/library/subprocess.html#subprocess.Popen) class.
    
    Specifically, it shares methods such as **wait()**, **communicate()**, and **send_signal()** and attributes such as stdin, stdout, and stderr with the subprocess.Popen.
    
    Now that we know what the **asyncio.subprocess.Process** class is, let’s look at how we might use it in our asyncio programs.
    
    We do not create a **asyncio.subprocess.Process** directly.
    
    Instead, an instance of the class is created for us when executing a subprocess in an asyncio program.
    
    > An object that wraps OS processes created by the create_subprocess_exec() and create_subprocess_shell() functions.
    >
    > — [INTERACTING WITH SUBPROCESSES](https://docs.python.org/3/library/asyncio-subprocess.html#interacting-with-subprocesses)
    
    There are two ways to execute an external program as a subprocess and acquire a Process instance, they are:
    
    - asyncio.create_subprocess_exec() for running commands directly.
    - asyncio.create_subprocess_shell() for running commands via the shell.

    Let’s look at examples of each in turn.

=== "Chinese"

    asyncio通过[asyncio.subprocess.Process](https://docs.python.org/3/library/asyncio-subprocess.html#asyncio.subprocess.Process) 类提供了对于运行子进程的支持和表示。
    
    它提供了 asyncio 程序中子进程的句柄，允许对其执行操作，例如等待和终止它。
    
    > Process是一个高级包装过后的类，允许与子进程通信并监视其完成情况。
    >
    > — [INTERACTING WITH SUBPROCESSES](https://docs.python.org/3/library/asyncio-subprocess.html#interacting-with-subprocesses)
    
    该 API 与 [multiprocessing.Process](https://docs.python.org/3/library/subprocess.html#subprocess.Popen) 类非常相似，也许与 [subprocess.Popen](https://docs.python.org/3/library/subprocess.html#subprocess.Popen) 类更新相似。
    
    具体来说，它与 **subprocess.Popen** 共享 **wait()**、**communicate()** 和 **send_signal()** 等方法以及 `stdin`、`stdout` 和 `stderr` 等属性。
    
    现在我们知道了 **asyncio.subprocess.Process** 类是什么，让我们看看如何在 asyncio 程序中使用它。
    
    我们不直接创建 **asyncio.subprocess.Process**。
    
    相反，当在 asyncio 程序中执行子进程时，会为我们创建该类的实例。
    
    > 包装由 **create_subprocess_exec()** 和 **create_subprocess_shell()** 函数创建的操作系统进程的对象。
    >
    > — [INTERACTING WITH SUBPROCESSES](https://docs.python.org/3/library/asyncio-subprocess.html#interacting-with-subprocesses)
    
    有两种方法可以将外部程序作为子进程执行并获取 Process 实例，它们是：
    
    - **asyncio.create_subprocess_exec()** 用于直接运行命令。
    - **asyncio.create_subprocess_shell()** 用于通过 shell 运行命令。

    让我们依次看一下每个例子。

### 19.2 如何直接运行命令

**19.2 How to Run a Command Directly**

=== "English"

    A [command](https://en.wikipedia.org/wiki/Command-line_interface) is a program executed on the command line (terminal or command prompt). It is another program that is run directly.
    
    Common examples on Linux and macOS might be:
    
    - ‘ls‘ to list the contents of a directory
    - ‘cat‘ to report the content of a file
    - ‘date‘ to report the date
    - ‘echo‘ to report back a string
    - ‘sleep‘ to sleep for a number of seconds
    
    And so on.
    
    We can execute a command from an asyncio program via the **create_subprocess_exec()** function.
    
    The **asyncio.create_subprocess_exec()** function takes a command and executes it directly.
    
    This is helpful as it allows the command to be executed in a subprocess and for asyncio coroutines to read, write, and wait for it.
    
    > Because all asyncio subprocess functions are asynchronous and asyncio provides many tools to work with such functions, it is easy to execute and monitor multiple subprocesses in parallel.
    >
    > — ASYNCIO SUBPROCESSES
    
    Unlike the **asyncio.create_subprocess_shell()** function, the **asyncio.create_subprocess_exec()** will not execute the command using the shell.
    
    This means that the capabilities provided by the shell, such as shell variables, scripting, and wildcards are not available when executing the command.
    
    It also means that executing the command may be more secure as there is no opportunity for a [shell injection](https://en.wikipedia.org/wiki/Code_injection#Shell_injection).
    
    Now that we know what **asyncio.create_subprocess_exec()** does, let’s look at how to use it.

=== "Chinese"

    [command](https://en.wikipedia.org/wiki/Command-line_interface) 是在命令行（终端或命令提示符）上执行的程序。 这是另一个直接运行的程序。
    
    Linux 和 macOS 上的常见示例可能是：
    
    - ‘ls‘ 列出目录的内容
    - ‘cat‘ 报告文件的内容
    - ‘date‘ 报告日期
    - ‘echo‘ 报告一个字符串
    - ‘sleep‘ 睡眠几秒钟
    
    等等。
    
    我们可以通过 **create_subprocess_exec()** 函数从 asyncio 程序执行命令。
    
    **asyncio.create_subprocess_exec()** 函数接受一个命令并直接执行它。
    
    这很有用，因为它允许在子进程中执行命令，并允许异步协程读取、写入和等待它。
    
    > 因为所有 asyncio 子进程函数都是异步的，并且 asyncio 提供了许多工具来使用这些函数，所以很容易并行执行和监视多个子进程。
    >
    > — [ASYNCIO SUBPROCESSES](https://docs.python.org/3/library/asyncio-subprocess.html)
    
    与 **asyncio.create_subprocess_shell()** 函数不同，**asyncio.create_subprocess_exec()** 不会使用 shell 执行命令。
    
    这意味着 shell 提供的功能（例如 shell 变量、脚本和通配符）在执行命令时不可用。
    
    这也意味着执行命令可能更安全，因为没有机会进行[shell注入](https://en.wikipedia.org/wiki/Code_injection#Shell_injection)。
    
    现在我们知道了 **asyncio.create_subprocess_exec()** 的作用，让我们看看如何使用它。

#### 19.2.1 如何使用 Asyncio 的 create_subprocess_exec()

**19.2.1 How to Use Asyncio create_subprocess_exec()**

=== "English"

    The [asyncio.create_subprocess_exec()](https://docs.python.org/3/library/asyncio-subprocess.html#asyncio.create_subprocess_exec) function will execute a given string command in a subprocess.
    
    It returns a [asyncio.subprocess.Process](https://docs.python.org/3/library/asyncio-subprocess.html#asyncio.subprocess.Process) object that represents the subprocess.
    
    > Process is a high-level wrapper that allows communicating with subprocesses and watching for their completion.
    >
    > — [INTERACTING WITH SUBPROCESSES](https://docs.python.org/3/library/asyncio-subprocess.html#asyncio.subprocess.Process)
    
    The create_subprocess_exec() function is a coroutine, which means we must await it. It will return once the subprocess has been started, not when the subprocess is finished.
    
    For example:
    
    ```python
    ...
    # execute a command in a subprocess
    process = await asyncio.create_subprocess_exec('ls')
    ```
    
    Arguments to the command being executed must be provided as subsequent arguments to the **create_subprocess_exec()** function.
    
    For example:
    
    ```python
    ...
    # execute a command with arguments in a subprocess
    process = await asyncio.create_subprocess_exec('ls', '-l')
    ```
    
    We can wait for the subprocess to finish by awaiting the **wait()** method.
    
    For example:
    
    ```python
    ...
    # wait for the subprocess to terminate
    await process.wait()
    ```
    
    We can stop the subprocess directly by calling the **terminate()** or **kill()** methods, which will raise a signal in the subprocess.
    
    For example:
    
    ```python
    ...
    # terminate the subprocess
    process.terminate()
    ```
    
    The input and output of the command will be handled by **stdin**, **stderr**, and **stdout**.
    
    We can have the asyncio program handle the input or output for the subprocess.
    
    This can be achieved by specifying the input or output stream and specifying a constant to redirect, such as **asyncio.subprocess.PIPE**.
    
    For example, we can redirect the output of a command to the asyncio program:
    
    ```python
    ...
    # start a subprocess and redirect output
    process = await asyncio.create_subprocess_exec('ls', stdout=asyncio.subprocess.PIPE)
    ```
    
    We can then read the output of the program via the **asyncio.subprocess.Process** instance via the **communicate()** method.
    
    This method is a coroutine and must be awaited. It is used to both send and receive data with the subprocess.
    
    For example:
    
    ```python
    ...
    # read data from the subprocess
    line = process.communicate()
    ```
    
    We can also send data to the subprocess via the **communicate()** method by setting the **“input”** argument in bytes.
    
    For example:
    
    ```python
    ...
    # start a subprocess and redirect input
    process = await asyncio.create_subprocess_exec('ls', stdin=asyncio.subprocess.PIPE)
    # send data to the subprocess
    process.communicate(input=b'Hello\n')
    ```
    
    Behind the scenes the **asyncio.subprocess.PIPE** configures the subprocess to point to a **StreamReader** or **StreamWriter** for sending data to or from the subprocess, and the **communicate()** method will read or write bytes from the configured reader.
    
    > If PIPE is passed to stdin argument, the Process.stdin attribute will point to a StreamWriter instance. If PIPE is passed to stdout or stderr arguments, the Process.stdout and Process.stderr attributes will point to StreamReader instances.
    >
    > — [ASYNCIO SUBPROCESSES](https://docs.python.org/3/library/asyncio-subprocess.html)
    
    We can interact with the **StreamReader** or **StreamWriter** directly via the subprocess via the stdin, stdout, and stderr attributes.
    
    For example:
    
    ```python
    ...
    # read a line from the subprocess output stream
    line = await process.stdout.readline()
    ```
    
    Now that we know how to use the **create_subprocess_exec()** function, let’s look at some worked examples.

=== "Chinese"

    [asyncio.create_subprocess_exec()](https://docs.python.org/3/library/asyncio-subprocess.html#asyncio.create_subprocess_exec) 函数将在子进程中执行给定的字符串命令。
    
    它返回一个表示子进程的 [asyncio.subprocess.Process](https://docs.python.org/3/library/asyncio-subprocess.html#asyncio.subprocess.Process) 对象。
    
    > Process是一个高级包装器，允许与子进程通信并监视其完成情况。
    >
    > — [INTERACTING WITH SUBPROCESSES](https://docs.python.org/3/library/asyncio-subprocess.html#asyncio.subprocess.Process)
    
    create_subprocess_exec() 函数是一个协程，这意味着我们必须等待它。 它会在子进程启动后返回，而不是在子进程完成时返回。
    
    例如:
    
    ```python
    ...
    # 在子进程中执行命令
    process = await asyncio.create_subprocess_exec('ls')
    ```
    
    正在执行的命令的参数必须作为 **create_subprocess_exec()** 函数的后续参数提供。
    
    例如:
    
    ```python
    ...
    # 在子进程中执行带参数的命令
    process = await asyncio.create_subprocess_exec('ls', '-l')
    ```
    
    我们可以通过等待 **wait()** 方法来等待子进程完成。
    
    例如:
    
    ```python
    ...
    # 等待子进程终止
    await process.wait()
    ```
    
    我们可以通过调用 **terminate()** 或 **kill()** 方法直接停止子进程，这将在子进程中引发一个信号。
    
    例如：
    
    ```python
    ...
    # 终止子进程
    process.terminate()
    ```
    
    命令的输入和输出将由 **stdin**、**stderr** 和 **stdout** 处理。
    
    我们可以让 asyncio 程序处理子进程的输入或输出。
    
    这可以通过指定输入或输出流并指定要重定向的常量来实现，例如 **asyncio.subprocess.PIPE**。
    
    例如，我们可以将命令的输出重定向到 asyncio 程序：

    ```python
    ...
    # 启动子进程并重定向输出
    process = await asyncio.create_subprocess_exec('ls', stdout=asyncio.subprocess.PIPE)
    ```
    
    然后我们可以通过**asyncio.subprocess.Process**实例通过**communicate()**方法读取程序的输出。
    
    该方法是一个协程，必须等待。 它用于通过子进程发送和接收数据。
    
    例如：

    ```python
    ...
    # 从子进程读取数据
    line = process.communicate()
    ```
    
    我们还可以通过 **communicate()** 方法通过设置 **“input”** 参数（以字节为单位）将数据发送到子进程。
    
    例如：

    ```python
    ...
    # 启动子进程并重定向输入
    process = await asyncio.create_subprocess_exec('ls', stdin=asyncio.subprocess.PIPE)
    # 向子进程发送数据
    process.communicate(input=b'Hello\n')
    ```
    
    在背后， **asyncio.subprocess.PIPE** 将子进程配置为指向 **StreamReader** 或 **StreamWriter** 用于向子进程发送数据或从子进程发送数据，以及 **communicate()** 方法 将从配置的读取器读取或写入字节。
    
    > 如果 PIPE 传递给 stdin 参数，则 Process.stdin 属性将指向 **StreamWriter** 实例。 如果 PIPE 传递给 **stdout** 或 **stderr** 参数，则 **Process.stdout** 和 **Process.stderr** 属性将指向 **StreamReader** 实例。
    >
    > — [ASYNCIO SUBPROCESSES](https://docs.python.org/3/library/asyncio-subprocess.html)
    
    我们可以通过子进程通过 `stdin`、`stdout` 和 `stderr` 属性直接与 **StreamReader** 或 **StreamWriter** 交互。
    
    例如：

    ```python
    ...
    # 从子进程输出流中读取一行
    line = await process.stdout.readline()
    ```
    
    现在我们知道如何使用 **create_subprocess_exec()** 函数，让我们看一些有效的示例。

#### 19.2.2 Asyncio 的 create_subprocess_exec() 的示例

**19.2.2 Example of Asyncio create_subprocess_exec()**

=== "English"

    We can explore how to run a command in a subprocess from asyncio.
    
    In this example, we will execute the [“echo”](https://en.wikipedia.org/wiki/Echo_(command)) command to report back a string.
    
    The echo command will report the provided string on standard output directly.
    
    The complete example is listed below.
    
    **Note**, this example assumes you have access to the **“echo”** command, I’m not sure it will work on Windows.
    
    ```python
    # SuperFastPython.com
    # example of executing a command as a subprocess with asyncio
    import asyncio
     
    # main coroutine
    async def main():
        # start executing a command in a subprocess
        process = await asyncio.create_subprocess_exec('echo', 'Hello World')
        # report the details of the subprocess
        print(f'subprocess: {process}')
     
    # entry point
    asyncio.run(main())
    ```
    
    Running the example first creates the **main()** coroutine and executes it as the entry point into the asyncio program.
    
    The **main()** coroutine runs and calls the create_subprocess_exec() function to execute a command.
    
    The **main()** coroutine suspends while the subprocess is created. A Process instance is returned.
    
    The **main()** coroutine resumes and reports the details of the subprocess. The **main()** process terminates and the asyncio program terminates.
    
    The output of the echo command is reported on the command line.
    
    This highlights how we can execute a command from an asyncio program.
    
    ```text
    Hello World
    subprocess: <Process 50249>
    ```

=== "Chinese"

    我们可以探索如何在 asyncio 的子进程中运行命令。
    
    在此示例中，我们将执行 [“echo”](https://en.wikipedia.org/wiki/Echo_(command)) 命令来报告一个字符串。
    
    echo 命令将直接在标准输出上报告提供的字符串。
    
    下面列出了完整的示例。
    
    **注意**，此示例假设您有权访问 **“echo”** 命令，我不确定它是否适用于 Windows。
    
    ```python
    # SuperFastPython.com
    # 使用 asyncio 作为子进程执行命令的示例
    import asyncio
     
    # 主协程
    async def main():
        # 开始在子进程中执行命令
        process = await asyncio.create_subprocess_exec('echo', 'Hello World')
        # 报告子流程的详细信息
        print(f'subprocess: {process}')
     
    # 入口点
    asyncio.run(main())
    ```
    
    运行该示例首先创建 **main()** 协程，并将其作为 asyncio 程序的入口点执行。
    
    **main()** 协程运行并调用 create_subprocess_exec() 函数来执行命令。
    
    创建子进程时，**main()** 协程会挂起。 返回一个 Process 实例。
    
    **main()** 协程恢复并报告子进程的详细信息。 **main()** 进程终止，并且 asyncio 程序终止。
    
    echo 命令的输出在命令行上报告。
    
    这突出显示了我们如何从 asyncio 程序执行命令。
    
    ```text
    Hello World
    subprocess: <Process 50249>
    ```

### 19.3 如何跟shell一起运行一个命令

**How to Run a Command Via the Shell**

=== "English"

    We can execute commands using the [shell](https://en.wikipedia.org/wiki/Shell_(computing)).

    The shell is a user interface for the command line, called a command line interpreter (CLI).

    It will interpret and execute commands on behalf of the user.

    It also offers features such as a primitive programming language for scripting, wildcards, piping, shell variables (e.g. PATH), and more.

    For example, we can redirect the output of one command as input to another command, such as the contents of the “**/etc/services**” file into the word count “wc” command and count the number of lines:

    ```python
    cat /etc/services | wc -l
    ```

    Examples of shells in the Unix based operating systems include:

    - ‘sh’
    - ‘bash’
    - ‘zsh’
    - And so on.

    On Windows, the shell is probably [cmd.exe](https://en.wikipedia.org/wiki/Cmd.exe).

    See this great list of command line shells:

    - [List of command-line interpreters](https://en.wikipedia.org/wiki/List_of_command-line_interpreters), Wikipedia

    The shell is already running, it was used to start the Python program.

    You don’t need to do anything special to get or have access to the shell.

    We can execute a command from an asyncio program via the **create_subprocess_shell()** function.

    The **asyncio.create_subprocess_shell()** function takes a command and executes it using the current user shell.

    This is helpful as it not only allows the command to be executed, but allows the capabilities of the shell to be used, such as redirection, wildcards and more.

    > … the specified command will be executed through the shell. This can be useful if you are using Python primarily for the enhanced control flow it offers over most system shells and still want convenient access to other shell features such as shell pipes, filename wildcards, environment variable expansion, and expansion of ~ to a user’s home directory.
    >
    > — [SUBPROCESS — SUBPROCESS MANAGEMENT](https://docs.python.org/3/library/subprocess.html)

    The command will be executed in a subprocess of the process executing the asyncio program.

    Importantly, the asyncio program is able to interact with the subprocess asynchronously, e.g. via coroutines.

    > Because all asyncio subprocess functions are asynchronous and asyncio provides many tools to work with such functions, it is easy to execute and monitor multiple subprocesses in parallel.
    >
    > — ASYNCIO SUBPROCESSES

    There can be security considerations when executing a command via the shell instead of directly.

    This is because there is at least one level of indirection and interpretation between the request to execute the command and the command being executed, allowing possible malicious injection.

    > Important It is the application’s responsibility to ensure that all whitespace and special characters are quoted appropriately to avoid shell injection vulnerabilities.
    >
    > — ASYNCIO SUBPROCESSES

    Now that we know what **asyncio.create_subprocess_shell()** does, let’s look at how to use it.

=== "Chinese"

    我们可以使用[shell](https://en.wikipedia.org/wiki/Shell_(computing))执行命令。

    shell 是命令行的用户界面，称为命令行解释器 (CLI)。

    它将代表用户解释并执行命令。

    它还提供诸如用于脚本、通配符、管道、shell 变量（例如 PATH）等的原始编程语言等功能。
    
    例如，我们可以将一个命令的输出重定向为另一个命令的输入，例如将“**/etc/services**”文件的内容重定向到字数统计“wc”命令并统计行数：

    ```python
    cat /etc/services | wc -l
    ```

    基于 Unix 的操作系统中的 shell 示例包括：

    - ‘sh’
    - ‘bash’
    - ‘zsh’
    - 等等。

    在 Windows 上，shell 可能是 [cmd.exe](https://en.wikipedia.org/wiki/Cmd.exe)。

    请参阅这个很棒的命令行 shell 列表：

    - [命令行解释器列表](https://en.wikipedia.org/wiki/List_of_command-line_interpreters), Wikipedia

    shell已经在运行，它被用来启动Python程序。

    您无需执行任何特殊操作即可获取或访问 shell。

    我们可以通过 **create_subprocess_shell()** 函数从 asyncio 程序执行命令。

    **asyncio.create_subprocess_shell()** 函数接受一个命令并使用当前用户 shell 执行它。

    这很有用，因为它不仅允许执行命令，还允许使用 shell 的功能，例如重定向、通配符等。

    > … 指定的命令将通过 shell 执行。 如果您使用 Python 主要是为了增强它在大多数系统 shell 上提供的控制流，并且仍然希望方便地访问其他 shell 功能（例如 shell 管道、文件名通配符、环境变量扩展以及将 ~ 扩展到用户主目录），那么这会很有用。
    >
    > — [SUBPROCESS — SUBPROCESS MANAGEMENT](https://docs.python.org/3/library/subprocess.html)

    该命令将在执行 asyncio 程序的进程的子进程中执行。

    重要的是，asyncio 程序能够与子进程异步交互，例如 通过协程。

    > 因为所有 asyncio 子进程函数都是异步的，并且 asyncio 提供了许多工具来使用这些函数，所以很容易并行执行和监视多个子进程。
    >
    > — [ASYNCIO SUBPROCESSES](https://docs.python.org/3/library/asyncio-subprocess.html)

    通过 shell 而不是直接执行命令时可能存在安全考虑。

    这是因为执行命令的请求和正在执行的命令之间至少存在一层间接和解释，从而允许可能的恶意注入。

    > 重要的应用程序有责任确保所有空格和特殊字符都被正确引用，以避免 shell 注入漏洞。
    >
    > — [ASYNCIO SUBPROCESSES](https://docs.python.org/3/library/asyncio-subprocess.html)

    现在我们知道了 **asyncio.create_subprocess_shell()** 的作用，让我们看看如何使用它。

#### 19.3.1 如何使用 Asyncio 的 create_subprocess_shell()

**19.3.1 How to Use Asyncio create_subprocess_shell()**

=== "English"

    The [asyncio.create_subprocess_shell()](https://docs.python.org/3/library/asyncio-subprocess.html#asyncio.create_subprocess_shell) function will execute a given string command via the current shell.

    It returns a [asyncio.subprocess.Process](https://docs.python.org/3/library/asyncio-subprocess.html#asyncio.subprocess.Process) object that represents the process.

    It is very similar to the **create_subprocess_shell()** function we saw in a previous section. Nevertheless, we will review how to use the function and interact with the process via the Process instance (in case you skipped straight to this section).

    The **create_subprocess_shell()** function is a coroutine, which means we must await it. It will return once the subprocess has been started, not when the subprocess is finished.

    For example:

    ```python
    ...
    # start a subprocess
    process = await asyncio.create_subprocess_shell('ls')
    We can wait for the subprocess to finish by awaiting the **wait()** method.
    ```

    For example:

    ```python
    ...
    # wait for the subprocess to terminate
    await process.wait()
    ```

    We can stop the subprocess directly by calling the **terminate()** or **kill()** methods, which will raise a signal in the subprocess.

    The input and output of the command will be handled by the shell, e.g. **stdin**, **stderr**, and **stdout**.

    We can have the asyncio program handle the input or output for the subprocess.

    This can be achieved by specifying the input or output stream and specifying a constant to redirect, such as **asyncio.subprocess.PIPE**.

    For example, we can redirect the output of a command to the asyncio program:

    ```python
    ...
    # start a subprocess and redirect output
    process = await asyncio.create_subprocess_shell('ls', stdout=asyncio.subprocess.PIPE)
    ```

    We can then read the output of the program via the **asyncio.subprocess.Process** instance via the **communicate()** method.

    This method is a coroutine and must be awaited. It is used to both send and receive data with the subprocess.

    For example:

    ```python
    ...
    # read data from the subprocess
    line = process.communicate()
    ```

    We can also send data to the subprocess via the **communicate()** method by setting the “input” argument in bytes.

    For example:

    ```python
    ...
    # start a subprocess and redirect input
    process = await asyncio.create_subprocess_shell('ls', stdin=asyncio.subprocess.PIPE)
    # send data to the subprocess
    process.communicate(input=b'Hello\n')
    ```

    Behind the scenes the **asyncio.subprocess.PIPE** configures the subprocess to point to a **StreamReader** or **StreamWriter** for sending data to or from the subprocess, and the **communicate()** method will read or write bytes from the configured reader.

    > If PIPE is passed to stdin argument, the Process.stdin attribute will point to a StreamWriter instance. If PIPE is passed to stdout or stderr arguments, the Process.stdout and Process.stderr attributes will point to StreamReader instances.
    > 
    > — [ASYNCIO SUBPROCESSES](https://docs.python.org/3/library/asyncio-subprocess.html)

    We can interact with the **StreamReader** or **StreamWriter** directly via the subprocess via the stdin, stdout, and stderr attributes.

    For example:

    ```python
    ...
    # read a line from the subprocess output stream
    line = await process.stdout.readline()
    ```

    Now that we know how to use the **create_subprocess_shell()** function, let’s look at some worked examples.

=== "Chinese"

    [asyncio.create_subprocess_shell()](https://docs.python.org/3/library/asyncio-subprocess.html#asyncio.create_subprocess_shell) 函数将通过当前 shell 执行给定的字符串命令。

    它返回一个表示进程的 [asyncio.subprocess.Process](https://docs.python.org/3/library/asyncio-subprocess.html#asyncio.subprocess.Process) 对象。

    它与我们在上一节中看到的 **create_subprocess_shell()** 函数非常相似。 尽管如此，我们将回顾如何使用该函数并通过 **Process** 实例与流程交互（如果您直接跳到本节）。

    **create_subprocess_shell()** 函数是一个协程，这意味着我们必须等待它。 它会在子进程启动后返回，而不是在子进程完成时返回。

    例如:

    ```python
    ...
    # 启动一个子进程
    process = await asyncio.create_subprocess_shell('ls')
    ```

    我们可以通过等待 **wait()** 方法来等待子进程完成。

    例如:

    ```python
    ...
    # 等待子进程终止
    await process.wait()
    ```

    我们可以通过调用 **terminate()** 或 **kill()** 方法直接停止子进程，这将在子进程中引发一个信号。

    命令的输入和输出将由 shell 处理，例如 **标准输入**、**标准错误**和**标准输出**。

    我们可以让 asyncio 程序处理子进程的输入或输出。

    这可以通过指定输入或输出流并指定要重定向的常量来实现，例如 **asyncio.subprocess.PIPE**。

    例如，我们可以将命令的输出重定向到 asyncio 程序：

    ```python
    ...
    # 启动子进程并重定向输出
    process = await asyncio.create_subprocess_shell('ls', stdout=asyncio.subprocess.PIPE)
    ```

    然后我们可以通过**asyncio.subprocess.Process**实例通过**communicate()**方法读取程序的输出。

    该方法是一个协程，必须等待。 它用于通过子进程发送和接收数据。

    例如：

    ```python
    ...
    # 从子进程读取数据
    line = process.communicate()
    ```

    我们还可以通过以字节为单位设置“input”参数，通过 **communicate()** 方法将数据发送到子进程。

    例如：

    ```python
    ...
    # 启动子进程并重定向输入
    process = await asyncio.create_subprocess_shell('ls', stdin=asyncio.subprocess.PIPE)
    # 向子进程发送数据
    process.communicate(input=b'Hello\n')
    ```

    在背后， **asyncio.subprocess.PIPE** 将子进程配置为指向 **StreamReader** 或 **StreamWriter** 用于向子进程发送数据或从子进程发送数据，以及 **communicate()** 方法 将从配置的读取器读取或写入字节。

    > 如果 PIPE 传递给 stdin 参数，则 Process.stdin 属性将指向 StreamWriter 实例。 如果 PIPE 传递给 stdout 或 stderr 参数，则 Process.stdout 和 Process.stderr 属性将指向 StreamReader 实例。
    > 
    > — [ASYNCIO SUBPROCESSES](https://docs.python.org/3/library/asyncio-subprocess.html)

    我们可以通过子进程的 `stdin`、`stdout` 和 `stderr` 属性直接与 **StreamReader** 或 **StreamWriter** 交互。

    例如：

    ```python
    ...
    # 从子进程输出流中读取一行
    line = await process.stdout.readline()
    ```

    现在我们知道如何使用 **create_subprocess_shell()** 函数，让我们看一些有效的示例。

#### 19.3.2 Asyncio 的 create_subprocess_shell() 的示例

**19.3.2 Example of Asyncio create_subprocess_shell()**

=== "English"

    We can explore how to run a command in a subprocess from asyncio using the shell.

    In this example, we will execute the [“echo”](https://en.wikipedia.org/wiki/Echo_(command)) command to report back a string.

    The echo command will report the provided string on standard output directly.

    The complete example is listed below.

    Note, this example assumes you have access to the “echo” command, I’m not sure it will work on Windows.

    ```python
    # SuperFastPython.com
    # example of executing a shell command as a subprocess with asyncio
    import asyncio
    
    # main coroutine
    async def main():
        # start executing a shell command in a subprocess
        process = await asyncio.create_subprocess_shell('echo Hello World')
        # report the details of the subprocess
        print(f'subprocess: {process}')
    
    # entry point
    asyncio.run(main())
    ```

    Running the example first creates the main() coroutine and executes it as the entry point into the asyncio program.

    The main() coroutine runs and calls the create_subprocess_shell() function to execute a command.

    The main() coroutine suspends while the subprocess is created. A Process instance is returned.

    The main() coroutine resumes and reports the details of the subprocess. The main() process terminates and the asyncio program terminates.

    The output of the echo command is reported on the command line.

    This highlights how we can execute a command using the shell from an asyncio program.

    ```text
    subprocess: <Process 43916>
    Hello World
    ```

=== "Chinese"

    我们可以探索如何使用 shell 从 asyncio 的子进程中运行命令。

    在此示例中，我们将执行 [“echo”](https://en.wikipedia.org/wiki/Echo_(command)) 命令来报告一个字符串。

    echo 命令将直接在标准输出上报告提供的字符串。

    下面列出了完整的示例。

    请注意，此示例假设您有权访问“echo”命令，我不确定它是否适用于 Windows。

    ```python
    # SuperFastPython.com
    # 使用 asyncio 作为子进程执行 shell 命令的示例
    import asyncio
    
    # 主协程
    async def main():
        # 开始在子进程中执行 shell 命令
        process = await asyncio.create_subprocess_shell('echo Hello World')
        # 报告子流程的详细信息
        print(f'subprocess: {process}')
    
    # 入口点
    asyncio.run(main())
    ```

    运行该示例首先创建 **main()** 协程并将其作为 asyncio 程序的入口点执行。

    **main()** 协程运行并调用 **create_subprocess_shell()** 函数来执行命令。

    **main()** 协程在子进程创建时挂起。 返回一个 [**Process**](https://docs.python.org/3/library/asyncio-subprocess.html#asyncio.subprocess.Process) 实例。

    **main()** 协程恢复并报告子流程的详细信息。 **main()** 进程终止，asyncio 程序终止。

    echo 命令的输出在命令行上报告。

    这突出显示了我们如何使用 asyncio 程序中的 shell 执行命令。

    ```text
    subprocess: <Process 43916>
    Hello World
    ```

## 20. 非阻塞流

**20. Non-Blocking Streams**

=== "English"

    A major benefit of asyncio is the ability to use non-blocking streams.

    Let’s take a closer look.

=== "Chinese"

    asyncio 的一个主要好处是能够使用非阻塞流。

    让我们仔细看看。

### 20.1 Asyncio 的流

**20.1 Asyncio Streams**

=== "English"

    Asyncio provides non-blocking I/O socket programming.

    This is provided via streams.

    > Streams are high-level async/await-ready primitives to work with network connections. Streams allow sending and receiving data without using callbacks or low-level protocols and transports.
    >
    > — ASYNCIO STREAMS

    Sockets can be opened that provide access to a stream writer and a stream writer.

    Data can then be written and read from the stream using coroutines, suspending when appropriate.

    Once finished, the socket can be closed.

    The asyncio streams capability is low-level meaning that any protocols required must be implemented manually.

    This might include common web protocols, such as:

    - HTTP or HTTPS for interacting with web servers
    - SMTP for interacting with email servers
    - FTP for interacting with file servers.

    The streams can also be used to create a server to handle requests using a standard protocol, or to develop your own application-specific protocol.

    Now that we know what asyncio streams are, let’s look at how to use them.

=== "Chinese"

    Asyncio 提供非阻塞 I/O 套接字编程。

    这是通过流(streams)提供的。

    > 流(streams)是高级异步/等待就绪原语，可与网络连接一起使用。 流允许在不使用回调或低级协议和传输的情况下发送和接收数据。
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    可以打开提供对流写入器和流写入器的访问的套接字。

    然后可以使用协程在流中写入和读取数据，并在适当的时候挂起。

    完成后，可以关闭套接字。

    异步流功能是低级的，这意味着必须手动实现所需的任何协议。

    这可能包括常见的网络协议，例如：

    - 用于与 Web 服务器交互的 HTTP 或 HTTPS
    - 用于与电子邮件服务器交互的 SMTP
    - 用于与文件服务器交互的 FTP。
    
    这些流还可用于创建服务器来使用标准协议处理请求，或开发您自己的特定于应用程序的协议。

    现在我们知道什么是异步流，让我们看看如何使用它们。

### 20.2 如何打开一个连接

**20.2 How to Open a Connection**

=== "English"

    An asyncio TCP client socket connection can be opened using the [asyncio.open_connection()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.open_connection) function.

    > Establish a network connection and return a pair of (reader, writer) objects. The returned reader and writer objects are instances of StreamReader and StreamWriter classes.
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    This is a coroutine that must be awaited and will return once the socket connection is open.

    The function returns a **StreamReader** and **StreamWriter** object for interacting with the socket.

    For example:

    ```python
    ...
    # open a connection
    reader, writer = await asyncio.open_connection(...)
    ```

    The **asyncio.open_connection()** function takes many arguments in order to configure the socket connection.

    The two required arguments are the host and the port.

    The host is a string that specifies the server to connect to, such as a domain name or an IP address.

    The port is the socket port number, such as 80 for HTTP servers, 443 for HTTPS servers, 23 for SMTP and so on.

    For example:

    ```python
    ...
    # open a connection to an http server
    reader, writer = await asyncio.open_connection('www.google.com', 80)
    ```

    Encrypted socket connections are supported over the SSL protocol.

    The most common example might be HTTPS which is replacing HTTP.

    This can be achieved by setting the “**ssl**” argument to **True**.

    For example:

    ```python
    ...
    # open a connection to an https server
    reader, writer = await asyncio.open_connection('www.google.com', 443, ssl=True)
    ```

=== "Chinese"

    可以使用 [asyncio.open_connection()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.open_connection) 函数打开 asyncio TCP 客户端套接字连接。

    > 建立网络连接并返回一对（reader、writer）对象。 返回的读取器和写入器对象是 StreamReader 和 StreamWriter 类的实例。
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    这是一个必须等待的协程，一旦套接字连接打开就会返回。

    该函数返回一个 **StreamReader** 和 **StreamWriter** 对象，用于与套接字交互。

    例如:

    ```python
    ...
    # 打开一个连接
    reader, writer = await asyncio.open_connection(...)
    ```

    **asyncio.open_connection()** 函数需要许多参数来配置套接字连接。

    两个必需的参数是主机和端口。

    主机是一个字符串，指定要连接的服务器，例如域名或IP地址。

    port 是套接字端口号，例如 HTTP 服务器为 80，HTTPS 服务器为 443，SMTP 为 23 等。

    例如:

    ```python
    ...
    # 打开与 http 服务器的连接
    reader, writer = await asyncio.open_connection('www.google.com', 80)
    ```

    SSL 协议支持加密套接字连接。

    最常见的例子可能是 HTTPS，它正在取代 HTTP。

    这可以通过将“**ssl**”参数设置为**True**来实现。

    例如:

    ```python
    ...
    # 打开与 https 服务器的连接
    reader, writer = await asyncio.open_connection('www.google.com', 443, ssl=True)
    ```

### 20.3 如何启动一个侦听服务

**20.3 How to Start a Server**

=== "English"

    An asyncio TCP server socket can be opened using the [asyncio.start_server()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.start_server) function.

    > Create a TCP server (socket type SOCK_STREAM) listening on port of the host address.
    >
    > — [ASYNCIO EVENT LOOP](https://docs.python.org/3/library/asyncio-eventloop.html)

    This is a coroutine that must be awaited.

    The function returns an [asyncio.Server](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.Server) object that represents the running server.

    For example:

    ```python
    ...
    # start a tcp server
    server = await asyncio.start_server(...)
    ```

    The three required arguments are the callback function, the host, and the port.

    The callback function is a custom function specified by name that will be called each time a client connects to the server.

    > The client_connected_cb callback is called whenever a new client connection is established. It receives a (reader, writer) pair as two arguments, instances of the StreamReader and StreamWriter classes.
    >
    > — ASYNCIO STREAMS

    The host is the domain name or IP address that clients will specify to connect. The port is the socket port number on which to receive connections, such as 21 for FTP or 80 for HTTP.

    For example:

    ```python
    # handle connections
    async def handler(reader, writer):
        # ...
    
    ...
    # start a server to receive http connections
    server = await asyncio.start_server(handler, '127.0.0.1', 80)
    ```

=== "Chinese"

    可以使用 [asyncio.start_server()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.start_server) 函数打开 **asyncio TCP** 服务器套接字。

    > 创建一个 TCP 服务器（套接字类型 **SOCK_STREAM**），侦听主机地址的端口。
    >
    > — [ASYNCIO EVENT LOOP](https://docs.python.org/3/library/asyncio-eventloop.html)

    这是一个必须等待的协程。

    该函数返回一个代表正在运行的服务器的 [asyncio.Server](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.Server) 对象。

    例如:

    ```python
    ...
    # 启动一个tcp服务器
    server = await asyncio.start_server(...)
    ```

    三个必需参数是回调函数、主机和端口。

    回调函数是一个由名称指定的自定义函数，每次客户端连接到服务器时都会调用该函数。

    > 每当建立新的客户端连接时，都会调用 `client_connected_cb` 回调。 它接收一个（读取器(reader)，写入器(writer)）对作为两个参数，即 **StreamReader** 和 **StreamWriter** 类的实例。
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    主机是客户端指定连接的域名或IP地址。 **port** 是接收连接的套接字端口号，例如 **FTP** 为 `21`，**HTTP** 为 `80`。

    例如:

    ```python
    # 处理连接
    async def handler(reader, writer):
        # ...
    
    ...
    # 启动一个服务器来接收http连接
    server = await asyncio.start_server(handler, '127.0.0.1', 80)
    ```

### 20.4 如何使用 StreamWriter 写入数据

**20.4 How to Write Data with the StreamWriter**

=== "English"

    We can write data to the socket using an [asyncio.StreamWriter](https://docs.python.org/3/library/asyncio-stream.html#streamwriter).

    > Represents a writer object that provides APIs to write data to the IO stream.
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    Data is written as bytes.

    Byte data can be written to the socket using the [write()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.StreamWriter.write) method.

    > The method attempts to write the data to the underlying socket immediately. If that fails, the data is queued in an internal write buffer until it can be sent.
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    For example:

    ```python
    ...
    # write byte data
    writer.write(byte_data)
    ```

    Alternatively, multiple “lines” of byte data organized into a list or iterable can be written using the [writelines()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.StreamWriter.writelines) method.

    For example:

    ```python
    ...
    # write lines of byte data
    writer.writelines(byte_lines)
    ```

    Neither method for writing data blocks or suspends the calling coroutine.

    After writing byte data it is a good idea to drain the socket via the [drain()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.StreamWriter.drain) method.

    > Wait until it is appropriate to resume writing to the stream.
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    This is a coroutine and will suspend the caller until the bytes have been transmitted and the socket is ready.

    For example:

    ```python
    ...
    # write byte data
    writer.write(byte_data)
    # wait for data to be transmitted
    await writer.drain()
    ```

=== "Chinese"

    我们可以使用 [asyncio.StreamWriter](https://docs.python.org/3/library/asyncio-stream.html#streamwriter) 将数据写入套接字。

    > 表示一个 writer 对象，它提供 API 将数据写入 IO 流。
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    数据以字节形式写入。

    可以使用 [write()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.StreamWriter.write) 方法将字节数据写入套接字。

    > 该方法尝试立即将数据写入底层套接字。 如果失败，数据将在内部写入缓冲区中排队，直到可以发送。
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    例如:

    ```python
    ...
    # 写入字节数据
    writer.write(byte_data)
    ```

    或者，可以使用 [writelines()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.StreamWriter) 写入组织成列表或可迭代的多“行”字节数据。 writelines) 方法。

    例如:

    ```python
    ...
    # 写入字节数据行
    writer.writelines(byte_lines)
    ```

    这两种方法都不会写入数据块或挂起调用协程。

    写入字节数据后，最好通过 [drain()](https://docs.python.org/3/library/asyncio-stream.html#asyncio.StreamWriter.drain) 方法排空套接字。

    > 等到合适的时候再继续写入流。
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    这是一个协程，将挂起调用者，直到字节已传输且套接字准备就绪。

    例如:

    ```python
    ...
    # 写入字节数据
    writer.write(byte_data)
    # 等待数据传输
    await writer.drain()
    ```

### 20.5 如何使用 StreamReader 读取数据

**20.5 How to Read Data with the StreamReader**

=== "English"

    We can read data from the socket using an [asyncio.StreamReader](https://docs.python.org/3/library/asyncio-stream.html#streamreader).

    > Represents a reader object that provides APIs to read data from the IO stream.
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    Data is read in byte format, therefore strings may need to be encoded before being used.

    All read methods are coroutines that must be awaited.

    An arbitrary number of bytes can be read via the read() method, which will read until the end of file (EOF).

    ```python
    ...
    # read byte data
    byte_data = await reader.read()
    ```

    Additionally, the number of bytes to read can be specified via the “n” argument.

    > Read up to n bytes. If n is not provided, or set to -1, read until EOF and return all read bytes.
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    This may be helpful if you know the number of bytes expected from the next response.

    For example:

    ```python
    ...
    # read byte data
    byte_data = await reader.read(n=100)
    ```

    A single line of data can be read using the readline() method.

    This will return bytes until a new line character ‘\n’ is encountered, or EOF.

    > Read one line, where “line” is a sequence of bytes ending with \n. If EOF is received and \n was not found, the method returns partially read data. If EOF is received and the internal buffer is empty, return an empty bytes object.
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    This is helpful when reading standard protocols that operate with lines of text.

    ```python
    ...
    # read a line data
    byte_line = await reader.readline()
    ```

    Additionally, there is a readexactly() method to read an exact number of bytes otherwise raise an exception, and a readuntil() that will read bytes until a specified character in byte form is read.

=== "Chinese"

    我们可以使用 [asyncio.StreamReader](https://docs.python.org/3/library/asyncio-stream.html#streamreader) 从套接字读取数据。

    > 表示一个读取器对象，它提供 API 以从 IO 流读取数据。
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    数据以字节格式读取，因此字符串在使用之前可能需要进行编码。

    所有读取方法都是必须等待的协程。

    可以通过 **read()** 方法读取任意数量的字节，该方法将读取到文件末尾 (EOF)。

    ```python
    ...
    # 读取字节数据
    byte_data = await reader.read()
    ```

    此外，可以通过“n”参数指定要读取的字节数。

    > 最多读取 n 个字节。 如果未提供 n 或设置为 -1，则读取直到 EOF 并返回所有读取的字节。
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    如果您知道下一个响应的预期字节数，这可能会有所帮助。

    例如:

    ```python
    ...
    # 读取字节数据
    byte_data = await reader.read(n=100)
    ```

    可以使用 **readline()** 方法读取单行数据。

    这将返回字节，直到遇到新行字符“\n”或 EOF。

    > 读取一行，其中“line”是以\n结尾的字节序列。 如果收到 EOF 但未找到 \n，则该方法返回部分读取的数据。 如果收到 EOF 并且内部缓冲区为空，则返回一个空字节对象。
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    这在阅读使用文本行操作的标准协议时很有帮助。

    ```python
    ...
    # 读取一行数据
    byte_line = await reader.readline()
    ```

    此外，还有一个 **readexactly()** 方法用于读取确切的字节数，否则会引发异常，还有一个 **readuntil()** 方法将读取字节，直到读取字节形式的指定字符。

### 20.6 如何关闭连接

**20.6 How to Close Connection**

=== "English"

    The socket can be closed via the asyncio.StreamWriter.

    The close() method can be called which will close the socket.

    > The method closes the stream and the underlying socket.
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)
    
    This method does not block.

    For example:

    ```python
    ...
    # close the socket
    writer.close()
    ```

    Although the close() method does not block, we can wait for the socket to close completely before continuing on.

    This can be achieved via the wait_closed() method.

    > Wait until the stream is closed. Should be called after close() to wait until the underlying connection is closed.
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    This is a coroutine that can be awaited.

    For example:

    ```python
    ...
    # close the socket
    writer.close()
    # wait for the socket to close
    await writer.wait_closed()
    ```

    We can check if the socket has been closed or is in the process of being closed via the is_closing() method.

    For example:

    ```python
    ...
    # check if the socket is closed or closing
    if writer.is_closing():
        # ...
    ```

=== "Chinese"

    可以通过 **asyncio.StreamWriter** 关闭套接字。

    可以调用 **close()** 方法来关闭套接字。

    > 该方法关闭流和底层套接字。
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)
    
    该方法不会阻塞。

    例如：

    ```python
    ...
    # 关闭套接字
    writer.close()
    ```

    虽然 **close()** 方法不会阻塞，但我们可以等待套接字完全关闭后再继续。

    这可以通过 **wait_close()** 方法来实现。

    > 等待流关闭。 应在 **close()** 之后调用以等待底层连接关闭。
    >
    > — [ASYNCIO STREAMS](https://docs.python.org/3/library/asyncio-stream.html)

    这是一个可以等待的协程。

    例如：

    ```python
    ...
    # 关闭套接字
    writer.close()
    # 等待套接字关闭
    await writer.wait_closed()
    ```

    我们可以通过 **is_close()** 方法检查套接字是否已关闭或正在关闭过程中。

    例如：

    ```python
    ...
    # 检查套接字是否已关闭或正在关闭
    if writer.is_closing():
        # ...
    ```

## 21. 检查网站状态的示例

**21. Example of Checking Website Status**

=== "English"

    We can query the HTTP status of websites using asyncio by opening a stream and writing and reading HTTP requests and responses.
    
    We can then use asyncio to query the status of many websites concurrently, and even report the results dynamically.
    
    Let’s get started.

=== "Chinese"

    我们可以使用 asyncio 通过打开流并写入和读取 HTTP 请求和响应来查询网站的 HTTP 状态。

    然后我们可以使用 asyncio 同时查询多个网站的状态，甚至动态报告结果。
    
    让我们开始吧。

### 21.1 如何使用 Asyncio 检查 HTTP 状态

**21.1 How to Check HTTP Status with Asyncio**

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

### 21.2 打开 HTTP 连接

**21.2 Open HTTP Connection**

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

### 21.3 写入 HTTP 请求

**21.3 Write HTTP Request**

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

### 21.4 读取 HTTP 响应

**21.4 Read HTTP Response**

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

### 21.5 关闭 HTTP 连接

**21.5 Close HTTP Connection**

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

### 21.6 按顺序检查 HTTP 状态的示例

**21.6 Example of Checking HTTP Status Sequentially**

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

### 21.7 并发检查网站状态的示例

**21.7 Example of Checking Website Status Concurrently**

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

## 22. Python Asyncio 常见错误

**22. Python Asyncio Common Errors**

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

### 22.1 错误 1: 尝试通过函数调用的方式来运行协程

**22.1 Error 1: Trying to Run Coroutines by Calling Them**

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

### 22.2 错误 2: 不在事件循环中运行协程

**22.2 Error 2: Not Letting Coroutines Run in the Event Loop**

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

### 22.3 错误 3: 使用低级的 Asyncio API

**22.3 Error 3: Using the Low-Level Asyncio API**

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

### 22.4 错误 4: 退出主协程太早

**22.4 Error 4: Exiting the Main Coroutine Too Early**

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

### 22.5 错误 5: 假设竞争条件和死锁是不可能的

**22.5 Error 5: Assuming Race Conditions and Deadlocks are Impossible**

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

## 23. Python Asyncio 常见问题

**23. Python Asyncio Common Questions**

=== "English"

    This section answers common questions asked by developers when using asyncio in Python.

    **Do you have a question about asyncio?**

    Ask your question in the comments below and I will do my best to answer it and perhaps add it to this list of questions.

=== "Chinese"

    本节回答开发人员在 Python 中使用 **asyncio** 时提出的常见问题。

    **您对 asyncio 有疑问吗?**

    在下面的评论中提出您的问题，我会尽力回答它，也许会将其添加到这个问题列表中。

### 23.1 如何停止任务？

**23.1 How to Stop a Task?**

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

### 23.2 如何等待任务完成？

**23.2 How to Wait for a Task To Finish?**

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

### 23.3 如何获取任务的返回值？

**23.3 How to Get a Return Value from a Task?**

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

### 23.4 如何在后台运行任务？

**23.4 How to Run a Task in the Background?**

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

### 23.5 如何等待所有后台任务？

**23.5 How to Wait for All Background Tasks?**

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

### 23.6 正在运行的任务是否会阻止事件循环退出？

**23.6 Does a Running Task Stop the Event Loop from Exiting?**

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

### 23.7 如何显示运行任务的进度？

**23.7 How to Show Progress of Running Tasks?**

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

### 23.8 如何在延迟后运行任务？

**23.8 How to Run a Task After a Delay?**

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

### 23.9 如何运行后续任务？

**23.9 How to Run a Follow-Up Task?**

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

### 23.10 如何在 Asyncio 中执行阻塞 I/O 或 CPU 密集型函数？

**23.10 How to Execute a Blocking I/O or CPU-bound Function in Asyncio?**

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

## 24. 使用 Asyncio 的常见反对意见

**24. Common Objections to Using Asyncio**

=== "English"

    Asyncio and coroutines may not be the best solution for all concurrency problems in your program.
    
    That being said, there may also be some misunderstandings that are preventing you from making full and best use of the capabilities of the asyncio in Python.
    
    In this section, we review some of the common objections seen by developers when considering using the asyncio.

=== "Chinese"

    异步和协程可能不是解决程序中所有并发问题的最佳解决方案。

    话虽如此，也可能存在一些误解，阻碍您充分、最佳地利用 Python 中 asyncio 的功能。

    在本节中，我们将回顾开发人员在考虑使用 asyncio 时遇到的一些常见反对意见。

### 24.1 全局解释器锁 (GIL) 怎么样？

**24.1 What About the Global Interpreter Lock (GIL)?**

=== "English"

    The GIL protects the internals of the Python interpreter from concurrent access and modification from multiple threads.
    
    The asyncio event loop runs in one thread.
    
    This means that all coroutines run in a single thread.
    
    As such the GIL is not an issue when using asyncio and coroutine.

=== "Chinese"

    GIL 保护 Python 解释器的内部免受多个线程的并发访问和修改。

    asyncio 事件循环在一个线程中运行。

    这意味着所有协程都在单个线程中运行。

    因此，使用 asyncio 和协程时，GIL 不是问题。

### 24.2 Python 协程是“真实的”吗？

**24.2 Are Python Coroutines “Real“?**

=== "English"

    Coroutines are managed in software.
    
    Coroutines run and are managed (switched) within the asyncio event loop in the Python runtime.
    
    They are not a software representation of a capability provided by the underlying operating system, like threads and processes.
    
    In this sense, Python does not have support for “native coroutines”, but I’m not sure such things exist in modern operating systems.

=== "Chinese"

    协程由软件管理。
    
    协程在 Python 运行时的 **asyncio 事件循环**中运行和管理（切换）。
    
    它们不是底层操作系统级别提供的功能以及软件表示，例如线程和进程。
    
    从这个意义上说，Python 不支持“原生协程”，但我不确定现代操作系统中是否存在这样的东西。

### 24.3 Python 的并发性不是有问题吗？

**24.3 Isn’t Python Concurrency Buggy?**

=== "English"

    No.
    
    Python provides first-class concurrency with coroutines, threads, and processes.
    
    It has for a long time now and it is widely used in open source and commercial projects.

=== "Chinese"

    不🙅🏻‍♀️。
    
    Python 通过协程、线程和进程提供一流的并发性。
    
    它已经存在很长时间了，并且广泛应用于开源和商业项目中。

### 24.4 对于并发来说，Python 不是一个糟糕的选择吗？

**24.4 Isn’t Python a Bad Choice for Concurrency?**

=== "English"

    Developers love python for many reasons, most commonly because it is easy to use and fast for development.
    
    Python is commonly used for glue code, one-off scripts, but more and more for large-scale software systems.
    
    If you are using Python and then you need concurrency, then you work with what you have. The question is moot.
    
    If you need concurrency and you have not chosen a language, perhaps another language would be more appropriate, or perhaps not. Consider the full scope of functional and non-functional requirements (or user needs, wants, and desires) for your project and the capabilities of different development platforms.

=== "Chinese"

    开发人员喜爱 Python 的原因有很多，最常见的是因为它易于使用且开发速度快。
    
    Python 通常用于粘合代码、一次性脚本，但越来越多地用于大型软件系统。
    
    如果您使用 Python 并且需要并发性，那么您可以使用现有的东西。 这个问题毫无意义。
    
    如果您需要并发性并且尚未选择一种语言，那么另一种语言可能更合适，也可能不合适。 需要考虑项目的全部功能和非功能需求（或用户的需求、想法和愿望）以及不同开发平台的功能。

### 25.5 为什么不使用线程来代替？

**25.5 Why Not Use Threads Instead?**

=== "English"

    You can use threads instead of asyncio.
    
    Any program developed using threads can be rewritten to use asyncio and coroutines.
    
    Any program developed using coroutines and asyncio can be rewritten to use threads.
    
    Adopting asyncio in a project is a choice, the rationale is yours.
    
    For the most part, they are **functionally equivalent.**（功能等效）
    
    Many use cases will execute faster using threads and may be more familiar(亲切) to a wider array of Python developers.
    
    Some use cases in the areas of network programming and executing system commands may be simpler(最简单) (less code) when using asyncio, and significantly more scalable than using threads.

=== "Chinese"

    - 您可以使用线程而不是异步。
    
    - 任何使用**线程**开发的程序都可以使用 **asyncio 和协程**重写。
    
    - 任何使用**协程和 asyncio** 开发的程序都可以使用**线程**重写。
    
    - 在项目中采用 asyncio 是一种选择，其理由由您决定。
    
    - 在大多数情况下，它们在功能上是等效的。
    
    - 许多用例使用线程将执行得更快，并且可能为更广泛的 Python 开发人员所熟悉。
    
    - 使用 asyncio 时，网络编程和执行系统命令领域的一些用例可能会更简单（最简单）（代码更少），并且比使用线程更具可扩展性。

## 25. 进一步阅读

**25. Further Reading**

=== "English"

    This section lists helpful additional resources on the topic.

=== "Chinese"

    本节列出了有关该主题的有用的其他资源。

### 25.1 Python 异步书籍

**25.1 Python Asyncio Books**

=== "English"

    This section lists my books on Python asyncio, designed to help you get started and get good, super fast.
    
    - [Python Asyncio Jump-Start](https://superfastpython.com/paj-further-reading), Jason Brownlee, 2022. (my book!)
    - [Python Asyncio Interview Questions](https://amzn.to/3XFEZgj)
    - [Asyncio Module API Cheat Sheet](https://superfastpython.gumroad.com/l/pacs)
    
    Other books on asyncio include:
    
    - [Python Concurrency with asyncio](https://amzn.to/3LZvxNn), Matthew Fowler, 2022.
    - [Using Asyncio in Python](https://amzn.to/3lNp2ml), Caleb Hattingh, 2020.

=== "Chinese"

    本节列出了我有关 Python asyncio 的书籍，旨在帮助您快速入门并获得良好的效果。
    
    - [Python Asyncio Jump-Start](https://superfastpython.com/paj-further-reading), Jason Brownlee, 2022. (my book!)
    - [Python Asyncio Interview Questions](https://amzn.to/3XFEZgj)
    - [Asyncio Module API Cheat Sheet](https://superfastpython.gumroad.com/l/pacs)
    
    其他关于 asyncio 的书籍包括：
    
    - [Python Concurrency with asyncio](https://amzn.to/3LZvxNn), Matthew Fowler, 2022.
    - [Using Asyncio in Python](https://amzn.to/3lNp2ml), Caleb Hattingh, 2020.

### 25.2 APIs

=== "English"

    - [asyncio — Asynchronous I/O](https://docs.python.org/3/library/asyncio.html)
    - [Asyncio Coroutines and Tasks](https://docs.python.org/3/library/asyncio-task.html)
    - [Asyncio Streams](https://docs.python.org/3/library/asyncio-stream.html)
    - [Asyncio Subprocesses](https://docs.python.org/3/library/asyncio-subprocess.html)
    - [Asyncio Queues](https://docs.python.org/3/library/asyncio-queue.html)
    - [Asyncio Synchronization Primitives](https://docs.python.org/3/library/asyncio-sync.html)

=== "Chinese"

    - [asyncio — Asynchronous I/O](https://docs.python.org/3/library/asyncio.html)
    - [Asyncio Coroutines and Tasks](https://docs.python.org/3/library/asyncio-task.html)
    - [Asyncio Streams](https://docs.python.org/3/library/asyncio-stream.html)
    - [Asyncio Subprocesses](https://docs.python.org/3/library/asyncio-subprocess.html)
    - [Asyncio Queues](https://docs.python.org/3/library/asyncio-queue.html)
    - [Asyncio Synchronization Primitives](https://docs.python.org/3/library/asyncio-sync.html)

### 25.3 参考

**25.3 References**

=== "English"

    - [Asynchronous I/O, Wikipedia.](https://en.wikipedia.org/wiki/Asynchronous_I/O)
    - [Coroutine, Wikipedia.](https://en.wikipedia.org/wiki/Coroutine)

=== "Chinese"

    - [Asynchronous I/O, Wikipedia.](https://en.wikipedia.org/wiki/Asynchronous_I/O)
    - [Coroutine, Wikipedia.](https://en.wikipedia.org/wiki/Coroutine)

## 26. 结论

**26. Conclusions**

=== "English"

    This is a large guide, and you have discovered in great detail how asyncio and coroutines work in Python and how to best use them in your project.
    
    **Did you find this guide useful?**
    
    I’d love to know, please share a kind word in the comments below.
    
    **Have you used asyncio on a project?**
    
    I’d love to hear about it, please let me know in the comments.
    
    **Do you have any questions?**
    
    Leave your question in a comment below and I will reply fast with my best advice.
    
    Join the discussion on [reddit](https://www.reddit.com/r/Python/comments/yqrr94/python_asyncio_the_complete_guide/) and [hackernews](https://news.ycombinator.com/item?id=33547323).

=== "Chinese"

    这是一本很大的指南，您已经详细了解了 asyncio 和协程如何在 Python 中工作以及如何在您的项目中最好地使用它们。
    
    **您觉得本指南有用吗？**
    
    我很想知道，请在下面的评论中分享一句好话。
    
    **你在项目中使用过 asyncio 吗？**
    
    我很想听听，请在评论中告诉我。
    
    **你有任何问题吗？**
    
    在下面的评论中留下您的问题，我会快速回复并提供我最好的建议。
    
    在 [reddit](https://www.reddit.com/r/Python/comments/yqrr94/python_asyncio_the_complete_guide/) 和 [hackernews](https://news.ycombinator.com/item?id=33547323) 中加入讨论。
