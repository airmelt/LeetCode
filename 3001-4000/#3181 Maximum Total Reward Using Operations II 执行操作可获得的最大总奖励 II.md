# 3181 Maximum Total Reward Using Operations II 执行操作可获得的最大总奖励 II

__Description:__

You are given an integer array `rewardValues` of length `n`, representing the values of rewards.

Initially, your total reward `x` is 0, and all indices are __unmarked__. You are allowed to perform the following operation __any__ number of times:

- Choose an __unmarked__ index `i` from the range `[0, n - 1]`.
- If `rewardValues[i]` is __greater__ than your current total reward `x`, then add `rewardValues[i]` to `x` (i.e., `x = x + rewardValues`[i]), and __mark__ the index `i`.

Return an integer denoting the __maximum__ _total reward_ you can collect by performing the operations optimally.

__Example:__

Example 1:

```text
Input: rewardValues = [1,1,3,3]

Output: 4

Explanation:

During the operations, we can choose to mark the indices 0 and 2 in order, and the total reward will be 4, which is the maximum.
```

Example 2:

```text
Input: rewardValues = [1,6,4,3,2]

Output: 11

Explanation:

Mark the indices 0, 2, and 1 in order. The total reward will then be 11, which is the maximum.
```

__Constraints:__

- `1 <= rewardValues.length <= 5 * 10 ^ 4`
- `1 <= rewardValues[i] <= 5 * 10 ^ 4`

__题目描述:__

给你一个整数数组 `rewardValues`，长度为 `n`，代表奖励的值。

最初，你的总奖励 `x` 为 0，所有下标都是 __未标记__ 的。你可以执行以下操作 __任意次__ ：

- 从区间 `[0, n - 1]` 中选择一个 __未标记__ 的下标 `i`。
- 如果 `rewardValues[i]` __大于__ 你当前的总奖励 `x`，则将 `rewardValues[i]` 加到 `x` 上（即 `x = x + rewardValues[i]`），并 __标记__ 下标 i。

以整数形式返回执行最优操作能够获得的 __最大__ 总奖励。

__示例:__

示例 1：

```text
输入：rewardValues = [1,1,3,3]

输出：4

解释：

依次标记下标 0 和 2，总奖励为 4，这是可获得的最大值。
```

示例 2：

```text
输入：rewardValues = [1,6,4,3,2]

输出：11

解释：

依次标记下标 0、2 和 1。总奖励为 11，这是可获得的最大值。
```

__提示：__

- `1 <= rewardValues.length <= 5 * 10 ^ 4`
- `1 <= rewardValues[i] <= 5 * 10 ^ 4`

__思路:__

```text
动态规划
设 dp[i][j] 表示是否从 nums[:i] 中能选出奖励为 j 
如果不选择当前的值 v, dp[i][j] = dp[i - 1][j]
如果选择当前的值 v, dp[i][j] = dp[i - 1][j - v], j - v >= 0, 并且按照题意 j - v < v, 即 v <= j < 2v
综上所述, dp[i][j] = dp[i - 1][j] | dp[i - 1][j - v]
注意到 dp[i] 只和 dp[i - 1] 有关, 可以将第一维优化掉
设 rewardValues 最大值为 m
那么最多只能选择 2 * m - 1, 即 j 的取值范围为 [1, 2 * m - 1]
这是因为如果最后一步选择了比 m 小的元素, 总可以替换为 m, 而上一步的总和最大只能为 m - 1, 才能满足最后一次选择是有效的, m - 1 + m = 2 * m - 1
先将 rewardValues 去重和排序
如果找到 m - 1, 或者找到两个数之和为 m - 1, 一定可以选到最大值 2 * m - 1
对于 dp[j] = dp[j] | dp[j - v], 还可以进一步优化为一个二进制数
这个二进制数的第 j 位为 1 表示可以取到 j
对于 v, v 以下的数一定都能取到, 因为最开始为 0 可以直接取 v
令 mask = (1 << v) - 1, 这些都能取到
dp 的低位 lowbit = dp & mask
dp |= lowbit << v
即把 dp 的低 lowbit 位左移 v 位合入 dp 中
初始值 dp = 1
最后返回 dp 的最高位, 即 dp.bit_length() - 1
或者从 2 * m - 1 位开始往 0 遍历, 第一位不为 0 的即为答案
时间复杂度为 O(NM), 空间复杂度为 O(N + M), N 为数组 rewardValues 的长度, M 为数组 rewardValues 的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxTotalReward(vector<int>& rewardValues) 
    {
        int m = *max_element(rewardValues.begin(), rewardValues.end()), shift = 0;
        unordered_set<int> s;
        for (const auto& v : rewardValues) if (!s.contains(v) and !s.insert(v).second and (v == m - 1 or s.contains(m - 1 - v))) return (m << 1) - 1;
        sort(rewardValues.begin(), rewardValues.end());
        rewardValues.erase(ranges::unique(rewardValues).begin(), rewardValues.end());
        bitset<100000> dp{1};
        for (const auto& v : rewardValues) dp |= dp << (shift = dp.size() - v) >> (shift - v); 
        for (int i = (m << 1) - 1; ; i--) if (dp[i]) return i;
    }
};
```

__Java__:

```Java
import java.math.BigInteger;
class Solution {
    public int maxTotalReward(int[] rewardValues) {
        int m = Arrays.stream(rewardValues).max().getAsInt();
        for (int v : rewardValues) if (v == m - 1) return (m << 1) - 1;
        BigInteger dp = BigInteger.ONE, mask = null;
        for (int v : Arrays.stream(rewardValues).distinct().sorted().toArray()) dp = dp.or(dp.and(mask = BigInteger.ONE.shiftLeft(v).subtract(BigInteger.ONE)).shiftLeft(v));
        return dp.bitLength() - 1;
    }
}
```

__Python__:

```Python
class Solution:
    def maxTotalReward(self, rewardValues: List[int]) -> int:
        return (reduce(lambda x, y: x | (x & ((1 << y) - 1)) << y, [1] + sorted(set(rewardValues)))).bit_length() - 1
```
