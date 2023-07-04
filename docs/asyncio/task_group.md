# 如何使用 asyncio.TaskGroup

转自: <https://superfastpython.com/asyncio-taskgroup/>

=== "中文"

    您可以使用 **asyncio.TaskGroup** 类将 **asyncio.Task** 对象集合作为一个组进行管理。
    
    **asyncio.TaskGroup** 将允许创建任务、跟踪已发出的任务、在一个任务失败时取消所有任务，并允许一起等待组中的所有任务。

    在本教程中，您将了解如何在 Python 中使用 **asyncio.TaskGroup**。

    让我们开始吧。

=== "原文"

    **How to use asyncio.TaskGroup**

    You can manage a collection of **asyncio.Task** objects as a group using the **asyncio.TaskGroup** class.
    
    The **asyncio.TaskGroup** will allow tasks to be created, keep track of issued tasks, cancel all tasks if one task fails, and allow all tasks in the group to be awaited together.
    
    In this tutorial, you will discover how to use the **asyncio.TaskGroup** in Python.
    
    Let’s get started.

## 需要将多个协程作为一个组进行管理

=== "中文"

    发出许多协程然后将它们作为一个组进行管理是很常见的。
    
    将多个协程视为一个组可以实现以下功能：

    1. 等待所有任务完成。
    2. 如果一项任务失败，则取消所有任务。
    3. 处理任何任务中引发的异常。

    在 Python 3.11 之前，有两种主要方法将多个协程作为一个组进行处理，它们是：
    
    - 调用 **asyncio.gather()**
    - 调用 **asyncio.wait()**

=== "原文"

    **Need To Manage Multiple Coroutines as a Group**

    It is common to issue many coroutines and then manage them as a group.
    
    Treating multiple coroutines as a group allows for functionality such as:
    
    1. Waiting until all tasks are completed.
    2. Canceling all tasks if one task fails.
    3. Handling an exception raised in any task.
    
    Prior to Python 3.11, there were two main approaches to handling multiple coroutines as a group, they were:
    
    - Call **asyncio.gather()**
    - Call **asyncio.wait()**

### 使用 asyncio.gather() 管理多个协程

