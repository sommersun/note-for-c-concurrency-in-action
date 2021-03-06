# managing threads 
## 1.launching a thread
std:: thread() 

```c++
  std::thread my_thread(my_func);
  my_thread.detach();
```

detach() 带来的问题，子线程会读取到父线程已经销毁的资源。  

## 2. wait for a thread to complete:

```c++
  my_thread.join();
```
用join() 等待子线程的结束，join() 会 clean up any storage associated with the thread, 所以它是一次性操作。  
join() 会带来的问题，我们并不总是在开启子线程之后，就立刻执行join()，也就是说程序有可能在join()操作执行之前就终止。
为了避免在exception throw 的情况下，出现上述问题，我们需要在try/catch block 的cath 部分也加入join()。
但这个方法显然过于啰嗦且容易出错。  
所以，一种经典的方法是 RAII Resource Acquisition Is Initialization。 利用了C++语言局部对象自动销毁，来控制资源的生命周期。注意使用=delete来禁止copy and assign thread_guard。  

## 3. running thread in the background:  
daemon thread ~~ after my_thread.detach()  

## 4. pass arguments to a thread function:  
std::thread t(f,a,b);
举了3种情况，这里理解的不太透彻  
a) std::string  buffer 指针未来得及转换  
b) 想传一个引用，但整个对象被复制了，需用std::ref()  
c) arguments cannot be copied but can only be moved: std::unique_ptr auto/std::move()  

## 5. transferring ownership of a thread：  
resource-owning types  movable but not copyable  

```c++
std::moved();
```
a) 普通transfer  
注意给已经被赋值的 std::thread 进行重复赋值，so std::terminate() is called  
b) transferred out of a function  
c) transferred into a fucntion  

scoped_thread 注意它与 thread_guard 的区别： thread_gurad 只是guard某个线程，而不own该线程，会有多个thread_gurad guard 同一个线程。  

https://stackoverflow.com/questions/56497350/thread-guard-vs-scoped-thread  

A joining_thread class  这堆没看懂在说啥  

## 6. choosing the number of threads at runtime  

这段代码也挺难读的  

```c++
std::thread::hardware_concurrency();
```

## 7. Indentifying threads

```c++
std::this_thread::get_id()
```
这个类很强大，可以用来比较，sort, hash ，std 库都有实现


