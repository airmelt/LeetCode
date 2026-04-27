# 2834 Find the Minimum Possible Sum of a Beautiful Array 找出美丽数组的最小和

__Description:__

You are given positive integers `n` and `target`.

An array `nums` is __beautiful__ if it meets the following conditions:

- `nums.length == n`.
- `nums` consists of pairwise __distinct__ __positive__ integers.
- There doesn't exist two __distinct__ indices, `i` and `j`, in the range `[0, n - 1]`, such that `nums[i] + nums[j] == target`.

Return _the __minimum__ possible sum that a beautiful array could have modulo_ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: n = 2, target = 3
Output: 4
Explanation: We can see that nums = [1,3] is beautiful.
```

- The array nums has length n = 2.
- The array nums consists of pairwise distinct positive integers.
- There doesn't exist two distinct indices, i and j, with nums[i] + nums[j] == 3.

It can be proven that 4 is the minimum possible sum that a beautiful array could have.

Example 2:

```text
Input: n = 3, target = 3
Output: 8
Explanation: We can see that nums = [1,3,4] is beautiful.
```

- The array nums has length n = 3.
- The array nums consists of pairwise distinct positive integers.
- There doesn't exist two distinct indices, i and j, with nums[i] + nums[j] == 3.

It can be proven that 8 is the minimum possible sum that a beautiful array could have.

Example 3:

```text
Input: n = 1, target = 1
Output: 1
Explanation: We can see, that nums = [1] is beautiful.
```

__Constraints:__

- `1 <= n <= 10 ^ 9`
- `1 <= target <= 10 ^ 9`

__题目描述:__

给你两个正整数: `n` 和 `target` 。

如果数组 `nums` 满足下述条件，则称其为 __美丽数组__ 。

- `nums.length == n`.
- `nums` 由两两互不相同的正整数组成。
- 在范围 `[0, n-1]` 内，__不存在__ 两个 __不同__ 下标 `i` 和 `j` ，使得 `nums[i] + nums[j] == target` 。

返回符合条件的美丽数组所可能具备的 __最小__ 和，并对结果进行取模 `10 ^ 9 + 7`。

__示例:__

示例 1：

```text
输入：n = 2, target = 3
输出：4
解释：nums = [1,3] 是美丽数组。
```

- nums 的长度为 n = 2 。
- nums 由两两互不相同的正整数组成。
- 不存在两个不同下标 i 和 j ，使得 nums[i] + nums[j] == 3 。

可以证明 4 是符合条件的美丽数组所可能具备的最小和。

示例 2：

```text
输入：n = 3, target = 3
输出：8
解释：
nums = [1,3,4] 是美丽数组。 
```

- nums 的长度为 n = 3 。
- nums 由两两互不相同的正整数组成。
- 不存在两个不同下标 i 和 j ，使得 nums[i] + nums[j] == 3 。

可以证明 8 是符合条件的美丽数组所可能具备的最小和。

示例 3：

```text
输入：n = 1, target = 1
输出：1
解释：nums = [1] 是美丽数组。
```

__提示：__

- `1 <= n <= 10 ^ 9`
- `1 <= target <= 10 ^ 9`

__思路:__

```text
数学
从 1 到 target >> 1 可以都选上
然后可以选 target 开始的 n - (target >> 1) 个数
如果 target >> 1 比 n 还要大
直接从 1 选到 n 即可
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumPossibleSum(int n, int target) 
    {
        return ((long long)min(target >> 1, n) * (min(target >> 1, n) + 1) + ((target << 1) + (long long)n - (long long)min(target >> 1, n) - 1) * (n - min(target >> 1, n)) >> 1) % (long long)(1e9 + 7LL);
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumPossibleSum(int n, int target) {
        return (int)(((long)Math.min(target >>> 1, n) * (Math.min(target >>> 1, n) + 1) + (n - (long)Math.min(target >>> 1, n) - 1 + (target << 1)) * (n - Math.min(target >>> 1, n)) >>> 1) % 1_000_000_007);
    }
}
```

__Python__:

```Python
class Solution:
    def minimumPossibleSum(self, n: int, target: int) -> int:
        return ((m := min(target >> 1, n)) * (m + 1) + ((target << 1) + n - m - 1) * (n - m) >> 1) % (10 ** 9 + 7)
```