=== "中文"

    **asyncio.gather()** 函数再接受一个协程或 **asyncio.Task** 对象。
    
    它返回一个 **Future** 对象，该对象允许将任务组与取消所有任务和等待所有任务等功能一起管理。

    您可以在教程中了解有关如何使用 **asyncio.gather()** 函数的更多信息：
    
    - [如何在 Python 中使用 asyncio.gather()](https://superfastpython.com/asyncio-gather/)
    
    如果组中的一项任务因异常而失败，则可以使用取消组中的所有任务。

    您可以在教程中看到这样的示例：
    
    - [Asyncio Gather() 如果一个任务失败则取消所有任务](https://superfastpython.com/asyncio-gather-cancel-all-if-one-fails/)

=== "原文"

    **Manage Multiple Coroutines with asyncio.gather()**

    The **asyncio.gather()** function takes one more coroutine or **asyncio.Task** objects.
    
    It returns a **Future** object that allows the group of tasks to be managed together with features such as canceling all tasks and waiting on all tasks.
    
    You can learn more about how to use the **asyncio.gather()** function in the tutorial:
    
    - [How to Use asyncio.gather() in Python](https://superfastpython.com/asyncio-gather/)
    
    It is possible to use cancel all tasks in the group if one task in the group fails with an exception.
    
    You can see an example of this in the tutorial:
    
    - [Asyncio gather() Cancel All Tasks if One Task Fails](https://superfastpython.com/asyncio-gather-cancel-all-if-one-fails/)

### 使用 asyncio.wait() 管理多个协程

=== "中文"

    asyncio.wait() 函数接受协程或任务的集合，并返回满足指定条件的任务集，例如一个完成、全部完成或第一个失败。

    您可以在教程中了解有关如何使用 asyncio.wait() 函数的更多信息：
    
    - [如何在 Python 中使用 Asyncio wait()](https://superfastpython.com/asyncio-wait/)

    Python 3.11 版的发布引入了一种将多个协程或任务作为一个组进行管理的新方法，称为 **asyncio.TaskGroup**。

=== "原文"

    **Manage Multiple Coroutines with asyncio.wait()**

    The asyncio.wait() function takes a collection of coroutines or tasks and returns the set of tasks that meet the specified conditions, such as one completed, all completed or first to fail.
    
    You can learn more about how to use the asyncio.wait() function in the tutorial:
    
    - [How to Use Asyncio wait() in Python](https://superfastpython.com/asyncio-wait/)
  
    The release of Python version 3.11 introduced a new approach to managing multiple coroutines or tasks as a group, called the asyncio.TaskGroup.

## 如何使用 asyncio.TaskGroup 类

=== "中文"

    Python 3.11 引入了 asyncio.TaskGroup 任务来管理一组关联的 asyncio 任务。
    
    !!! info ""
    
        添加了 TaskGroup 类，这是一个异步上下文管理器，包含一组任务，在退出时将等待所有任务。 对于新代码，建议直接使用 **create_task()** 和 **Gather()**。
        
        — [Python 3.11 的新功能](https://docs.python.org/3/whatsnew/3.11.html)
    
    **asyncio.TaskGroup** 类旨在替代用于创建任务的 **asyncio.create_task()** 函数和用于等待一组任务的 **asyncio.gather()** 函数。
    
    从历史上看，我们使用 **asyncio.create_task()** 函数创建并发出一个协程作为 **asyncio.Task**。

    例如：
    
    ```python
    ...
    # 创建协程并将其作为任务发出
    task = asyncio.create_task(coro())
    ```
    
    这将创建一个新的 **asyncio.Task** 对象，并将其发送到 asyncio 事件循环以便尽快执行。
    
    然后我们可以选择等待任务并等待它完成。

    例如：
    
    ```python
    ...
    # 将协程作为任务发出并等待它们完成
    results = await asyncio.gather(coro1(), coro2(), coro2)
    ```
    
    **asyncio.TaskGroup** 可以执行这两项活动，但不是首选方法。
    
    !!! info ""
        
        保存一组任务的异步上下文管理器。 可以使用 create_task() 将任务添加到组中。 当上下文管理器退出时，所有任务都将等待。

        — [异步任务组](https://docs.python.org/3/library/asyncio-task.html#asyncio.TaskGroup)

=== "原文"

    **How to Use asyncio.TaskGroup**

    Python 3.11 introduce the asyncio.TaskGroup task for managing a group of associated asyncio task.
    
    !!! info ""
    
        Added the TaskGroup class, an asynchronous context manager holding a group of tasks that will wait for all of them upon exit. For new code this is recommended over using create_task() and gather() directly.
        
        — [WHAT’S NEW IN PYTHON 3.11](https://docs.python.org/3/whatsnew/3.11.html)
    
    The **asyncio.TaskGroup** class is intended as a replacement for the **asyncio.create_task()** function for creating tasks and the **asyncio.gather()** function for waiting on a group of tasks.
    
    Historically, we create and issue a coroutine as an **asyncio.Task** using the **asyncio.create_task()** function.
    
    For example:
    
    ```python
    ...
    # create and issue coroutine as task
    task = asyncio.create_task(coro())
    ```
    
    This creates a new **asyncio.Task** object and issues it to the asyncio event loop for execution as soon as it is able.
    
    We can then choose to await the task and wait for it to be completed.
    
    For example:
    
    ```python
    ...
    # issue coroutines as tasks and wait for them to complete
    results = await asyncio.gather(coro1(), coro2(), coro2)
    ```
    
    The asyncio.TaskGroup can perform both of these activities and is not the preferred approach.
    
    !!! info ""
        
        An asynchronous context manager holding a group of tasks. Tasks can be added to the group using create_task(). All tasks are awaited when the context manager exits.

        — [ASYNCIO TASK GROUPS](https://docs.python.org/3/library/asyncio-task.html#asyncio.TaskGroup)

### 如何创建 asyncio.TaskGroup

=== "中文"

    **asyncio.TaskGroup** 对象实现异步上下文管理器接口，这是该类的首选用法。
    
    这意味着该类的实例被创建并通过**“async with”**表达式使用。

    例如：
    
    ```python
    ...
    # 创建任务组
    async with asyncio.TaskGroup() as group:
        # ...
    ```
    
    如果您不熟悉 **“async with”** 表达式，请参阅教程：
    
    - [如何在 Python 中使用 **“async with”** 表达式](https://superfastpython.com/asyncio-async-with/)
    
    回想一下，异步上下文管理器实现了可以等待的 **\_\_aenter\_\_()** 和 **\_\_aexit\_\_()** 方法。
    
    对于 **asyncio.TaskGroup**，退出上下文管理器块时自动调用**asyncio.TaskGroup** 的 **\_\_aexit\_\_()** 方法并等待创建的所有任务 。

    这意味着正常或通过异常退出 **TaskGroup** 对象的块将自动等待，直到所有组任务完成。
    
    ```python
    ...
    # 创建任务组
    async with asyncio.TaskGroup() as group:
        # ...
    # 等待所有小组任务完成
    ```
    
    您可以在教程中了解有关异步上下文管理器的更多信息：
    
    - [Python 中的异步上下文管理器](https://superfastpython.com/asynchronous-context-manager/)

=== "原文"

    **How to Create an asyncio.TaskGroup**

    An asyncio.TaskGroup object implements the asynchronous context manager interface, and this is the preferred usage of the class.
    
    This means that an instance of the class is created and is used via the “async with” expression.
    
    For example:
    
    ```python
    ...
    # create a taskgroup
    async with asyncio.TaskGroup() as group:
        # ...
    ```
    
    If you are new to the **“async with”** expression, see the tutorial:
    
    - [How to Use the **“async with”** Expression in Python](https://superfastpython.com/asyncio-async-with/)
    
    Recall that an asynchronous context manager implements the **\_\_aenter\_\_()** and **\_\_aexit\_\_()** methods which can be awaited.
    
    In the case of the **asyncio.TaskGroup**, the **\_\_aexit\_\_()** method which is called automatically when the context manager block is exited will await all tasks created by the **asyncio.TaskGroup**.
    
    This means that exiting the **TaskGroup** object’s block normally or via an exception will automatically await until all group tasks are done.
    
    ```python
    ...
    # create a taskgroup
    async with asyncio.TaskGroup() as group:
        # ...
    # wait for all group tasks are done
    ```
    
    You can learn more about asynchronous context managers in the tutorial:
    
    - [Asynchronous Context Managers in Python](https://superfastpython.com/asynchronous-context-manager/)

### 如何使用 asyncio.TaskGroup 创建任务

=== "中文"

    我们可以通过 **asyncio.TaskGroup** 对象上的 **create_task()** 方法在任务组中创建任务。

    例如：
    
    ```python
    ...
    # 创建任务组
    async with asyncio.TaskGroup() as group:
        # 创建并发出任务
        task = group.create_task(coro())
    ```
    
    这将创建一个 **asyncio.Task** 对象并将其发送到 asyncio 事件循环来执行，就像 **asyncio.create_task()** 函数一样，只不过该任务与组关联。
    
    如果我们选择并得到结果，我们可以直接等待任务。

    例如：
    
    ```python
    ...
    # 创建任务组
    async with asyncio.TaskGroup() as group:
        # 创建并发出任务
        esult = await group.create_task(coro())
    ```
    
    使用 **asyncio.TaskGroup** 的好处是我们可以在组中发出多个任务并在其间执行代码。 例如检查结果或收集更多数据。

=== "原文"

    **How to Create Tasks Using asyncio.TaskGroup**

    We can create a task in the task group via the **create_task()** method on the **asyncio.TaskGroup** object.
    
    For example:
    
    ```python
    ...
    # create a taskgroup
    async with asyncio.TaskGroup() as group:
        # create and issue a task
        task = group.create_task(coro())
    ```
    
    This will create an **asyncio.Task** object and issue it to the asyncio event loop for execution, just like the **asyncio.create_task()** function, except that the task is associated with the group.
    
    We can await the task directly if we choose and get results.
    
    For example:
    
    ```python
    ...
    # create a taskgroup
    async with asyncio.TaskGroup() as group:
        # create and issue a task
        esult = await group.create_task(coro())
    ```
    
    The benefit of using the asyncio.TaskGroup is that we can issue multiple tasks in the group and execute code in between. such as checking results or gathering more data.

### 如何使用 asyncio.TaskGroup 等待任务

=== "中文"

    我们可以通过退出异步上下文管理器块来等待组中的所有任务。
    
    因此，任务会自动等待，不需要任何额外的操作。

    例如：
    
    ```python
    ...
    # 创建任务组
    async with asyncio.TaskGroup() as group:
        # ...
    # 等待所有小组任务完成
    ```
    
    如果这种行为不是首选，那么我们必须确保在退出上下文管理器之前所有任务都“完成(done)”（完成、取消或失败）。

=== "原文"

    **How to Wait on Tasks Using asyncio.TaskGroup**

    We can wait on all tasks in the group by exiting the asynchronous context manager block.
    
    As such, the tasks are awaited automatically and nothing additional is required.
    
    For example:
    
    ```python
    ...
    # create a taskgroup
    async with asyncio.TaskGroup() as group:
        # ...
    # wait for all group tasks are done
    ```
    
    If this behavior is not preferred, then we must ensure all tasks are “done” (finished, canceled, or failed) before exiting the context manager.

### 如果一项任务失败，如何使用 asyncio.TaskGroup 取消所有任务

=== "中文"

    如果组中的一项任务因异常而失败，则组中剩余的所有未完成的任务将被取消。
    
    这是自动执行的，不需要任何额外的代码。

    例如：
    
    ```python
    # 处理组中任何任务的失败
    try:
        ...
        # 创建任务组
        async with asyncio.TaskGroup() as group:
            # 创建并发出任务
            task1 = group.create_task(coro1())
            # 创建并发出任务
            task2 = group.create_task(coro2())
            # 创建并发出任务
            task3 = group.create_task(coro3())
        # 等待所有小组任务完成
    except:
        # 所有未完成的任务都被取消
        pass
    ```
    
    如果这种行为不是首选，则必须在任务本身内管理每个任务的失败，例如 通过协程内的 try- except 块。
    
    现在我们知道如何使用 **asyncio.TaskGroup**，让我们看一些有效的示例。

=== "原文"

    **How to Cancel All Tasks If One Task Fails Using asyncio.TaskGroup**
    
    If one task in the group fails with an exception, then all non-done tasks remaining in the group will be canceled.
    
    This is performed automatically and does not require any additional code.
    
    For example:
    
    ```python
    # handle the failure of any tasks in the group
    try:
        ...
        # create a taskgroup
        async with asyncio.TaskGroup() as group:
            # create and issue a task
            task1 = group.create_task(coro1())
            # create and issue a task
            task2 = group.create_task(coro2())
            # create and issue a task
            task3 = group.create_task(coro3())
        # wait for all group tasks are done
    except:
        # all non-done tasks are cancelled
        pass
    ```
    
    If this behavior is not preferred, then the failure of each task must be managed within the tasks themselves, e.g. by a try-except block within the coroutine.
    
    Now that we know how to use the asyncio.TaskGroup, let’s look at some worked examples.

## 使用TaskGroup等待多个任务的示例

=== "中文"

    我们可以探索在 **asyncio.TaskGroup** 中创建多个任务，然后等待所有任务完成的情况。
    
    这可以通过首先定义一组代表我们想要完成的任务的不同协程来实现。

    在本例中，我们将定义 3 个协程，每个协程报告不同的消息，然后休眠一秒钟。
    
    ```python
    # 协程任务
    async def task1():
        # 报告消息
        print('Hello from coroutine 1')
        # sleep 来模拟等待
        await asyncio.sleep(1)
     
    # 协程任务
    async def task2():
        # 报告消息
        print('Hello from coroutine 2')
        # sleep 来模拟等待
        await asyncio.sleep(1)
     
    # 协程任务
    async def task3():
        # 报告消息
        print('Hello from coroutine 3')
        # sleep 来模拟等待
        await asyncio.sleep(1)
    ```
    
    接下来，我们可以定义一个 **main()** 协程，它通过上下文管理器接口创建 **asyncio.TaskGroup**。
    
    ```python
    # 异步入口点
    async def main():
        # 创建任务组
        async with asyncio.TaskGroup() as group:
        # ...
    ```
    
    然后，我们可以创建每个协程并将其作为任务发送到事件循环中，尽管它们作为组的一部分收集在一起。
    
    ```python
    ...
    # 运行第一个任务
    group.create_task(task1())
    # 运行第二个任务
    group.create_task(task2())
    # 运行第三个任务
    group.create_task(task3())
    ```
    
    请注意，我们不需要保留对 **asyncio.Task** 对象的引用，因为 **asyncio.TaskGroup** 将为我们跟踪它们。
    
    另外，请注意，我们不需要等待任务，因为当我们退出 **asyncio.TaskGroup** 的上下文管理器块时，我们将等待组中的所有任务。

    将它们结合在一起，下面列出了完整的示例。
    
    ```python
    # asyncio 任务组示例
    import asyncio
     
    # 协程任务
    async def task1():
        # 报告消息
        print('Hello from coroutine 1')
        # sleep 来模拟等待
        await asyncio.sleep(1)
     
    # 协程任务
    async def task2():
        # 报告消息
        print('Hello from coroutine 2')
        # sleep 来模拟等待
        await asyncio.sleep(1)
     
    # 协程任务
    async def task3():
        # 报告消息
        print('Hello from coroutine 3')
        # sleep 来模拟等待
        await asyncio.sleep(1)
     
    # 异步入口点
    async def main():
        # 创建任务组
        async with asyncio.TaskGroup() as group:
            # 运行第一个任务
            group.create_task(task1())
            # 运行第二个任务
            group.create_task(task2())
            # 运行第三个任务
            group.create_task(task3())
        # 等待所有任务完成...
        print('Done')
     
    # 入口点
    asyncio.run(main())
    ```
    
    运行示例首先执行 **main()** 协程，为我们启动一个新的事件循环。
    
    **main()** 协程运行并创建一个 **asyncio.TaskGroup**。

    然后，所有三个协程都创建为 **asyncio.Task** 对象，并通过 **asyncio.TaskGroup** 发送到事件循环。

    **asyncio.TaskGroup** 的上下文管理器块已退出，它会自动等待所有三个任务。

    任务报告它们的消息并休眠。

    所有任务完成后，main() 协程会报告最终消息。
    
    ```text
    Hello from coroutine 1
    Hello from coroutine 2
    Hello from coroutine 3
    Done
    ```
    
    接下来，让我们探讨如何将 **asyncio.TaskGroup** 与接受参数和返回值的任务一起使用。

=== "原文"

    **Example of Waiting on Multiple Tasks with a TaskGroup**
    
    We can explore the case of creating multiple tasks within an **asyncio.TaskGroup** and then waiting for all tasks to complete.
    
    This can be achieved by first defining a suite of different coroutines that represent the tasks we want to complete.
    
    In this case, we will define 3 coroutines that each report a different message and then sleep for one second.
    
    ```python
    # coroutine task
    async def task1():
        # report a message
        print('Hello from coroutine 1')
        # sleep to simulate waiting
        await asyncio.sleep(1)
     
    # coroutine task
    async def task2():
        # report a message
        print('Hello from coroutine 2')
        # sleep to simulate waiting
        await asyncio.sleep(1)
     
    # coroutine task
    async def task3():
        # report a message
        print('Hello from coroutine 3')
        # sleep to simulate waiting
        await asyncio.sleep(1)
    ```
    
    Next, we can define a **main()** coroutine that creates the **asyncio.TaskGroup** via the context manager interface.
    
    ```python
    # asyncio entry point
    async def main():
        # create task group
        async with asyncio.TaskGroup() as group:
        # ...
    ```
    
    We can then create and issue each coroutine as a task into the event loop, although collected together as part of the group.
    
    ```python
    ...
    # run first task
    group.create_task(task1())
    # run second task
    group.create_task(task2())
    # run third task
    group.create_task(task3())
    ```
    
    Notice that we don’t need to keep a reference to the **asyncio.Task** objects as the **asyncio.TaskGroup** will keep track of them for us.
    
    Also, notice that we don’t need to await the tasks because when we exit the context manager block for the **asyncio.TaskGroup** we will await all tasks in the group.
    
    Tying this together, the complete example is listed below.
    
    ```python
    # example of asyncio task group
    import asyncio
     
    # coroutine task
    async def task1():
        # report a message
        print('Hello from coroutine 1')
        # sleep to simulate waiting
        await asyncio.sleep(1)
     
    # coroutine task
    async def task2():
        # report a message
        print('Hello from coroutine 2')
        # sleep to simulate waiting
        await asyncio.sleep(1)
     
    # coroutine task
    async def task3():
        # report a message
        print('Hello from coroutine 3')
        # sleep to simulate waiting
        await asyncio.sleep(1)
     
    # asyncio entry point
    async def main():
        # create task group
        async with asyncio.TaskGroup() as group:
            # run first task
            group.create_task(task1())
            # run second task
            group.create_task(task2())
            # run third task
            group.create_task(task3())
        # wait for all tasks to complete...
        print('Done')
     
    # entry point
    asyncio.run(main())
    ```
    
    Running the example first executes the **main()** coroutine, starting a new event loop for us.
    
    The **main()** coroutine runs and creates an **asyncio.TaskGroup**.
    
    All three coroutines are then created as **asyncio.Task** objects and issued to the event loop via the **asyncio.TaskGroup**.
    
    The context manager block for the **asyncio.TaskGroup** is exited which automatically awaits all three tasks.
    
    The tasks report their message and sleep.
    
    Once all tasks are completed the main() coroutine reports a final message.
    
    ```text
    Hello from coroutine 1
    Hello from coroutine 2
    Hello from coroutine 3
    Done
    ```
    
    Next, let’s explore how we might use an **asyncio.TaskGroup** with tasks that take arguments and return values.

## 带有参数和返回值的任务组示例

=== "中文"

    我们可以探索将协程执行为带有参数和返回值的任务的情况。
    
    这些就像我们通常在没有 **asyncio.TaskGroup** 的情况下作为任务发出的协程一样，但最好有一个示例可供参考。

    在本例中，我们将定义一个任务，该任务接受一个参数，休眠，然后返回该参数乘以 100。
    
    ```python
    # 协程任务
    async def task(value):
        # sleep 来模拟等待
        await asyncio.sleep(1)
        # 返回值
        return value * 100
    ```
    
    然后，主协程将创建一个 **asyncio.TaskGroup**，然后创建该任务的 9 个实例，并将值 1 到 9 作为参数传递给该任务。
    
    保留任务对象，以便我们稍后可以从中检索值。 这是使用列表理解来实现的。

    所有任务完成后，将检索并报告返回值。
    
    ```python
    # 异步入口点
    async def main():
        # 创建任务组
        async with asyncio.TaskGroup() as group:
            # 创建并发布任务
            tasks = [group.create_task(task(i)) for i in range(1,10)]
        # 等待所有任务完成...
        # 报告所有结果
        for t in tasks:
            print(t.result())
    ```
    
    将它们结合在一起，下面列出了完整的示例。
    
    ```python
    # 具有返回值的 asyncio 任务组示例
    import asyncio
     
    # 协程任务
    async def task(value):
        # sleep 来模拟等待
        await asyncio.sleep(1)
        # 返回值
        return value * 100
     
    # 异步入口点
    async def main():
        # 创建任务组
        async with asyncio.TaskGroup() as group:
            # 创建并发布任务
            tasks = [group.create_task(task(i)) for i in range(1,10)]
        # 等待所有任务完成...
        # 报告所有结果
        for t in tasks:
            print(t.result())
     
    # 入口点
    asyncio.run(main())
    ```
    
    运行示例首先执行 **main()** 协程，为我们启动一个新的事件循环。
    
    **main()** 协程运行并创建一个 **asyncio.TaskGroup**。

    总共 9 个协程通过 **asyncio.TaskGroup** 作为任务发出，并且 **asyncio.Task** 对象存储在列表中。

    然后 main() 协程等待所有任务。

    每个任务运行、休眠，然后返回其输入参数的一百倍。

    所有任务完成后，将迭代 **asyncio.Task** 对象并报告每个对象的返回值。

    这展示了我们如何将参数传递给通过 **asyncio.TaskGroup** 创建的任务，以及如何跟踪 **asyncio.Task** 对象，以便在稍后阶段手动检索每个任务的结果。
    
    ```text
    100
    200
    300
    400
    500
    600
    700
    800
    900
    ```
    
    接下来我们看一个例子，如果一个任务失败，则取消组中的所有任务。

=== "原文"

    **Example of TaskGroup with Arguments and Return Values**

    We can explore the case of executing coroutines as tasks that take arguments and return values.
    
    These are just like coroutines we might issue normally as tasks without the **asyncio.TaskGroup**, but it is good to have an example for reference.
    
    In this case, we will define a task that takes an argument, sleeps, then returns the argument multiplied by 100.
    
    ```python
    # coroutine task
    async def task(value):
        # sleep to simulate waiting
        await asyncio.sleep(1)
        # return value
        return value * 100
    ```
    
    The main coroutine will then create an **asyncio.TaskGroup** and then create 9 instances of the task, passing the value 1 to 9 as arguments to the task.
    
    The task objects are kept so we can retrieve the values from them later. This is achieved using a list comprehension.
    
    Once all tasks are complete, the return values are retrieved and reported.
    
    ```python
    # asyncio entry point
    async def main():
        # create task group
        async with asyncio.TaskGroup() as group:
            # create and issue tasks
            tasks = [group.create_task(task(i)) for i in range(1,10)]
        # wait for all tasks to complete...
        # report all results
        for t in tasks:
            print(t.result())
    ```
    
    Tying this together, the complete example is listed below.
    
    ```python
    # example of asyncio task group with return values
    import asyncio
     
    # coroutine task
    async def task(value):
        # sleep to simulate waiting
        await asyncio.sleep(1)
        # return value
        return value * 100
     
    # asyncio entry point
    async def main():
        # create task group
        async with asyncio.TaskGroup() as group:
            # create and issue tasks
            tasks = [group.create_task(task(i)) for i in range(1,10)]
        # wait for all tasks to complete...
        # report all results
        for t in tasks:
            print(t.result())
     
    # entry point
    asyncio.run(main())
    ```
    
    Running the example first executes the **main()** coroutine, starting a new event loop for us.
    
    The **main()** coroutine runs and creates an **asyncio.TaskGroup**.
    
    A total of 9 coroutines are issued as tasks via the **asyncio.TaskGroup** and the **asyncio.Task** objects are stored in a list.
    
    The main() coroutine then awaits all tasks.
    
    Each task runs, sleeps, then returns its input argument multiples by one hundred.
    
    Once all tasks are complete, the **asyncio.Task** objects are iterated and the return value is reported from each.
    
    This shows how we might pass arguments to tasks created via the **asyncio.TaskGroup** and how we might keep track of **asyncio.Task** objects in order to manually retrieve results from each task at a later stage.
    
    ```text
    100
    200
    300
    400
    500
    600
    700
    800
    900
    ```
    
    Next, let’s look at an example of canceling all tasks in the group if one task fails.

## 使用 TaskGroup 如果一项任务失败则取消所有任务的示例

=== "中文"

    我们可以探讨如果一个任务失败则取消 **asyncio.TaskGroup** 中的所有任务的情况。
    
    失败的任务意味着协程在 **asyncio.Task** 对象中执行，该对象引发协程中未处理的异常，这意味着它会冒泡到任务并导致任务提前停止。

    发出许多任务并在一项或多项任务失败时取消所有任务是很常见的。

    **asyncio.TaskGroup** 将为我们自动执行此操作。

    在本例中，我们将定义 3 个不同的协程来报告消息和睡眠。 然后，第二个协程将失败并出现未捕获的异常。
    
    ```python
    # 协程任务
    async def task1():
        # 报告消息
        print('Hello from coroutine 1')
        # sleep 来模拟等待
        await asyncio.sleep(1)
     
    # 协程任务
    async def task2():
        # 报告消息
        print('Hello from coroutine 2')
        # sleep 来模拟等待
        await asyncio.sleep(0.5)
        # sleep 来模拟等待
        raise Exception('Something bad happened')
     
    # 协程任务
    async def task3():
        # 报告消息
        print('Hello from coroutine 3')
        # sleep 来模拟等待
        await asyncio.sleep(1)
    ```
    
    请注意，在引发异常之前，第二个任务的睡眠时间少于其他两个任务。
    
    这是为了确保在第二个任务失败时其他两个任务仍在运行，以便我们可以看到它们是否如我们预期的那样被取消。

    **main()** 协程将通过 **asyncio.TaskGroup** 发出所有任务，然后在所有任务“完成”后依次报告每个任务的完成和取消状态。

    回想一下，“完成”任务是指正常完成、取消或因异常而失败的任务。

    ```python
    # 异步入口点
    async def main():
        # 处理异常
        try:
            # 创建任务组
            async with asyncio.TaskGroup() as group:
                # 运行第一个任务
                t1 = group.create_task(task1())
                # 运行第二个任务
                t2 = group.create_task(task2())
                # 运行第三个任务
                t3 = group.create_task(task3())
        except:
            pass
        # 检查每个任务的状态
        print(f'Task1: done={t1.done()}, cancelled={t1.cancelled()}')
        print(f'Task2: done={t2.done()}, cancelled={t2.cancelled()}')
        print(f'Task3: done={t3.done()}, cancelled={t3.cancelled()}')
    ```
    
    请注意，我们将整个 **asyncio.TaskGroup** 包装在异常中，因为任务中发生的任何未捕获的异常都会由 **asyncio.TaskGroup** 重新引发

    将它们结合在一起，下面列出了完整的示例。

    ```python
    # 任务失败的 asyncio 任务组示例
    import asyncio
     
    # 协程任务
    async def task1():
        # 报告消息
        print('Hello from coroutine 1')
        # sleep 来模拟等待
        await asyncio.sleep(1)
     
    # 协程任务
    async def task2():
        # 报告消息
        print('Hello from coroutine 2')
        # sleep 来模拟等待
        await asyncio.sleep(0.5)
        # 因异常而失败
        raise Exception('Something bad happened')
     
    # 协程任务
    async def task3():
        # 报告消息
        print('Hello from coroutine 3')
        # sleep 来模拟等待
        await asyncio.sleep(1)
     
    # 异步入口点
    async def main():
        # 处理异常
        try:
            # 创建任务组
            async with asyncio.TaskGroup() as group:
                # 运行第一个任务
                t1 = group.create_task(task1())
                # 运行第二个任务
                t2 = group.create_task(task2())
                # 运行第三个任务
                t3 = group.create_task(task3())
        except:
            pass
        # 检查每个任务的状态
        print(f'Task1: done={t1.done()}, cancelled={t1.cancelled()}')
        print(f'Task2: done={t2.done()}, cancelled={t2.cancelled()}')
        print(f'Task3: done={t3.done()}, cancelled={t3.cancelled()}')
     
    # 入口点
    asyncio.run(main())
    ```
    
    运行示例首先执行 **main()** 协程，为我们启动一个新的事件循环。
    
    **main()** 协程运行并创建一个 **asyncio.TaskGroup**。

    然后，这三个协程通过 **asyncio.TaskGroup** 作为任务发出，并且 **asyncio.Task** 对象保存在局部变量中以供稍后使用。

    **asyncio.TaskGroup** 上下文管理器块退出，然后 **main()** 协程等待所有三个任务。

    任务运行、报告消息并休眠。 然后第二个协程失败并出现异常。

    **asyncio.TaskGroup** 处理异常并取消所有剩余的未完成任务。 然后，该异常在顶层重新引发并被忽略。

    然后报告每个任务的完成和取消状态。 我们可以看到所有任务都已完成，并且任务 2 因异常失败时正在运行的两个任务（1 和 3）确实被取消。

    这突出显示了如果组中的任务因未处理的异常而失败，则将如何取消组中的所有正在运行的任务。

    可以防止任务被取消。 您可以在教程中了解更多相关信息：
    
    - [Asyncio 防止取消](https://superfastpython.com/asyncio-shield/)
    
    ```python
    Hello from coroutine 1
    Hello from coroutine 2
    Hello from coroutine 2
    Task1: done=True, cancelled=True
    Task2: done=True, cancelled=False
    Task3: done=True, cancelled=True
    ```
    
    接下来我们看一个手动取消组内一项任务的例子。

=== "原文"

    **Example of Cancelling All Tasks if One Task Fails Using TaskGroup**

    We can explore the case of canceling all tasks in the **asyncio.TaskGroup** if one task fails.
    
    A failed task means that a coroutine is executed in an **asyncio.Task** object that raises an exception that is not handled in the coroutine, meaning that it bubbles up to the task and causes the task to be halted early.
    
    It is common to issue many tasks and cancel all tasks if one or more of the tasks fails.
    
    The **asyncio.TaskGroup** will perform this action automatically for us.
    
    In this case, we will define 3 different coroutines that report a message and sleep. The second coroutine will then fail with an uncaught exception.

    ```python
    # coroutine task
    async def task1():
        # report a message
        print('Hello from coroutine 1')
        # sleep to simulate waiting
        await asyncio.sleep(1)
     
    # coroutine task
    async def task2():
        # report a message
        print('Hello from coroutine 2')
        # sleep to simulate waiting
        await asyncio.sleep(0.5)
        # fail with an exception
        raise Exception('Something bad happened')
     
    # coroutine task
    async def task3():
        # report a message
        print('Hello from coroutine 2')
        # sleep to simulate waiting
        await asyncio.sleep(1)
    ```
    
    Note that the second task sleeps less than the other two tasks before raising an exception.
    
    This is to ensure that the other two tasks are still running at the point that the second task fails so that we can see if they are canceled as we expect.
    
    The **main()** coroutine will issue all tasks via the **asyncio.TaskGroup** and then report the done and cancel status of each in turn once all tasks are “done”.
    
    Recall a “done” task is a task that is finished normally, canceled, or failed with an exception.

    ```python
    # asyncio entry point
    async def main():
        # handle exceptions
        try:
            # create task group
            async with asyncio.TaskGroup() as group:
                # run first task
                t1 = group.create_task(task1())
                # run second task
                t2 = group.create_task(task2())
                # run third task
                t3 = group.create_task(task3())
        except:
            pass
        # check the status of each task
        print(f'Task1: done={t1.done()}, cancelled={t1.cancelled()}')
        print(f'Task2: done={t2.done()}, cancelled={t2.cancelled()}')
        print(f'Task3: done={t3.done()}, cancelled={t3.cancelled()}')
    ```
    
    Notice that we wrap the entire **asyncio.TaskGroup** in an exception as any uncaught exception that occurs in a task is re-raised by the **asyncio.TaskGroup**
    
    Tying this together, the complete example is listed below.

    ```python
    # example of asyncio task group with a failed task
    import asyncio
     
    # coroutine task
    async def task1():
        # report a message
        print('Hello from coroutine 1')
        # sleep to simulate waiting
        await asyncio.sleep(1)
     
    # coroutine task
    async def task2():
        # report a message
        print('Hello from coroutine 2')
        # sleep to simulate waiting
        await asyncio.sleep(0.5)
        # fail with an exception
        raise Exception('Something bad happened')
     
    # coroutine task
    async def task3():
        # report a message
        print('Hello from coroutine 2')
        # sleep to simulate waiting
        await asyncio.sleep(1)
     
    # asyncio entry point
    async def main():
        # handle exceptions
        try:
            # create task group
            async with asyncio.TaskGroup() as group:
                # run first task
                t1 = group.create_task(task1())
                # run second task
                t2 = group.create_task(task2())
                # run third task
                t3 = group.create_task(task3())
        except:
            pass
        # check the status of each task
        print(f'Task1: done={t1.done()}, cancelled={t1.cancelled()}')
        print(f'Task2: done={t2.done()}, cancelled={t2.cancelled()}')
        print(f'Task3: done={t3.done()}, cancelled={t3.cancelled()}')
     
    # entry point
    asyncio.run(main())
    ```
    
    Running the example first executes the **main()** coroutine, starting a new event loop for us.
    
    The **main()** coroutine runs and creates an **asyncio.TaskGroup**.
    
    The three coroutines are then issued as tasks via the **asyncio.TaskGroup** and the **asyncio.Task** objects are kept in local variables for later.
    
    The **asyncio.TaskGroup** context manager block is exited and the **main()** coroutine then awaits all three tasks.
    
    The tasks run, report a message and sleep. The second coroutine then fails with an exception.
    
    The **asyncio.TaskGroup** handles the exception and cancels all remaining not-done tasks. The exception is then re-raised at the top level and ignored.
    
    The done and canceled status of each task is then reported. We can see that all tasks are done and that the two tasks (1 and 3) that were running at the time task 2 failed with an exception were indeed canceled.
    
    This highlights how all running tasks in the group will be canceled if a task in the group fails with an unhanded exception.
    
    It is possible to shield a task from cancellation. You can learn more about this in the tutorial:
    
    - [Asyncio Shield From Cancellation](https://superfastpython.com/asyncio-shield/)

    ```text
    Hello from coroutine 1
    Hello from coroutine 2
    Hello from coroutine 2
    Task1: done=True, cancelled=True
    Task2: done=True, cancelled=False
    Task3: done=True, cancelled=True
    ```
    
    Next, let’s look at an example of manually canceling one task in the group.

## 取消任务组中的一项任务的示例

=== "中文"

    我们可以探讨手动取消组中一项任务的情况。
    
    这可以通过调用 **asyncio.Task** 对象上的 **cancel()** 方法来实现。

    如果任务没有完成，取消任务的请求将由任务处理。

    您可以在教程中了解有关取消任务的更多信息：
    
    - [如何取消 Asyncio 任务](https://superfastpython.com/asyncio-cancel-task/)
    
    在本例中，我们将使用 **asyncio.TaskGroup** 发出 3 个任务，稍等片刻，然后取消第二个任务。
    
    预期只有第二个任务会被取消，所有其他任务将继续正常运行。 我们将在所有任务完成后通过报告所有任务的“已完成”和“已取消”状态来确认这一点。

    ```python
    # 异步入口点
    async def main():
        # 创建任务组
        async with asyncio.TaskGroup() as group:
            # 运行第一个任务
            t1 = group.create_task(task1())
            # 运行第二个任务
            t2 = group.create_task(task2())
            # 运行第三个任务
            t3 = group.create_task(task3())
            # 暂停片刻
            await asyncio.sleep(0.5)
            # 取消第二个任务
            t2.cancel()
        # 检查每个任务的状态
        print(f'Task1: done={t1.done()}, cancelled={t1.cancelled()}')
        print(f'Task2: done={t2.done()}, cancelled={t2.cancelled()}')
        print(f'Task3: done={t3.done()}, cancelled={t3.cancelled()}')
    ```
    
    将它们结合在一起，下面列出了完整的示例。

    ```python
    # 具有已取消任务的 asyncio 任务组示例
    import asyncio
     
    # 协程任务
    async def task1():
        # sleep 来模拟等待
        await asyncio.sleep(1)
        # 报告消息
        print('Hello from coroutine 1')
     
    # 协程任务
    async def task2():
        # sleep 来模拟等待
        await asyncio.sleep(1)
        # 报告消息
        print('Hello from coroutine 2')
     
    # 协程任务
    async def task3():
        # sleep 来模拟等待
        await asyncio.sleep(1)
        # 报告消息
        print('Hello from coroutine 3')
     
    # 异步入口点
    async def main():
        # 创建任务组
        async with asyncio.TaskGroup() as group:
            # 运行第一个任务
            t1 = group.create_task(task1())
            # 运行第二个任务
            t2 = group.create_task(task2())
            # 运行第三个任务
            t3 = group.create_task(task3())
            # 暂停片刻
            await asyncio.sleep(0.5)
            # 取消第二个任务
            t2.cancel()
        # 检查每个任务的状态
        print(f'Task1: done={t1.done()}, cancelled={t1.cancelled()}')
        print(f'Task2: done={t2.done()}, cancelled={t2.cancelled()}')
        print(f'Task3: done={t3.done()}, cancelled={t3.cancelled()}')
     
    # 入口点
    asyncio.run(main())
    ```
    
    运行示例首先执行 **main()** 协程，为我们启动一个新的事件循环。
    
    **main()** 协程运行并创建一个 **asyncio.TaskGroup**。

    然后，这三个协程通过 **asyncio.TaskGroup** 作为任务发出，并且 **asyncio.Task** 对象保存在局部变量中以供稍后使用。

    **main()** 协程会休眠一会儿，以允许任务运行。

    **main()** 协程结果然后取消第二个任务。 然后它退出 **asyncio.TaskGroup** 的上下文管理器并等待所有任务。

    第二个任务被取消。 其余任务正常完成。 我们只能看到来自任务 1 和 3 的消息，因为任务 2 在报告消息之前已被取消。

    检查任务的状态，我们可以看到所有任务都已完成，只有任务 2 被取消。

    这凸显了我们可以手动取消组中的任务，而不影响其他任务。
    
    ```python
    Hello from coroutine 1
    Hello from coroutine 2
    Task1: done=True, cancelled=False
    Task2: done=True, cancelled=True
    Task3: done=True, cancelled=False
    ```

=== "原文"

    **Example of Cancelling One Task in a TaskGroup**

    We can explore the case of manually canceling one task in the group.
    
    This can be achieved by calling the **cancel()** method on the **asyncio.Task** object.
    
    If the task is not done, the request to cancel the task will be handled by the task.
    
    You can learn more about canceling tasks in the tutorial:
    
    - [How to Cancel an Asyncio Task](https://superfastpython.com/asyncio-cancel-task/)
    
    In this case, we will issue 3 tasks using the **asyncio.TaskGroup**, wait a moment, then cancel the second task.
    
    The expectation is that only the second task will be canceled and all other tasks will be left to run as per normal. We will confirm this by reporting the “done” and “cancelled” status of all tasks after all tasks are done.
    
    ```python
    # asyncio entry point
    async def main():
        # create task group
        async with asyncio.TaskGroup() as group:
            # run first task
            t1 = group.create_task(task1())
            # run second task
            t2 = group.create_task(task2())
            # run third task
            t3 = group.create_task(task3())
            # wait a moment
            await asyncio.sleep(0.5)
            # cancel the second task
            t2.cancel()
        # check the status of each task
        print(f'Task1: done={t1.done()}, cancelled={t1.cancelled()}')
        print(f'Task2: done={t2.done()}, cancelled={t2.cancelled()}')
        print(f'Task3: done={t3.done()}, cancelled={t3.cancelled()}')
    ```
    
    Tying this together, the complete example is listed below.
    
    ```python
    # example of asyncio task group with a canceled task
    import asyncio
     
    # coroutine task
    async def task1():
        # sleep to simulate waiting
        await asyncio.sleep(1)
        # report a message
        print('Hello from coroutine 1')
     
    # coroutine task
    async def task2():
        # sleep to simulate waiting
        await asyncio.sleep(1)
        # report a message
        print('Hello from coroutine 2')
     
    # coroutine task
    async def task3():
        # sleep to simulate waiting
        await asyncio.sleep(1)
        # report a message
        print('Hello from coroutine 2')
     
    # asyncio entry point
    async def main():
        # create task group
        async with asyncio.TaskGroup() as group:
            # run first task
            t1 = group.create_task(task1())
            # run second task
            t2 = group.create_task(task2())
            # run third task
            t3 = group.create_task(task3())
            # wait a moment
            await asyncio.sleep(0.5)
            # cancel the second task
            t2.cancel()
        # check the status of each task
        print(f'Task1: done={t1.done()}, cancelled={t1.cancelled()}')
        print(f'Task2: done={t2.done()}, cancelled={t2.cancelled()}')
        print(f'Task3: done={t3.done()}, cancelled={t3.cancelled()}')
     
    # entry point
    asyncio.run(main())
    ```
    
    Running the example first executes the **main()** coroutine, starting a new event loop for us.
    
    The **main()** coroutine runs and creates an **asyncio.TaskGroup**.
    
    The three coroutines are then issued as tasks via the **asyncio.TaskGroup** and the **asyncio.Task** objects are kept in local variables for later.
    
    The **main()** coroutine sleeps for a moment, allowing the tasks to run.
    
    The **main()** coroutine results and then cancels the second task. It then exits the context manager of the **asyncio.TaskGroup** and awaits all tasks.
    
    The second task is canceled. The remaining tasks complete normally. We see messages from tasks 1 and 3 only because task 2 was canceled before the message could be reported.
    
    Checking the status of the tasks, we can see that all tasks are done and only task 2 was canceled.
    
    This highlights that we can manually cancel tasks in the group, leaving other tasks unaffected.
    
    ```python
    Hello from coroutine 1
    Hello from coroutine 2
    Task1: done=True, cancelled=False
    Task2: done=True, cancelled=True
    Task3: done=True, cancelled=False
    ```

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
