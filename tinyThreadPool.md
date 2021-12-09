# tinyThreads
## step-0 threads

如果你完成 c-05 的某项挑战任务，或者 java-0x (我忘了番号了)，那么你应该已经对线程有了初步的了解，可以跳过 step-0

--- 
## step-3 threadPool


## hints
你可以填充这段代码使之成为可使用的简单线程池

但你也可以试试完全从零开始写

虽然这一段给出的代码是用 `cpp` 写的，但是你可以自选喜欢的语言完成

tips:
 - 这门语言应该有多线程机制
 - 最好有 lambda 表达式的特性
 - 有对象的概念

``` cpp
#ifndef THREADPOOL_HH
#define THREADPOOL_HH

#include <vector>
#include <queue>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <functional>
#include <future>

class ThreadPool {
  private:
    std::vector<std::thread> _workers;
    std::queue<std::function<void()>> _queue;
    std::mutex _mtx;
    std::condition_variable _cv;
    bool _is_stop;

  public:
    ThreadPool(size_t size): _is_stop(false) {
        // todo: your code here
    }

    ~ThreadPool() {
        // todo: your code here
    }

    template<class F, class ... Args>
    auto enqueue(F &&f, Args && ... args)
     -> std::future<typename std::result_of<F(Args...)>::type> {
        // todo: your code here
     }
};

#endif
```

测试

最后，你应该使用一段简单的代码来测试你的线程池是否能顺利运行

比如说下面这段

```cpp
#include <iostream>
#include <future>
#include "thread.hh"

void use_pool() {
    ThreadPool pool(8);
    const size_t kJobsCounts = 1000;
    auto task = [](int x) { std::cout<<x<<std::endl; };
    std::future<int> result[1001];
    for(int i=0; i<kJobsCounts; i++) 
        result[i] = pool.enqueue(task, i);
    int res = 0;
    for(int i=0; i<kJobsCounts; i++) 
        res += result[i].get();
    std::cout << "total: " << res <<std::endl;
}

int main() {
    use_pool();
    std::cout << "it's done" << std::endl;
    return 0;
}
```
Java中关于线程池的实现可以参照ThreadPoolExecutor，这边准备了某个爪哇弱鸡写的模板（相比源码简化了挺多东西），如果觉得太烂也可以不参考自己直接造一个玩，如果对Java并发感兴趣的群友可以入手一本《Java并发的艺术》（作者是一位阿里员工），感觉讲的挺好的。
```java
public class MyThreadPoolExcutor{
    //简单来说这个是一个记录线程池状态的整型值，高三位表示线程池状态，低二十九位表示线程池目前容量，至于Atomic可以去了解一下原子性，CAS等概念
    private AtomicInteger ctl=new AtomicInteger(ctlOf(RUNNING,0));
    private static final int COUNT_BITS = Integer.SIZE-3;
    private static final int CAPACITY = (1<<COUNT_BITS)-1;

    private static final int RUNNING = -1<<COUNT_BITS;
    private static final int SHUTDOWN = 0<<COUNT_BITS;

    private static int runStateOf(int c){
        return c & ~CAPACITY;
    }

    private static int workerCountOf(int c){
        return c & CAPACITY;
    }

    private static int ctlOf(int wc,int rs){
        return wc | rs;
    }

    private static boolean isRunning(int c){
        return c < SHUTDOWN;
    }

    private boolean compareAndSetIncrementWorkerSize(int c){
        return ctl.compareAndSet(c,c+1);
    }

    private boolean compareAndSetDecrementWorkerSize(int c){
        return ctl.compareAndSet(c,c-1);
    }

    
    private volatile int maxPoolSize;

    private ReentrantLock mainLock;

    private HashSet<Worker> workers = new HashSet();

    private BlockingQueue<Runnable> workerQueue;

    private ThreadFactory threadFactory;

    private final class Worker implements Runnable{

        Runnable firstTask;

        Thread thread;

        public Worker(Runnable firstTask){
            this.firstTask = firstTask;
            this.thread = threadFactory.newThread(this);
        }

        @Override
        public void run() {
            runWorker(this);
        }
    }
  
    public MyThreadPoolExcutor(int maxPoolSize, ReentrantLock mainLock, HashSet<Worker> workers, BlockingQueue<Runnable> workerQueue, ThreadFactory threadFactory){
        //TODO: your code here
    }

    public void execute(Runnable command){
        //TODO: your code here
    }

    final void runWorker(Worker w){
        //TODO: your code here
    }
}
```
