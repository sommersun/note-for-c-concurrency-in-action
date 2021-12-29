# the C++ memory model and operations on atomic types

## 1. memory model basics

### objects and memory locations

### objects, memory locations, and concurrency
if either thread is modifying the data , there's a potential for a race condition.
so to avoid this:  
mutex  
atomic : still race condintion , but no undefined behavior  

### modification orders

