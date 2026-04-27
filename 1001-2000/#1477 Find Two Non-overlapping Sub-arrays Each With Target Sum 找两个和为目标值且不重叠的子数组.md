# 1477 Find Two Non-overlapping Sub-arrays Each With Target Sum 找两个和为目标值且不重叠的子数组

__Description:__

You are given an array of integers  `arr` and an integer  `target`.

You have to find __two non-overlapping sub-arrays__ of  `arr` each with a sum equal  `target`. There can be multiple answers so you have to find an answer where the sum of the lengths of the two sub-arrays is __minimum__.

Return _the minimum sum of the lengths_ of the two required sub-arrays, or return  `-1` if you cannot find such two sub-arrays.

__Example:__

Example 1:

```text
Input: arr = [3,2,2,4,3], target = 3
Output: 2
Explanation: Only two sub-arrays have sum = 3 ([3] and [3]). The sum of their lengths is 2.
```

Example 2:

```text
Input: arr = [7,3,4,7], target = 7
Output: 2
Explanation: Although we have three non-overlapping sub-arrays of sum = 7 ([7], [3,4] and [7]), but we will choose the first and third sub-arrays as the sum of their lengths is 2.
```

Example 3:

```text
Input: arr = [4,3,2,6,2,3,4], target = 6
Output: -1
Explanation: We have only one sub-array of sum = 6.
```

__Constraints:__

- `1 <= arr.length <= 10 ^ 5`
- `1 <= arr[i] <= 1000`
- `1 <= target <= 10 ^ 8`

__题目描述:__

给你一个整数数组  `arr` 和一个整数值  `target` 。

请你在  `arr` 中找 __两个互不重叠的子数组__ 且它们的和都等于  `target` 。可能会有多种方案，请你返回满足要求的两个子数组长度和的 __最小值__ 。

请返回满足要求的最小长度和，如果无法找到这样的两个子数组，请返回 -1 。

__示例:__

示例 1：

```text
输入：arr = [3,2,2,4,3], target = 3
输出：2
解释：只有两个子数组和为 3 （[3] 和 [3]）。它们的长度和为 2 。
```

示例 2：

```text
输入：arr = [7,3,4,7], target = 7
输出：2
解释：尽管我们有 3 个互不重叠的子数组和为 7 （[7], [3,4] 和 [7]），但我们会选择第一个和第三个子数组，因为它们的长度和 2 是最小值。
```

示例 3：

```text
输入：arr = [4,3,2,6,2,3,4], target = 6
输出：-1
解释：我们只有一个和为 6 的子数组。
```

示例 4：

```text
输入：arr = [5,5,4,4,5], target = 3
输出：-1
解释：我们无法找到和为 3 的子数组。
```

示例 5：

```text
输入：arr = [3,1,1,1,5,1,2,1], target = 3
输出：3
解释：注意子数组 [1,2] 和 [2,1] 不能成为一个方案因为它们重叠了。
```

__提示：__

- `1 <= arr.length <= 10 ^ 5`
- `1 <= arr[i] <= 1000`
- `1 <= target <= 10 ^ 8`

__思路:__

```text
动态规划 ➕ 滑动窗口
设 dp[i] 表示 [0, i) 中连续子数组和为 target 的最小长度
如果滑动窗口内的和大于 target 窗口向右滑动
初始化 dp[0] = n + 1, 表示一个取不到的大值
如果 s == target, 即滑动窗口中的和等于 target
那么 dp[right] 取当前窗口的长度或者前一个窗口的长度
结果更新为当前窗口的长度和 dp[left] 的和的较小值
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minSumOfLengths(vector<int>& arr, int target) 
    {
        int n = arr.size(), result = n + 1, s = 0, left = 0;
        vector<int> dp(n + 1, n + 1);
        for (int right = 0; right < n; right++)
        {
            s += arr[right];
            while (s > target) s -= arr[left++];
            if (s == target)
            {
                result = min(result, right - left + 1 + dp[left]);
                dp[right + 1] = min(dp[right], right - left + 1);
            }
            else dp[right + 1] = dp[right];
        }
        return result > n ? -1 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minSumOfLengths(int[] arr, int target) {
        int n = arr.length, result = n + 1, s = 0, left = 0, dp[] = new int[n + 1];
        dp[0] = n + 1;
        for (int right = 0; right < n; right++) {
            s += arr[right];
            while (s > target) s -= arr[left++];
            if (s == target) {
                result = Math.min(result, right - left + 1 + dp[left]);
                dp[right + 1] = Math.min(dp[right], right - left + 1);
            }
            else dp[right + 1] = dp[right];
        }
        return result > n ? -1 : result;
    }
}
```

__Python__:

```Python
class Solution:
    def minSumOfLengths(self, arr: List[int], target: int) -> int:
        dp, left, right, s, result = [(n := len(arr)) + 1] * (n + 1), 0, 0, 0, n + 1
        while right < n:
            s += arr[right]
            while s > target:
                s -= arr[left]
                left += 1
            if s == target:
                result = min(result, right - left + 1 + dp[left])
                dp[right + 1] = min(dp[right], right - left + 1)
            else:
                dp[right + 1] = dp[right]
            right += 1
        return -1 if result > n else result
```
