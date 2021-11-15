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