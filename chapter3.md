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

