## 异步和Promise

### 1. 为什么需要异步

> 因为JS是单线程的，其次只能有一个任务执行。如果对于一些耗时动作，比如IO读取，网络请求等等也采取同步的策略，将对系统性能造成很大的影响。所以产生了异步编程，将耗时的任务交给外部，当外部处理完之后再通知JS线程处理结果。所以异步中最大的难点就是我当前的程序何时能够去读取外部程序处理之后的值，因为我们并不知道外部程序什么时候能处理完。