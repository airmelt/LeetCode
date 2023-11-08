# 1877 Minimize Maximum Pair Sum in Array 数组中最大数对和的最小值

__Description:__

The __pair sum__ of a pair `(a,b)` is equal to `a + b`. The __maximum pair sum__ is the largest __pair sum__ in a list of pairs.

- For example, if we have pairs `(1,5)`, `(2,3)`, and `(4,4)`, the __maximum pair sum__ would be `max(1+5, 2+3, 4+4) = max(6, 5, 8) = 8`.

Given an array `nums` of __even__ length `n`, pair up the elements of `nums` into `n / 2` pairs such that:

- Each element of `nums` is in __exactly one__ pair, and
- The __maximum pair sum__ is __minimized__.

Return the minimized maximum pair sum after optimally pairing up the elements.

__Example:__

Example 1:

```text
Input: nums = [3,5,2,3]
Output: 7
Explanation: The elements can be paired up into pairs (3,3) and (5,2).
The maximum pair sum is max(3+3, 5+2) = max(6, 7) = 7.
```

Example 2:

```text
Input: nums = [3,5,4,2,4,6]
Output: 8
Explanation: The elements can be paired up into pairs (3,5), (4,4), and (6,2).
The maximum pair sum is max(3+5, 4+4, 6+2) = max(8, 8, 8) = 8.
```

__Constraints:__

- `n == nums.length`
- `2 <= n <= 10 ^ 5`
- `n` is __even__.
- `1 <= nums[i] <= 10 ^ 5`

__题目描述:__

一个数对 `(a,b)` 的 __数对和__ 等于 `a + b` 。__最大数对和__ 是一个数对数组中最大的 __数对和__ 。

- 比方说，如果我们有数对 `(1,5)` ， `(2,3)` 和 `(4,4)`，__最大数对和__ 为 `max(1+5, 2+3, 4+4) = max(6, 5, 8) = 8` 。

给你一个长度为 __偶数__ `n` 的数组 `nums` ，请你将 `nums` 中的元素分成 `n / 2` 个数对，使得:

- `nums` 中每个元素 __恰好__ 在 __一个__ 数对中，且
- __最大数对和__ 的值 __最小__ 。

请你在最优数对划分的方案下，返回最小的 最大数对和 。

__示例:__

示例 1：

```text
输入：nums = [3,5,2,3]
输出：7
解释：数组中的元素可以分为数对 (3,3) 和 (5,2) 。
最大数对和为 max(3+3, 5+2) = max(6, 7) = 7 。
```

示例 2：

```text
输入：nums = [3,5,4,2,4,6]
输出：8
解释：数组中的元素可以分为数对 (3,5)，(4,4) 和 (6,2) 。
最大数对和为 max(3+5, 4+4, 6+2) = max(8, 8, 8) = 8 。
```

__提示：__

- `n == nums.length`
- `2 <= n <= 10 ^ 5`
- `n` 是 __偶数__ 。
- `1 <= nums[i] <= 10 ^ 5`

__思路:__

```text
排序
先排序
然后选择头尾的数字的和的最小值即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    int minPairSum(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        int result = 0, n = nums.size(), total = n >> 1;
        for (int i = 0; i < total; i++) result = max(result, nums[i] + nums[n - i - 1]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minPairSum(int[] nums) {
        Arrays.sort(nums);
        int result = 0, n = nums.length, total = n >> 1;
        for (int i = 0; i < total; i++) result = Math.max(result, nums[i] + nums[n - i - 1]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minPairSum(self, nums: List[int]) -> int:
        nums.sort()
        result, n = 0, len(nums)
        for i in range(n >> 1):
            result = max(result, nums[i] + nums[n - i - 1])
        return result
```
