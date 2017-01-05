#iOS WAKEUPS Crash error

> Exception Type: EXC_RESOURCE
> Exception Subtype: WAKEUPS
> Exception Message: (Limit 150/sec)
> so, don't create many threads and sleep.
> all resource need a same way to asyn_load, please keep less thread

Your app is sending a wakeup command to a particular thread in the app quite often - apparently an average of 206 times a second. Background threads in iOS 8 have a hard limit on how many times you can run a sleep/wake cycle on each thread per second, and having a high count here is usually an indication that something is wrong / inefficient in your thread management.
Without seeing your code, my recommendation is that you check your C++ algorithms for sleep/wake calls, or multithread the background process to start new threads each cycle.
Ray Wenderlich has a fantastic tutorial on Apple's system for multithreading, Grand Central Dispactch, which might also be a good resource for you:[http://www.raywenderlich.com/60749/grand-central-dispatch-in-depth-part-1](http://www.raywenderlich.com/60749/grand-central-dispatch-in-depth-part-1)