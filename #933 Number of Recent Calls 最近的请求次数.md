__Description__:
Write a class RecentCounter to count recent requests.

It has only one method: ping(int t), where t represents some time in milliseconds.

Return the number of pings that have been made from 3000 milliseconds ago until now.

Any ping with time in [t - 3000, t] will count, including the current ping.

It is guaranteed that every call to ping uses a strictly larger value of t than before.

__Example:__
Example 1:

Input: inputs = ["RecentCounter","ping","ping","ping","ping"], inputs = [[],[1],[100],[3001],[3002]]
Output: [null,1,2,3,3]
 
__Note:__

Each test case will have at most 10000 calls to ping.
Each test case will call ping with strictly increasing values of t.
Each call to ping will have 1 <= t <= 10^9.

__题目描述__:
写一个 RecentCounter 类来计算最近的请求。

它只有一个方法：ping(int t)，其中 t 代表以毫秒为单位的某个时间。

返回从 3000 毫秒前到现在的 ping 数。

任何处于 [t - 3000, t] 时间范围之内的 ping 都将会被计算在内，包括当前（指 t 时刻）的 ping。

保证每次对 ping 的调用都使用比之前更大的 t 值。

__示例 :__

输入：inputs = ["RecentCounter","ping","ping","ping","ping"], inputs = [[],[1],[100],[3001],[3002]]
输出：[null,1,2,3,3]
 
__提示：__

每个测试用例最多调用 10000 次 ping。
每个测试用例会使用严格递增的 t 值来调用 ping。
每次调用 ping 都有 1 <= t <= 10^9。


__思路__:
维护一个长度为 3000的滑动窗口, 可用队列实现
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class RecentCounter {
public:
    RecentCounter() {}
    
    int ping(int t) 
    {
        q.push(t);
        while (t - 3000 > q.front()) q.pop();
        return q.size();
    }
private:
    queue<int> q;
};


/**
 * Your RecentCounter object will be instantiated and called as such:
 * RecentCounter* obj = new RecentCounter();
 * int param_1 = obj->ping(t);
 */
```

__Java__:
```Java
class RecentCounter {

    private Queue<Integer> queue;
    
    public RecentCounter() {
        queue = new LinkedList<>();
    }
    
    public int ping(int t) {
        queue.offer(t);
        while (queue.peek() < t - 3000) queue.poll();
        return queue.size();
    }
}

/**
 * Your RecentCounter object will be instantiated and called as such:
 * RecentCounter obj = new RecentCounter();
 * int param_1 = obj.ping(t);
 */
```

__Python__:
```Python
class RecentCounter:

    def __init__(self):
        self.queue = collections.deque()

    def ping(self, t: int) -> int:
        self.queue.append(t)
        while self.queue[0] < t - 3000:
            self.queue.popleft()
        return len(self.queue)


# Your RecentCounter object will be instantiated and called as such:
# obj = RecentCounter()
# param_1 = obj.ping(t)
```