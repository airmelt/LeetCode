__Description__:
Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

For example,
[2,3,4], the median is 3

[2,3], the median is (2 + 3) / 2 = 2.5

Design a data structure that supports the following two operations:

void addNum(int num) - Add a integer number from the data stream to the data structure.
double findMedian() - Return the median of all elements so far.
 
__Example:__

addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2
 
__Follow up:__

If all integer numbers from the stream are between 0 and 100, how would you optimize it?
If 99% of all integer numbers from the stream are between 0 and 100, how would you optimize it?

__题目描述__:
中位数是有序列表中间的数。如果列表长度是偶数，中位数则是中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

void addNum(int num) - 从数据流中添加一个整数到数据结构中。
double findMedian() - 返回目前所有元素的中位数。

__示例 :__

addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2

__进阶：__

如果数据流中所有整数都在 0 到 100 范围内，你将如何优化你的算法？
如果数据流中 99% 的整数都在 0 到 100 范围内，你将如何优化你的算法？

__思路__:
设置一个大根堆, 一个小根堆
大根堆里放较小的元素, 记为 small, 小根堆里放较大的元素, 记为large
保证两个堆的元素个数差不超过 1
并且 large中的元素要不少于 small中的元素
由于刚开始两个堆元素相等, 每次如果都优先向 large中存放元素, small的 size()永远不大于 large
放入元素时, 先放入另一个堆, 使得堆一直保持有序, 再弹出放入目标堆
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
class MedianFinder 
{
public:
    /** initialize your data structure here. */
    MedianFinder() 
    {
        
    }
    
    void addNum(int num) 
    {
        if (small.size() >= large.size())
        {
            small.push(num);
            large.push(small.top());
            small.pop();
        }
        else
        {
            large.push(num);
            small.push(large.top());
            large.pop();
        }
    }
    
    double findMedian() 
    {
        return large.size() < small.size() ? small.top() : (large.size() > small.size() ? large.top() : (large.top() + small.top()) / 2.0);
    }
private:
    priority_queue<int, vector<int>, greater<int>> large;
    priority_queue<int, vector<int>, less<int>> small;
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

__Java__:
```Java
class MedianFinder {

    private PriorityQueue<Integer> large;
    private PriorityQueue<Integer> small;
    
    /** initialize your data structure here. */
    public MedianFinder() {
        large = new PriorityQueue<>();
        small = new PriorityQueue<>((a, b) -> {
            return b - a;
        });
    }
    
    public void addNum(int num) {
        if (small.size() >= large.size()) {
            small.offer(num);
            large.offer(small.poll());
        } else {
            large.offer(num);
            small.offer(large.poll());
        }
    }
    
    public double findMedian() {
        return large.size() < small.size() ? small.peek() : (large.size() > small.size() ? large.peek() : (large.peek() + small.peek()) / 2.0);
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

__Python__:
```Python
class MedianFinder:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.large = []
        self.small = []

    def addNum(self, num: int) -> None:
        if len(self.small) >= len(self.large):
            heapq.heappush(self.small, -num)
            heapq.heappush(self.large, -heapq.heappop(self.small))
        else:
            heapq.heappush(self.large, num)
            heapq.heappush(self.small, -heapq.heappop(self.large))
                           
    def findMedian(self) -> float:
        return self.large[0] if len(self.large) > len(self.small) else -self.small[0] if len(self.large) > len(self.small) else (self.large[0] - self.small[0]) / 2.0


# Your MedianFinder object will be instantiated and called as such:
# obj = MedianFinder()
# obj.addNum(num)
# param_2 = obj.findMedian()
```