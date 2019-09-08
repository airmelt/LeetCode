__Description__:
Design a class to find the kth largest element in a stream. Note that it is the kth largest element in the sorted order, not the kth distinct element.

Your KthLargest class will have a constructor which accepts an integer k and an integer array nums, which contains initial elements from the stream. For each call to the method KthLargest.add, return the element representing the kth largest element in the stream.

__Example:__

int k = 3;
int[] arr = [4,5,8,2];
KthLargest kthLargest = new KthLargest(3, arr);
kthLargest.add(3);   // returns 4
kthLargest.add(5);   // returns 5
kthLargest.add(10);  // returns 5
kthLargest.add(9);   // returns 8
kthLargest.add(4);   // returns 8

__Note:__
You may assume that nums' length ≥ k-1 and k ≥ 1.

__题目描述__:
设计一个找到数据流中第K大元素的类（class）。注意是排序后的第K大元素，不是第K个不同的元素。

你的 KthLargest 类需要一个同时接收整数 k 和整数数组nums 的构造器，它包含数据流中的初始元素。每次调用 KthLargest.add，返回当前数据流中第K大的元素。

__示例 :__

int k = 3;
int[] arr = [4,5,8,2];
KthLargest kthLargest = new KthLargest(3, arr);
kthLargest.add(3);   // returns 4
kthLargest.add(5);   // returns 5
kthLargest.add(10);  // returns 5
kthLargest.add(9);   // returns 8
kthLargest.add(4);   // returns 8

__说明:__
你可以假设 nums 的长度≥ k-1 且k ≥ 1。

__思路__:
由于需要输出第 k大的元素, 最好的方案是维护一个小顶堆, 可以用优先队列实现
时间复杂度O(n), 空间复杂度O(k), n为数组 arr的大小

__代码__:
__C++__:
```
class KthLargest {
public:
    KthLargest(int k, vector<int>& nums) {
        pq_size = k;
        for (int i = 0; i < nums.size(); i++) {
            pq.push(nums[i]);
            if (pq.size() > pq_size) pq.pop();
        }
    }
    
    int add(int val) {
        pq.push(val);
        if (pq.size() > pq_size) pq.pop();
        return pq.top();
    }
private:
    priority_queue<int, vector<int>, greater<int>> pq;
    int pq_size;
};

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest* obj = new KthLargest(k, nums);
 * int param_1 = obj->add(val);
 */
```

__Java__:
```
class KthLargest {

    private PriorityQueue<Integer> pq;
    private int pqSize;
    
    public KthLargest(int k, int[] nums) {
        pqSize = k;
        pq = new PriorityQueue<Integer>(pqSize);
        for (int i : nums) add(i);
    }
    
    public int add(int val) {
        if (pq.size() < pqSize) pq.offer(val);
        else if (pq.peek() < val) {
            pq.poll();
            pq.offer(val);
        }
        return pq.peek();
    }
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest obj = new KthLargest(k, nums);
 * int param_1 = obj.add(val);
 */
```

__Python__:
```
import heapq
class KthLargest:

    def __init__(self, k: int, nums: List[int]):
        self.k, self.pq = k, nums
        heapq.heapify(self.pq)
        while len(self.pq) > k:
            heapq.heappop(self.pq)

    def add(self, val: int) -> int:
        if len(self.pq) < self.k:
            heapq.heappush(self.pq, val)
        elif self.pq[0] < val:
            heapq.heapreplace(self.pq, val)
        return self.pq[0]


# Your KthLargest object will be instantiated and called as such:
# obj = KthLargest(k, nums)
# param_1 = obj.add(val)
```