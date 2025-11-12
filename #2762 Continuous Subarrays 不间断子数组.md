# 2762 Continuous Subarrays 不间断子数组

__Description:__

You are given a __0-indexed__ integer array `nums`. A subarray of `nums` is called __continuous__ if:

- Let `i`, `i + 1`, ..., `j` be the indices in the subarray. Then, for each pair of indices `i <= i1, i2 <= j`, `0 <= |nums[i1] - nums[i2]| <= 2`.

Return the total number of continuous subarrays.

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [5,4,2,4]
Output: 8
Explanation: 
Continuous subarray of size 1: [5], [4], [2], [4].
Continuous subarray of size 2: [5,4], [4,2], [2,4].
Continuous subarray of size 3: [4,2,4].
There are no subarrys of size 4.
Total continuous subarrays = 4 + 3 + 1 = 8.
It can be shown that there are no more continuous subarrays.
```

Example 2:

```text
Input: nums = [1,2,3]
Output: 6
Explanation: 
Continuous subarray of size 1: [1], [2], [3].
Continuous subarray of size 2: [1,2], [2,3].
Continuous subarray of size 3: [1,2,3].
Total continuous subarrays = 3 + 2 + 1 = 6.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。 `nums` 的一个子数组如果满足以下条件，那么它是 __不间断__ 的:

- `i`， `i + 1` ，...， `j` 表示子数组中的下标。对于所有满足 `i <= i1, i2 <= j` 的下标对，都有 `0 <= |nums[i1] - nums[i2]| <= 2` 。

请你返回 不间断 子数组的总数目。

子数组是一个数组中一段连续 非空 的元素序列。

__示例:__

示例 1：

```text
输入：nums = [5,4,2,4]
输出：8
解释：
大小为 1 的不间断子数组：[5], [4], [2], [4] 。
大小为 2 的不间断子数组：[5,4], [4,2], [2,4] 。
大小为 3 的不间断子数组：[4,2,4] 。
没有大小为 4 的不间断子数组。
不间断子数组的总数目为 4 + 3 + 1 = 8 。
除了这些以外，没有别的不间断子数组。
```

示例 2：

```text
输入：nums = [1,2,3]
输出：6
解释：
大小为 1 的不间断子数组：[1], [2], [3] 。
大小为 2 的不间断子数组：[1,2], [2,3] 。
大小为 3 的不间断子数组：[1,2,3] 。
不间断子数组的总数目为 3 + 2 + 1 = 6 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
滑动窗口, 保证滑动窗口里的最值之差不超过 2
每次加上滑动窗口的长度即可
1. 哈希表(Python)
将数字都放入哈希表
如果哈希表里的最大值减去最小值比 2 大, 依次弹出左边窗口的值
也可以用有序列表实现
哈希表中最多存储 3 个数字
时间复杂度为 O(N), 空间复杂度为 O(1), 实际上还与绝对值差 2 有关, 需要乘上这个常数倍数
2. 单调队列
用两个单调队列分别维护滑动窗口里的最大值和最小值
最大值单调队列里维护一个递减的队列, 这样保证堆首为最大值
最小值单调队列类似
如果最大值减去最小值比 2 大
那么移动滑动窗口, 将不符合值从队列中移除
时间复杂度为 O(N), 空间复杂度为 O(N), 与绝对值差 2 无关
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long continuousSubarrays(vector<int>& nums) 
    {
        deque<int> min_values, max_values;
        long long result = 0LL;
        for (int left = 0, right = 0, n = nums.size(); right < n; right++) 
        {
            while (!min_values.empty() and nums[right] <= nums[min_values.back()]) min_values.pop_back();
            min_values.push_back(right);
            while (!max_values.empty() and nums[right] >= nums[max_values.back()]) max_values.pop_back();
            max_values.push_back(right);
            while (nums[max_values.front()] - nums[min_values.front()] > 2) 
            {
                if (min_values.front() < ++left) min_values.pop_front();
                if (max_values.front() < left) max_values.pop_front();
            }
            result += right - left + 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long continuousSubarrays(int[] nums) {
        Deque<Integer> maxValues = new ArrayDeque<>(), minValues = new ArrayDeque<>();
        long result = 0L;
        for (int left = 0, right = 0, n = nums.length; right < n; right++) {
            while (!minValues.isEmpty() && nums[right] <= nums[minValues.peekLast()]) minValues.pollLast();
            minValues.addLast(right);
            while (!maxValues.isEmpty() && nums[right] >= nums[maxValues.peekLast()]) maxValues.pollLast();
            maxValues.addLast(right);
            while (nums[maxValues.peekFirst()] - nums[minValues.peekFirst()] > 2) {
                if (minValues.peekFirst() < ++left) minValues.pollFirst();
                if (maxValues.peekFirst() < left) maxValues.pollFirst();
            }
            result += right - left + 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def continuousSubarrays(self, nums: List[int]) -> int:
        result, left, window = 0, 0, Counter()
        for right, num in enumerate(nums):
            window[num] += 1
            while max(window) - min(window) > 2:
                window[y := nums[left]] -= 1
                if not window[y]:
                    del window[y]
                left += 1
            result += right - left + 1
        return result
```
