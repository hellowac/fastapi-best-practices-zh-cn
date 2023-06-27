# Python Asyncio: The Complete Guide

**Asyncio** allows us to use asynchronous programming with coroutine-based concurrency in Python.

Although asyncio has been available in Python for many years now, it remains one of the most interesting and yet one of the most frustrating areas of Python.

It is just plain hard to get started with asyncio for new developers.

This guide provides a detailed and comprehensive(全面的) review of asyncio in Python, including how to define, create and run coroutines, what is asynchronous programming, what is non-blocking-io, concurrency primitives(原始并发) used with coroutines, common questions, and best practices.

This is a massive 29,000+ word guide. You may want to bookmark it so you can refer to it as you develop your concurrent programs.

Let’s dive in.

## 1. What is Asynchronous Programming

### 1.1 Asynchronous Tasks

### 1.2 Asynchronous Programming

### 1.3 Asynchronous Programming in Python

## 2. What is Asyncio

### 2.1 Changes to Python to add Support for Coroutines

### 2.2 The asyncio Module

## 3. When to Use Asyncio

### 3.1 Reasons to Use Asyncio in Python

### 3.2 Other Reasons to Use Asyncio

### 3.3 When to Not Use Asyncio

## 4. Coroutines in Python

### 4.1 What is a Coroutine

### 4.2 Coroutine vs Routine and Subroutine

### 4.3 Coroutine vs Generator

### 4.4 Coroutine vs Task

### 4.5 Coroutine vs Thread

### 4.6 Coroutine vs Process

### 4.7 When Were Coroutines Added to Python

## 5. Define, Create and Run Coroutines

### 5.1 How to Define a Coroutine

### 5.2 How to Create a Coroutine

### 5.3 How to Run a Coroutine From Python

## 6. What is the Event Loop

### 6.1 What is the Asyncio Event Loop

### 6.2 How To Start and Get An Event Loop

### 6.3 What is an Event Loop Object

### 6.4 Why Get Access to The Event Loop

## 7. Create and Run Asyncio Tasks

### 7.1 What is an Asyncio Task

### 7.2 How to Create a Task

### 7.3 When Does a Task Run?

## 8. Work With and Query Tasks

### 8.1 Task Life-Cycle

### 8.2 How to Check Task Status

### 8.3 How to Get Task Result

### 8.4 How to Get Task Exception

### 8.5 How to Cancel a Task

### 8.6 How to Use Callback With a Task

### 8.7 How to Set the Task Name

## 9. Current and Running Tasks

### 9.1 How to Get the Current Task

### 9.2 How to Get All Tasks

## 10. Run Many Coroutines Concurrently

### 10.1 What is Asyncio gather()

### 10.2 How to use Asyncio gather()

### 10.3 Example of gather() For Many Coroutines in a List

## 11. Wait for A Collection of Tasks

### 11.1 What is asyncio.wait()

### 11.2 How to Use asyncio.wait()

### 11.3 Example of Waiting for All Tasks

## 12. Wait for a Coroutine with a Time Limit

### 12.1 What is Asyncio wait_for()

### 12.2 How to Use Asyncio wait_for()

### 12.3 Example of Asyncio wait_for() With a Timeout

## 13. Shield Tasks from Cancellation

### 13.1 What is Asyncio shield()

### 13.2 How to Use Asyncio shield()

### 13.3 Example of Asyncio shield() for a Task

## 14. Run a Blocking Task in Asyncio

### 14.1 Need to Run Blocking Tasks in Asyncio

### 14.2 How to Run Blocking Tasks

### 14.3 Example of Running I/O-Bound Task in Asyncio with to_thread()

## 15. Asynchronous Iterators

### 15.1 What Are Asynchronous Iterators

### 15.2 What is the “async for” loop?

### 15.3 How to Use Asynchronous Iterators

### 15.4 Example of an Asynchronous Iterator

## 16. Asynchronous Generators

### 16.1 What Are Asynchronous Generators

### 16.2 How to Use an Asynchronous Generator

### 16.3 Example of an Asynchronous Generator

## 17. Asynchronous Context Managers

### 17.1 What is an Asynchronous Context Manager

### 17.2 How to Use Asynchronous Context Managers

### 17.3 Example of an Asynchronous Context Manager and “async with”

## 18. Asynchronous Comprehensions

### 18.1 What are Asynchronous Comprehensions

### 18.2 Comprehensions

### 18.3 Asynchronous Comprehensions

### 18.4 Await Comprehensions

## 19. Run Commands in Non-Blocking Subprocesses

### 19.1 What is asyncio.subprocess.Process

### 19.2 How to Run a Command Directly

### 19.3 How to Run a Command Via the Shell

## 20. Non-Blocking Streams

### 20.1 Asyncio Streams

### 20.2 How to Open a Connection

### 20.3 How to Start a Server

### 20.4 How to Write Data with the StreamWriter

### 20.5 How to Read Data with the StreamReader

### 20.6 How to Close Connection

## 21. Example of Checking Website Status

### 21.1 How to Check HTTP Status with Asyncio

### 21.2 Open HTTP Connection

### 21.3 Write HTTP Request

### 21.4 Read HTTP Response

### 21.5 Close HTTP Connection

### 21.6 Example of Checking HTTP Status Sequentially

### 21.7 Example of Checking Website Status Concurrently

## 22. Python Asyncio Common Errors

### 22.1 Error 1: Trying to Run Coroutines by Calling Them

### 22.2 Error 2: Not Letting Coroutines Run in the Event Loop

