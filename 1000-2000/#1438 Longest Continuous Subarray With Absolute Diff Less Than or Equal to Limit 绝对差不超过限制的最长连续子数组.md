# 1438 Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit 绝对差不超过限制的最长连续子数组

__Description:__

Given an array of integers `nums` and an integer `limit`, return the size of the longest __non-empty__ subarray such that the absolute difference between any two elements of this subarray is less than or equal to `limit`_._

__Example:__

Example 1:

```text
Input: nums = [8,2,4,7], limit = 4
Output: 2 
Explanation: All subarrays are: 
[8] with maximum absolute diff |8-8| = 0 <= 4.
[8,2] with maximum absolute diff |8-2| = 6 > 4. 
[8,2,4] with maximum absolute diff |8-2| = 6 > 4.
[8,2,4,7] with maximum absolute diff |8-2| = 6 > 4.
[2] with maximum absolute diff |2-2| = 0 <= 4.
[2,4] with maximum absolute diff |2-4| = 2 <= 4.
[2,4,7] with maximum absolute diff |2-7| = 5 > 4.
[4] with maximum absolute diff |4-4| = 0 <= 4.
[4,7] with maximum absolute diff |4-7| = 3 <= 4.
[7] with maximum absolute diff |7-7| = 0 <= 4. 
Therefore, the size of the longest subarray is 2.
```

Example 2:

```text
Input: nums = [10,1,2,4,7,2], limit = 5
Output: 4 
Explanation: The subarray [2,4,7,2] is the longest since the maximum absolute diff is |2-7| = 5 <= 5.
```

Example 3:

```text
Input: nums = [4,2,2,2,4,4,2,2], limit = 0
Output: 3
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `0 <= limit <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `nums` ，和一个表示限制的整数 `limit`，请你返回最长连续子数组的长度，该子数组中的任意两个元素之间的绝对差必须小于或者等于 `limit` _。_

如果不存在满足条件的子数组，则返回 `0` 。

__示例:__

示例 1：

```text
输入：nums = [8,2,4,7], limit = 4
输出：2 
解释：所有子数组如下：
[8] 最大绝对差 |8-8| = 0 <= 4.
[8,2] 最大绝对差 |8-2| = 6 > 4. 
[8,2,4] 最大绝对差 |8-2| = 6 > 4.
[8,2,4,7] 最大绝对差 |8-2| = 6 > 4.
[2] 最大绝对差 |2-2| = 0 <= 4.
[2,4] 最大绝对差 |2-4| = 2 <= 4.
[2,4,7] 最大绝对差 |2-7| = 5 > 4.
[4] 最大绝对差 |4-4| = 0 <= 4.
[4,7] 最大绝对差 |4-7| = 3 <= 4.
[7] 最大绝对差 |7-7| = 0 <= 4. 
因此，满足题意的最长子数组的长度为 2 。
```

示例 2：

```text
输入：nums = [10,1,2,4,7,2], limit = 5
输出：4 
解释：满足题意的最长子数组是 [2,4,7,2]，其最大绝对差 |2-7| = 5 <= 5 。
```

示例 3：

```text
输入：nums = [4,2,2,2,4,4,2,2], limit = 0
输出：3
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `0 <= limit <= 10 ^ 9`

__思路:__

```text
单调队列
用一个单调递减队列记录窗口内的最大值
用另一个单调递增队列记录窗口内的最小值
这样两个队列的队首之差就是窗口内的最大差值
当队首之差大于 limit 时, 就从两个队列中弹出值并向右移动左边界
每次循环移动一次右边界
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestSubarray(vector<int>& nums, int limit) 
    {
        deque<int> max_queue, min_queue;
        int n = nums.size(), left = 0, right = 0, result = 0;
        while (right < n)
        {
            while (!max_queue.empty() and max_queue.back() < nums[right]) max_queue.pop_back();
            while (!min_queue.empty() and min_queue.back() > nums[right]) min_queue.pop_back();
            max_queue.push_back(nums[right]);
            min_queue.push_back(nums[right]);
            while (max_queue.front() - min_queue.front() > limit)
            {
                if (nums[left] == min_queue.front()) min_queue.pop_front();
                if (nums[left++] == max_queue.front()) max_queue.pop_front();
            }
            result = max(result, right++ - left + 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestSubarray(int[] nums, int limit) {
        Deque<Integer> maxQueue = new LinkedList<>(), minQueue = new LinkedList<>();
        int n = nums.length, left = 0, right = 0, result = 0;
        while (right < n) {
            while (!maxQueue.isEmpty() && maxQueue.peekLast() < nums[right]) maxQueue.pollLast();
            while (!minQueue.isEmpty() && minQueue.peekLast() > nums[right]) minQueue.pollLast();
            maxQueue.offerLast(nums[right]);
            minQueue.offerLast(nums[right]);
            while (maxQueue.peekFirst() - minQueue.peekFirst() > limit) {
                if (nums[left] == minQueue.peekFirst()) minQueue.pollFirst();
                if (nums[left++] == maxQueue.peekFirst()) maxQueue.pollFirst();
            }
            result = Math.max(result, right++ - left + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestSubarray(self, nums: List[int], limit: int) -> int:
        max_queue, min_queue, n, left, right, result = deque(), deque(), len(nums), 0, 0, 0
        while right < n:
            while max_queue and max_queue[-1] < nums[right]:
                max_queue.pop()
            while min_queue and min_queue[-1] > nums[right]:
                min_queue.pop()
            max_queue.append(nums[right])
            min_queue.append(nums[right])
            while max_queue[0] - min_queue[0] > limit:
                if nums[left] == min_queue[0]:
                    min_queue.popleft()
                if nums[left] == max_queue[0]:
                    max_queue.popleft()
                left += 1
            result = max(result, right - left + 1)
            right += 1
        return result
```
