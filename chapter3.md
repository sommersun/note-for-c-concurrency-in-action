# Sharing data between threads
both benefit and drawback  
## 1. problems with sharing data between threads  
safe: all shared data is read-only  
### race conditions  
### avoid problematic race conditions  
wrap data structure with a protection mechanism 
lock-free programming  
transcation  software transcational memory (STM)
## 2. protect shared data with mutexes  
直接操作 mutex，即直接调用 mutex 的 lock / unlock 函数, 代码如有异常，unlock 就调不到了  
使用 lock_guard 自动加锁、解锁。原理是 RAII，在构造函数里加锁，在析构函数里解锁。

```c++
std::lock_guard<std::mutex> guard(some_mutex);
```
### structuring code for protecting shared data
通常我们会设计一个类来包裹数据，功能函数和锁，但这个类需要精心设计。
Don’t pass pointers and references to protected data outside the scope of the lock, whether by
returning them from a function, storing them in externally visible memory, or passing them as
arguments to user-supplied functions.
### spotting race conditions inherent in interfaces
这里使用std::stack这个例子，在单线程的情况它是安全的。  
但在多线程的情况下，会出现多种race condition:  
1. empty()/top()  
2. top()/pop()  

stack()的pop()为了保证其本身的安全，将其分割成top()和pop()两个操作，但这也带来了race condition。后面它提供了一些方法解决这个问题，有点深奥。  
提供了一个thread-safe stack, (⊙﹏⊙) 体会不了，以后再看。

### Deadlock
two or more mutex
The common advice for avoiding deadlock is to always lock the two mutexes in the same order。  
在某些情况下并不容易实现，它举了一个交换数据的例子。  
c++ 提供了 避免死锁的解决方案。  
### Further guidelines for avoiding deadlock  
deadlock baused by : lock , join() for each other(don’t
wait for another thread if there’s a chance it’s waiting for you.)  
avoid nested locks  
avoid calling user_supplied code while holding  a clock  
acquire locks in a fixed order  
use a lock hierarchy :thread_local 线程周期
extending these guidelines beyond locks
###  flexible locking std::unique_lock  
它提供了lock()和unlock()接口，能记录现在处于上锁还是没上锁状态，在析构的时候，会根据当前状态来决定是否要进行解锁（lock_guard就一定会解锁）。同样，可以使用std::defer_lock设置初始化的时候不进行默认的上锁操作。  
使用起来就比lock_guard更加灵活！然后这也是有代价的，因为它内部需要维护锁的状态，所以效率要比lock_guard低一点。  
### Further guidelines for avoiding deadlock  
deadlock baused by : lock , join() for each other(don’t
wait for another thread if there’s a chance it’s waiting for you.)  
avoid nested locks  
avoid calling user_supplied code while holding  a clock  
acquire locks in a fixed order  
use a lock hierarchy :thread_local 线程周期
extending these guidelines beyond locks
###  flexible locking std::unique_lock  
它提供了lock()和unlock()接口，能记录现在处于上锁还是没上锁状态，在析构的时候，会根据当前状态来决定是否要进行解锁（lock_guard就一定会解锁）。同样，可以使用std::defer_lock设置初始化的时候不进行默认的上锁操作。  
使用起来就比lock_guard更加灵活，然后这也是有代价的，因为它内部需要维护锁的状态，所以效率要比lock_guard低一点。  
### locking at an appropriate granularity    
As this example shows, locking at an appropriate granularity isn’t only about the
amount of data locked; it’s also about how long the lock is held and what operations
are performed while the lock is held.  






