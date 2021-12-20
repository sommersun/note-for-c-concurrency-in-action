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
。。。


