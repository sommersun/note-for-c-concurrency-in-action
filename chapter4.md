# Synchronizing concurrent operations
condition variables / futures  
latches / barriers  
## waiting for an event or other condition
### waiting for a condition with condition variables
这里详细解释了下面这段代码的用法，非常清晰:

```c++
std::unique_lock <std::mutex> lk(mut);
data_cond.wait(lk,[]{return !data_queue.empty();})
```
### building a thread-safe queue with condition variables
这里解释了为什么要写成 mutable std:: mutex mut; 我没看懂  
notify_one() : There is no guarantee of which thread will be notified or even if there is a thread waiting to be notified.  

## waiting for one-off events with futures
std::future<>  
std::shared_future<>

### returning values from backgrounds tasks

### Associating a task with a future
std::packaged_task<>

```c++
std::packaged_task<void()> task(f);
std::future<void> res= task.get_future();
```
