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

=== "English"

We can wait for a task to finish by awaiting the asyncio.Task object directly.

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

We can wait for a task to finish by awaiting the asyncio.Task object directly.

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
