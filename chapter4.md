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
### making (std::) promises
have a small number of threads(possibly only one handling the connections), width each thread dealing with multiple connections at once.  

### saving an exception for  the future
when the task is invoked if the wrapped function throws an exception, that exception is stored in the futurein place of the result, ready to be thrown on a call to get().  

~~ tired Friday why people can stand on 996

### waiting from multiply threads
wait for the same event from more than one thread  
std::shared_future  copyable

## waiting with a time  limit
a duration based time out 
a  absolute time out  
two overloads of wait() ： wait_for() wait_until()  

### clocks
### durations
### time points







