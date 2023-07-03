# Asyncio 表达式及API备忘录

## 为什么是asyncio ?

Asyncio 提供适合非阻塞套接字 I/O 应用程序的基于协程的并发性。

## 协程

**导入**:

```python
import asyncio
```

**定义一个协程**:

```python
async def custom_coroutine():
    # ...
```

**创建协程对象**:

```python
coro = custom_coroutine()
```

**运行协程的入口**:

```python
asyncio.run(main())
```

**从协程中挂起和运行协程**:

```python
await custom_coroutine()
```

**sleep 一个协程**：

```python
await asyncio.sleep(1)
```

## 异步推导和循环

**异步推导式**：

```python
res = [r async for r in async_gen()]
```

**Await 推导式**：

```python
res = [r await a in awaitables]
```

**异步for循环**：

```python
async for item in async_gen():
    print(item)
```

## 任务(Task)

将任务作为协程并调度为独立运行。

**创建并调度一个任务**(高级)：

```python
task = asyncio.create_task(coro)
```

**创建并调度一个任务**(低级)：

```python
task = asyncio.create_task(coro)
```

**创建并调度一个任务**：

```python
task = asyncio.ensure_future(coro)
```

**挂起并等待任务完成**：

```python
await task
```

**获取当前任务**：

```python
task = asyncio.current_task()
```

**获取所有正在运行的任务**：

```python
tasks = asyncio.all_tasks()
```

**获取任务结果**：

```python
value = task.result()
```

**获取任务未处理的异常**：

```python
ex = task.exception()
```

**取消一个任务**：

```python
was_canceled = task.cancel()
```

**检查任务是否运行完毕(未运行)**：

```python
if task.done():
    # ...
```

**检查任务是否被取消**：

```python
if task.cancelled():
    # ...
```

**添加执行完后的回调函数**：

```python
task.add_done_callback(handler)
```

**删除任务的执行后的回调函数**：

```python
task.remove_done_callback(handler)
```

**设置和获取任务名称**：

```python
task.set_name("MyTask")
name = task.get_name()
```

## 操作任务合集

操作一个可等待任务合集、任务、或者任务合集

**带超时的使用Await**：

```python
try:
    await asyncio.wait_for(tk, timeout=1)
except asyncio.TimeoutError:
    # ...
```

**防止任务被取消**：

```python
shielded = asyncio.shield(task)
```

**在新线程中运行阻塞任务**：

```python
coro = asyncio.to_thread(myfunc)
```

**在asyncio事件循环中运行任务**：

```python
fut = run_coroutine_threadsafe(coro, loop)
```

**将许多可等待协程对象作为组运行**：

```python
await asyncio.gather(c1(), c2())
```

**等待一个集合中的所有协程任务**：

```python
done, pen = await asyncio.wait(tasks)
```

**带超时的等待一个集合中的所有协程任务**：

```python
try:
    done, pen = await asyncio.wait(tasks, timeout=5)
except asyncio.TimeoutError:
    # ...
```

**等待一个任务合集中的第一个任务完成**：

```python
done, pen = await asyncio.wait(tasks, return_when=FIRST_COMPLETED)
```

**等待一个任务合集中的第一个任务失败**：

```python
done, pen = await asyncio.wait(tasks, return_when=FIRST_EXCEPTION)
```

**按任务完成顺序获取结果**：

```python
for c in asyncio.as_completed(tasks):
    result = await c
```

## 非阻塞I/O的子进程

**在子进程中运行命令**：

```python
p = await create_subprocess_exec('ls')
```

**使用shell和子进程运行命令**：

```python
p = await create_subprocess_shell('ls')
```

**从子进程中读取数据**：

```python
await process.communicate(input=data)
```

**终止一个子进程**：

```python
process.terminate()
```

## 非阻塞I/O 流

**打开一个客户端TCP链接**：

```python
reader, writer = await open_connection('google.com', 80)
```

**启动一个TCP服务**：

```python
server = await start_server(hanler, '127.0.0.1', 9876)
```

**从套接字读取数据**：

```python
data = await reader.readline()
```

**往套接字写入数据**：

```python
writer.write(data)
```

**清空socket并等待就绪**：

```python
await writer.drain()
```

**关闭套接字连接**：

```python
writer.close()
await writer.wait_closed()
```

## 信号量和事件和条件

**信号量，设置 num 个位置**：

```python
semaphore = asyncio.Semaphore(10)
await semaphore.acquire()
# ...
semaphore.release()
```

**信号量, 上下文管理**：

```python
async with semaphore:
    # ...
```

**创建事件，并设置事件**：

```python
event = asyncio.Event()
event.set()
```

**检查事件是否设置**：

```python
if event.is_set():
    # ...
```

**等待事件被设置(阻塞)**：

```python
await event.wait()
```

**条件变量**：

```python
condition = asyncio.Condition()
await condition.acquired()
# ...
condition.release()
```

**等待条件被通知(阻塞)**：

```python
async with condition:
    await condition.wait()
```

**等待表达式的条件（阻塞）**：

```python
async with condition:
    await condition.wait_for(check)
```

**通知任何等待条件的单个线程**：

```python
async with condition:
    condition.notify(n=1)
```

**通知所有等待条件的线程**：

```python
async with condition:
    condition.notify_all()
```

## 异步锁(Async Locks)

**互斥锁**：

```python
lock = asyncio.Lock()
await lock.acquire()
# ...
lock.release()
```

**互斥锁和上下文管理器**：

```python
async with lock:
    # ...
```

## 队列

Queue 、LifoQueue， PriorityQueue

普通队列、后进先出队列、优先级队列

**创建队列**：

```python
queue = asyncio.Queue()
```

**创建队列并限制数量**：

```python
queue = asyncio.Queue(100)
```

**添加元素到队列中**(阻塞，如果限制了数量)：

```python
await queue.put(item)
```

**从队列中获取元素**：

```python
item = await queue.get()
```

**检查队列是否为空**：

```python
if queue.empty():
    # ...
```

**检查队列是否已满**：

```python
if queue.full():
    # ...
```

**获取队列当前容量**：

```python
capacity = queue.qsize()
```

**将工作单元标记为完成**：

```python
queue.task_done()
```

**等待所有单元完成**：

```python
await queue.join()
```

## 异步生成器和迭代器

**定义异步生成器**：

```python
async def async_generator():
    for i in range(10):
        await asyncio.sleep(1)
        yield i
```

**定义异步迭代器**：

```python
class AsyncIterator():
    def __init__(self):
        self.counter = 0
    def __aiter__(self):
        return self
    async def __anext__(self):
        if self.counter >= 10:
            raise StopAsyncIteration
        await asyncio.sleep(1)
        self.counter += 1
        return self.counter
```

**使用异步迭代器**：

```python
async for value in AsyncIterator():
    # ...
```

## 异步上下文管理器

**定义异步上下文管理器**：

```python
class AsyncContextManager():
    async def __aenter__(self):
        await asyncio.sleep(1)
    async def __aexit__(self, et, exc, tb):
        await asyncio.sleep(1)
```

**使用异步迭代器**：

```python
async with CustomClass() as mgmr:
    # ...
```
