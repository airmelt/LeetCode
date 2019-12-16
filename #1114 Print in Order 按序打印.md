__Description__:
Suppose we have a class:

public class Foo {
  public void first() { print("first"); }
  public void second() { print("second"); }
  public void third() { print("third"); }
}
The same instance of Foo will be passed to three different threads. Thread A will call first(), thread B will call second(), and thread C will call third(). Design a mechanism and modify the program to ensure that second() is executed after first(), and third() is executed after second().

__Example:__
Example 1:

Input: [1,2,3]
Output: "firstsecondthird"
Explanation: There are three threads being fired asynchronously. The input [1,2,3] means thread A calls first(), thread B calls second(), and thread C calls third(). "firstsecondthird" is the correct output.

Example 2:

Input: [1,3,2]
Output: "firstsecondthird"
Explanation: The input [1,3,2] means thread A calls first(), thread B calls third(), and thread C calls second(). "firstsecondthird" is the correct output.
 
__Note:__

We do not know how the threads will be scheduled in the operating system, even though the numbers in the input seems to imply the ordering. The input format you see is mainly to ensure our tests' comprehensiveness.

__题目描述__:
我们提供了一个类：

public class Foo {
  public void one() { print("one"); }
  public void two() { print("two"); }
  public void three() { print("three"); }
}
三个不同的线程将会共用一个 Foo 实例。

线程 A 将会调用 one() 方法
线程 B 将会调用 two() 方法
线程 C 将会调用 three() 方法
请设计修改程序，以确保 two() 方法在 one() 方法之后被执行，three() 方法在 two() 方法之后被执行。

__示例 :__
示例 1:

输入: [1,2,3]
输出: "onetwothree"
解释: 
有三个线程会被异步启动。
输入 [1,2,3] 表示线程 A 将会调用 one() 方法，线程 B 将会调用 two() 方法，线程 C 将会调用 three() 方法。
正确的输出是 "onetwothree"。

示例 2:

输入: [1,3,2]
输出: "onetwothree"
解释: 
输入 [1,3,2] 表示线程 A 将会调用 one() 方法，线程 B 将会调用 three() 方法，线程 C 将会调用 two() 方法。
正确的输出是 "onetwothree"。
 

__注意:__

尽管输入中的数字似乎暗示了顺序，但是我们并不保证线程在操作系统中的调度顺序。

你看到的输入格式主要是为了确保测试的全面性。

__思路__:
用信号量, 给 3个打印函数加锁
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:
```C++
#include<semaphore.h>
class Foo 
{
public:
    Foo() 
    {
        sem_init(&s1, 0, 0);
        sem_init(&s1, 0, 0);
    }

    void first(function<void()> printFirst) 
    {
        
        // printFirst() outputs "first". Do not change or remove this line.
        printFirst();
        sem_post(&s1);
    }

    void second(function<void()> printSecond) 
    {
        sem_wait(&s1);
        // printSecond() outputs "second". Do not change or remove this line.
        printSecond();
        sem_post(&s2);
    }

    void third(function<void()> printThird) 
    {
        sem_wait(&s2);
        // printThird() outputs "third". Do not change or remove this line.
        printThird();
    }
private:
    sem_t s1, s2;
};
```

__Java__:
```Java
class Foo {

    private Semaphore s1 = new Semaphore(0);
    private Semaphore s2 = new Semaphore(0);
    
    public Foo() {
        
    }

    public void first(Runnable printFirst) throws InterruptedException {
        
        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
        s1.release();
    }

    public void second(Runnable printSecond) throws InterruptedException {
        s1.acquire();
        // printSecond.run() outputs "second". Do not change or remove this line.
        printSecond.run();
        s2.release();
    }

    public void third(Runnable printThird) throws InterruptedException {
        s2.acquire();
        // printThird.run() outputs "third". Do not change or remove this line.
        printThird.run();
    }
}
```

__Python__:
```Python
class Foo:
    def __init__(self):
        self.s1 = threading.Semaphore(0)
        self.s2 = threading.Semaphore(0)


    def first(self, printFirst: 'Callable[[], None]') -> None:
        
        # printFirst() outputs "first". Do not change or remove this line.
        printFirst()
        self.s1.release()


    def second(self, printSecond: 'Callable[[], None]') -> None:
        self.s1.acquire()
        # printSecond() outputs "second". Do not change or remove this line.
        printSecond()
        self.s2.release()


    def third(self, printThird: 'Callable[[], None]') -> None:
        self.s2.acquire()
        # printThird() outputs "third". Do not change or remove this line.
        printThird()
```