### 22.3 Error 3: Using the Low-Level Asyncio API

### 22.4 Error 4: Exiting the Main Coroutine Too Early

### 22.5 Error 5: Assuming Race Conditions and Deadlocks are Impossible

## 23. Python Asyncio Common Questions

### 23.1 How to Stop a Task?

### 23.2 How to Wait for a Task To Finish?

### 23.3 How to Get a Return Value from a Task?

### 23.4 How to Run a Task in the Background?

### 23.5 How to Wait for All Background Tasks?

### 23.6 Does a Running Task Stop the Event Loop from Exiting?

### 23.7 How to Show Progress of Running Tasks?

### 23.8 How to Run a Task After a Delay?

### 23.9 How to Run a Follow-Up Task?

### 23.10 How to Execute a Blocking I/O or CPU-bound Function in Asyncio?

The asyncio module provides two approaches for executing blocking calls in asyncio programs.

- asyncio 模块提供了两种在 asyncio 程序中执行阻塞调用的方法。

The first is to use the [asyncio.to_thread()](https://docs.python.org/3/library/asyncio-task.html#asyncio.to_thread) function.

- 第一种是使用 [asyncio.to_thread()](https://docs.python.org/3/library/asyncio-task.html#asyncio.to_thread) 函数。

This is in the high-level API and is intended for application developers.

- 这是高级 API 中的内容，适用于应用程序开发人员。

The [asyncio.to_thread()](https://docs.python.org/3/library/asyncio-task.html#asyncio.to_thread) function takes a function name to execute and any arguments.

- `asyncio.to_thread()` 函数接受要执行的函数名称和任何参数。

The function is executed in a separate thread. It returns a coroutine that can be awaited or scheduled as an independent task.

- 该函数在单独的线程中执行。 它返回一个可以作为独立任务等待或调度的协程。

For example:

- 例如：

```python
...
# execute a function in a separate thread
# 在单独的线程中执行函数
await asyncio.to_thread(task)
```

The task will not begin executing until the returned coroutine is given an opportunity to run in the event loop.

- 任务一开始并不会执行。直到协程返回并且给个在事件循环中运行的机会时，才会运行。

The asyncio.to_thread() function creates a **ThreadPoolExecutor** behind the scenes to execute blocking calls.

- `asyncio.to_thread()` 函数在后台创建一个 **ThreadPoolExecutor** 来执行阻塞调用。

As such, the `asyncio.to_thread()` function is only appropriate for IO-bound tasks.

- 因此，`asyncio.to_thread()` 函数仅适用于 IO 密集型任务。

An alternative approach is to use the [loop.run_in_executor()](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.loop.run_in_executor) function.

- 另一种方法是使用[loop.run_in_executor()](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.loop.run_in_executor)函数。

This is in the low-level asyncio API and first requires access to the event loop, such as via the [asyncio.get_running_loop()](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.get_running_loop) function.

- 这是在低级 asyncio API 中，首先需要访问事件循环，例如通过 [asyncio.get_running_loop()](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.get_running_loop) 函数。

The `loop.run_in_executor()` function takes an executor and a function to execute.

- `Loop.run_in_executor()` 函数需要一个执行器和一个要执行的函数。

If None is provided for the executor, then the default executor is used, which is a ThreadPoolExecutor.

- 如果没有为执行器提供 None，则使用默认执行器，即 ThreadPoolExecutor。

The `loop.run_in_executor()` function returns an awaitable that can be awaited if needed. The task will begin executing immediately, so the returned awaitable does not need to be awaited or scheduled for the blocking call to start executing.

- `Loop.run_in_executor()` 函数返回一个可等待的对象，如果需要可以等待。 该任务将立即开始执行，因此不需要等待或安排返回的可等待对象来开始执行阻塞调用。

For example:

- 例如：

```python
...
# get the event loop
# 获取事件循环
loop = asyncio.get_running_loop()
# execute a function in a separate thread
# 在单独的线程中执行函数
await loop.run_in_executor(None, task)
```

Alternatively, an executor can be created and passed to the loop.run_in_executor() function, which will execute the asynchronous call in the executor.

- 或者，可以创建一个执行器并将其传递给`loop.run_in_executor()`函数，该函数将在执行器中执行异步调用。

The caller must manage the executor in this case, shutting it down once the caller is finished with it.

- 在这种情况下，调用者必须管理执行器，在调用者完成后将其关闭。

For example:

- 例如:

```python
...
# create a process pool
# 创建一个进程池
with ProcessPoolExecutor as exe:
    # get the event loop
    # 获取事件循环
    loop = asyncio.get_running_loop()
    # execute a function in a separate thread
    # 在单独的线程中执行函数
    await loop.run_in_executor(exe, task)
    # process pool is shutdown automatically...
    # 进程池自动关闭...
```

These two approaches allow a blocking call to be executed as an asynchronous task in an asyncio program.

- 这两种方法允许阻塞调用作为 asyncio 程序中的异步任务执行。

## 24. Common Objections to Using Asyncio

Asyncio and coroutines may not be the best solution for all concurrency problems in your program.

- 异步和协程可能不是解决程序中所有并发问题的最佳解决方案。

That being said, there may also be some misunderstandings that are preventing you from making full and best use of the capabilities of the asyncio in Python.

- 话虽如此，也可能存在一些误解，阻碍您充分、最佳地利用 Python 中 asyncio 的功能。

In this section, we review some of the common objections seen by developers when considering using the asyncio.

- 在本节中，我们将回顾开发人员在考虑使用 asyncio 时遇到的一些常见反对意见。

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
