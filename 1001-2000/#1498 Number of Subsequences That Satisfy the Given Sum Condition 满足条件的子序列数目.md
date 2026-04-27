# 1498 Number of Subsequences That Satisfy the Given Sum Condition 满足条件的子序列数目

__Description:__

You are given an array of integers `nums` and an integer `target`.

Return _the number of __non-empty__ subsequences of_ `nums` _such that the sum of the minimum and maximum element on it is less or equal to_ `target`. Since the answer may be too large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: nums = [3,5,6,7], target = 9
Output: 4
Explanation: There are 4 subsequences that satisfy the condition.
[3] -> Min value + max value <= target (3 + 3 <= 9)
[3,5] -> (3 + 5 <= 9)
[3,5,6] -> (3 + 6 <= 9)
[3,6] -> (3 + 6 <= 9)
```

Example 2:

```text
Input: nums = [3,3,6,8], target = 10
Output: 6
Explanation: There are 6 subsequences that satisfy the condition. (nums can have repeated numbers).
[3] , [3] , [3,3], [3,6] , [3,6] , [3,3,6]
```

Example 3:

```text
Input: nums = [2,3,3,4,6,7], target = 12
Output: 61
Explanation: There are 63 non-empty subsequences, two of them do not satisfy the condition ([6,7], [7]).
Number of valid subsequences (63 - 2 = 61).
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`
- `1 <= target <= 10 ^ 6`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `target` 。

请你统计并返回 `nums` 中能满足其最小元素与最大元素的 __和__ 小于或等于 `target` 的 __非空__ 子序列的数目。

由于答案可能很大，请将结果对 `10 ^ 9 + 7` 取余后返回。

__示例:__

示例 1：

```text
输入：nums = [3,5,6,7], target = 9
输出：4
解释：有 4 个子序列满足该条件。
[3] -> 最小元素 + 最大元素 <= target (3 + 3 <= 9)
[3,5] -> (3 + 5 <= 9)
[3,5,6] -> (3 + 6 <= 9)
[3,6] -> (3 + 6 <= 9)
```

示例 2：

```text
输入：nums = [3,3,6,8], target = 10
输出：6
解释：有 6 个子序列满足该条件。（nums 中可以有重复数字）
[3] , [3] , [3,3], [3,6] , [3,6] , [3,3,6]
```

示例 3：

```text
输入：nums = [2,3,3,4,6,7], target = 12
输出：61
解释：共有 63 个非空子序列，其中 2 个不满足条件（[6,7], [7]）
有效序列总数为（63 - 2 = 61）
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`
- `1 <= target <= 10 ^ 6`

__思路:__

```text
排序 ➕ 双指针
由于只需要求子序列先排序
预处理 2 ^ i % MOD
由于只要最小值和最大值相加满足不大于 target 中的所有子数组都成立
给每个满足的子数组加上 2 ^ (right - left), 再取余即可
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 主要时间复杂度为排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numSubseq(vector<int>& nums, int target) 
    {
        int n = nums.size(), MOD = 1e9 + 7, result = 0, left = 0, right = n - 1;
        vector<int> count(n, 1);
        sort(nums.begin(), nums.end());
        for (int i = 1; i < n; i++) count[i] = (count[i - 1] << 1) % MOD;
        while (left <= right) 
        {
            if (nums[left] + nums[right] > target) --right;
            else result = (result + count[right - left++]) % MOD;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numSubseq(int[] nums, int target) {
        int n = nums.length, count[] = new int[n], MOD = 1_000_000_007, result = 0, left = 0, right = n - 1;
        count[0] = 1;
        Arrays.sort(nums);
        for (int i = 1; i < n; i++) count[i] = (count[i - 1] << 1) % MOD;
        while (left <= right) {
            if (nums[left] + nums[right] > target) --right;
            else result = (result + count[right - left++]) % MOD;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numSubseq(self, nums: List[int], target: int) -> int:
        nums.sort()
        result, mod, left, right = 0, 10 ** 9 + 7, 0, len(nums) - 1
        while left <= right:
            if nums[left] + nums[right] > target:
                right -= 1
            else:
                result += (1 << right - left)
                left += 1
        return result % mod
```
