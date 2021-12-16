# managing threads 
1.launching a thread
std:: thread() 

```c++
  std::thread my_thread(my_func);
  my_thread.detach();
```

detach() 带来的问题，子线程会读取到父线程已经销毁的资源。  

wait for a thread to complete:

```c++
  my_thread.join();
```
用join() 等待子线程的结束，join() 会 clean up any storage associated with the thread, 所以它是一次性操作。  
join() 会带来的问题，我们并不总是在开启子线程之后，就立刻执行join()，也就是说程序有可能在join()操作执行之前就终止。
为了避免在exception throw 的情况下，出现上述问题，我们需要在try/catch block 的cath 部分也加入join()。
但这个方法显然过于啰嗦且容易出错。
所以，一种经典的方法是 RAII Resource Acquisition Is Initialization
