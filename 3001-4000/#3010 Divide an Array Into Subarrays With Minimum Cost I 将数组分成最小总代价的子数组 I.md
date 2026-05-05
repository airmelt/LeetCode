# 3010 Divide an Array Into Subarrays With Minimum Cost I 将数组分成最小总代价的子数组 I

__Description:__

You are given an array of integers `nums` of length `n`.

The __cost__ of an array is the value of its __first__ element. For example, the cost of `[1,2,3]` is `1` while the cost of `[3,4,1]` is `3`.

You need to divide `nums` into `3` __disjoint contiguous__ subarrays.

Return the minimum possible sum of the cost of these subarrays.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,12]
Output: 6
Explanation: The best possible way to form 3 subarrays is: [1], [2], and [3,12] at a total cost of 1 + 2 + 3 = 6.
The other possible ways to form 3 subarrays are:
```

- [1], [2,3], and [12] at a total cost of 1 + 2 + 12 = 15.
- [1,2], [3], and [12] at a total cost of 1 + 3 + 12 = 16.

Example 2:

```text
Input: nums = [5,4,3]
Output: 12
Explanation: The best possible way to form 3 subarrays is: [5], [4], and [3] at a total cost of 5 + 4 + 3 = 12.
It can be shown that 12 is the minimum cost achievable.
```

Example 3:

```text
Input: nums = [10,3,1,1]
Output: 12
Explanation: The best possible way to form 3 subarrays is: [10,3], [1], and [1] at a total cost of 10 + 1 + 1 = 12.
It can be shown that 12 is the minimum cost achievable.
```

__Constraints:__

- `3 <= n <= 50`
- `1 <= nums[i] <= 50`

__题目描述:__

给你一个长度为 `n` 的整数数组 `nums` 。

一个数组的 __代价__ 是它的 __第一个__ 元素。比方说， `[1,2,3]` 的代价是 `1` ， `[3,4,1]` 的代价是 `3` 。

你需要将 `nums` 分成 `3` 个 __连续且没有交集__ 的子数组。

请你返回这些子数组的 最小 代价 总和 。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,12]
输出：6
解释：最佳分割成 3 个子数组的方案是：[1] ，[2] 和 [3,12] ，总代价为 1 + 2 + 3 = 6 。
其他得到 3 个子数组的方案是：
```

- [1] ，[2,3] 和 [12] ，总代价是 1 + 2 + 12 = 15 。
- [1,2] ，[3] 和 [12] ，总代价是 1 + 3 + 12 = 16 。

示例 2：

```text
输入：nums = [5,4,3]
输出：12
解释：最佳分割成 3 个子数组的方案是：[5] ，[4] 和 [3] ，总代价为 5 + 4 + 3 = 12 。
12 是所有分割方案里的最小总代价。
```

示例 3：

```text
输入：nums = [10,3,1,1]
输出：12
解释：最佳分割成 3 个子数组的方案是：[10,3] ，[1] 和 [1] ，总代价为 10 + 1 + 1 = 12 。
12 是所有分割方案里的最小总代价。
```

__提示：__

- `3 <= n <= 50`
- `1 <= nums[i] <= 50`

__思路:__

```text
数组的第一个数必选
1. 排序
剩下两个数可以任选之后的最小值
可以先将后面的 n - 1 个数排序
选择最小的两个数
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
2. 模拟
维护最小的两个值
可以使用堆或者直接模拟
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumCost(vector<int>& nums) 
    {
        sort(nums.begin() + 1, nums.end());
        return accumulate(nums.begin(), nums.begin() + 3, 0);
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumCost(int[] nums) {
        int first = 51, second = 51, n = nums.length;
        for (int i = 1; i < n; i++) {
            if (nums[i] < first) {
                second = first;
                first = nums[i];
            } else if (nums[i] < second) second = nums[i];
        }
        return nums[0] + first + second;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumCost(self, nums: List[int]) -> int:
        return nums[0] + sum(sorted(nums[1:])[:2])
```
