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

