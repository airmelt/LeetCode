# 862 Shortest Subarray with Sum at Least K 和至少为 K 的最短子数组

__Description__:
Given an integer array nums and an integer k, return the length of the shortest non-empty subarray of nums with a sum of at least k. If there is no such subarray, return -1.

A subarray is a contiguous part of an array.

__Example:__

Example 1:

Input: nums = [1], k = 1
Output: 1

Example 2:

Input: nums = [1,2], k = 4
Output: -1

Example 3:

Input: nums = [2,-1,2], k = 3
Output: 3

__Constraints:__

1 <= nums.length <= 10^5
-10^5 <= nums[i] <= 10^5
1 <= k <= 10^9

__题目描述__:
返回 A 的最短的非空连续子数组的长度，该子数组的和至少为 K 。

如果没有和至少为 K 的非空子数组，返回 -1 。

__示例 :__

示例 1：

输入：A = [1], K = 1
输出：1

示例 2：

输入：A = [1,2], K = 4
输出：-1

示例 3：

输入：A = [2,-1,2], K = 3
输出：3

__提示:__

1 <= A.length <= 50000
-10 ^ 5 <= A[i] <= 10 ^ 5
1 <= K <= 10 ^ 9

__思路__:

前缀和 + 单调队列
由于题目要求求连续的子数组, 考虑使用滑动窗口, 但是因为数组中有负数, 不满足单调性, 所以不能使用滑动窗口
可以用类似滑动窗口的思想, 使用前缀和
因为需要求出子数组的和, 可以用前缀和预先处理, 方便查找子数组的和
假定已经找到 i1 < i2 < j, pre[i1] >= pre[i2], 如果有 pre[j] - pre[i1] >= k, 那么一定有 pre[j] - pre[i2] >= k, 由于 i1 < i2, 所以 j - i1 > j - i2, 这时可以保证 i1 必然不是需要的值
所以需要一个单调队列来存放, 遍历到比队列尾对应的前缀和小的时候, 弹出队尾
比如 [1, 3, -2, 4] -> [0, 1, 4, 2, 6], 6 - 2 > 6 - 4, 所以 4 应该被弹出
按照滑动窗口的思想, 如果 pre[j] - pre[queue.front()] >= k, 这时可以弹出队头并且更新结果值
最后需要判断是否存在结果满足 k
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int shortestSubarray(vector<int>& nums, int k) 
    {
        int n = nums.size(), result = n + 1;
        vector<int> pre(n + 1, 0);
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i];
        list<int> queue;
        for (int i = 0; i <= n; i++) 
        {
            while (!queue.empty() and pre[i] <= pre[queue.back()]) queue.pop_back();
            while (!queue.empty() and pre[i] - pre[queue.front()] >= k) 
            {
                result = min(result, i - queue.front());
                queue.pop_front();
            }
            queue.push_back(i);
        }
        return result == n + 1 ? -1 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int shortestSubarray(int[] nums, int k) {
        int n = nums.length, result = n + 1, pre[] = new int[n + 1];
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i];
        LinkedList<Integer> queue = new LinkedList<>();
        for (int i = 0; i <= n; i++) {
            while (!queue.isEmpty() && pre[i] <= pre[queue.peekLast()]) queue.pollLast();
            while (!queue.isEmpty() && pre[i] - pre[queue.peek()] >= k) result = Math.min(result, i - queue.poll());
            queue.add(i);
        }
        return result == n + 1 ? -1 : result;
    }
}
```

__Python__:

```Python
class Solution:
    def shortestSubarray(self, nums: List[int], k: int) -> int:
        pre, result, queue = [0] + list(accumulate(nums)), (n := len(nums)) + 1, deque()
        for i, v in enumerate(pre):
            while queue and v <= pre[queue[-1]]:
                queue.pop()
            while queue and v - pre[queue[0]] >= k:
                result = min(result, i - queue.popleft())
            queue.append(i)
        return result if result < n + 1 else -1
```
