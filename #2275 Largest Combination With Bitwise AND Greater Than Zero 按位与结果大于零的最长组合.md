# 2275 Largest Combination With Bitwise AND Greater Than Zero 按位与结果大于零的最长组合

__Description:__

The __bitwise AND__ of an array `nums` is the bitwise AND of all integers in `nums`.

- For example, for `nums = [1, 5, 3]`, the bitwise AND is equal to `1 & 5 & 3 = 1`.
- Also, for `nums = [7]`, the bitwise AND is `7`.

You are given an array of positive integers `candidates`. Evaluate the __bitwise AND__ of every __combination__ of numbers of `candidates`. Each number in `candidates` may only be used __once__ in each combination.

Return _the size of the __largest__ combination of_ `candidates` _with a bitwise AND __greater__ than_ `0`.

__Example:__

Example 1:

```text
Input: candidates = [16,17,71,62,12,24,14]
Output: 4
Explanation: The combination [16,17,62,24] has a bitwise AND of 16 & 17 & 62 & 24 = 16 > 0.
The size of the combination is 4.
It can be shown that no combination with a size greater than 4 has a bitwise AND greater than 0.
Note that more than one combination may have the largest size.
For example, the combination [62,12,24,14] has a bitwise AND of 62 & 12 & 24 & 14 = 8 > 0.
```

Example 2:

```text
Input: candidates = [8,8]
Output: 2
Explanation: The largest combination [8,8] has a bitwise AND of 8 & 8 = 8 > 0.
The size of the combination is 2, so we return 2.
```

__Constraints:__

- `1 <= candidates.length <= 10 ^ 5`
- `1 <= candidates[i] <= 10 ^ 7`

__题目描述:__

对数组 `nums` 执行 __按位与__ 相当于对数组 `nums` 中的所有整数执行 __按位与__ 。

- 例如，对 `nums = [1, 5, 3]` 来说，按位与等于 `1 & 5 & 3 = 1` 。
- 同样，对 `nums = [7]` 而言，按位与等于 `7` 。

给你一个正整数数组 `candidates` 。计算 `candidates` 中的数字每种组合下 __按位与__ 的结果。 `candidates` 中的每个数字在每种组合中只能使用 __一次__ 。

返回按位与结果大于 `0` 的 __最长__ 组合的长度_。_

__示例:__

示例 1：

```text
输入：candidates = [16,17,71,62,12,24,14]
输出：4
解释：组合 [16,17,62,24] 的按位与结果是 16 & 17 & 62 & 24 = 16 > 0 。
组合长度是 4 。
可以证明不存在按位与结果大于 0 且长度大于 4 的组合。
注意，符合长度最大的组合可能不止一种。
例如，组合 [62,12,24,14] 的按位与结果是 62 & 12 & 24 & 14 = 8 > 0 。
```

示例 2：

```text
输入：candidates = [8,8]
输出：2
解释：最长组合是 [8,8] ，按位与结果 8 & 8 = 8 > 0 。
组合长度是 2 ，所以返回 2 。
```

__提示：__

- `1 <= candidates.length <= 10 ^ 5`
- `1 <= candidates[i] <= 10 ^ 7`

__思路:__

```text
位运算
即计算每个数字的二进制表示中 1 的个数
取最大值即可
时间复杂度为 O(N), 空间复杂度为 O(1), 由于题目中的数字范围为 10 ^ 7, 因此可以直接使用 24 位的数组
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int largestCombination(vector<int>& candidates) 
    {
        vector<int> dp(24);
        for (int c : candidates) for (int i = 0; i < 24; i++) if (((1 << i) & c)) ++dp[i];
        return *max_element(dp.begin(), dp.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int largestCombination(int[] candidates) {
        int[] dp = new int[24];
        for (int c : candidates) for (int i = 0; i < 24; i++) if (((1 << i) & c) > 0) ++dp[i];
        return Arrays.stream(dp).max().getAsInt();
    }
}
```

__Python__:

```Python
class Solution:
    def largestCombination(self, candidates: List[int]) -> int:
        return max(sum(num >> i & 1 for num in candidates) for i in range(24))
```
