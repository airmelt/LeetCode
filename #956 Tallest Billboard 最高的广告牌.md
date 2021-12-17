# 956 Tallest Billboard 最高的广告牌

__Description__:
You are installing a billboard and want it to have the largest height. The billboard will have two steel supports, one on each side. Each steel support must be an equal height.

You are given a collection of rods that can be welded together. For example, if you have rods of lengths 1, 2, and 3, you can weld them together to make a support of length 6.

Return the largest possible height of your billboard installation. If you cannot support the billboard, return 0.

__Example:__

Example 1:

Input: rods = [1,2,3,6]
Output: 6
Explanation: We have two disjoint subsets {1,2,3} and {6}, which have the same sum = 6.

Example 2:

Input: rods = [1,2,3,4,5,6]
Output: 10
Explanation: We have two disjoint subsets {2,3,5} and {4,6}, which have the same sum = 10.

Example 3:

Input: rods = [1,2]
Output: 0
Explanation: The billboard cannot be supported, so we return 0.

__Constraints:__

1 <= rods.length <= 20
1 <= rods[i] <= 1000
sum(rods[i]) <= 5000

__题目描述__:
你正在安装一个广告牌，并希望它高度最大。这块广告牌将有两个钢制支架，两边各一个。每个钢支架的高度必须相等。

你有一堆可以焊接在一起的钢筋 rods。举个例子，如果钢筋的长度为 1、2 和 3，则可以将它们焊接在一起形成长度为 6 的支架。

返回广告牌的最大可能安装高度。如果没法安装广告牌，请返回 0。

__示例 :__

示例 1：

输入：[1,2,3,6]
输出：6
解释：我们有两个不相交的子集 {1,2,3} 和 {6}，它们具有相同的和 sum = 6。

示例 2：

输入：[1,2,3,4,5,6]
输出：10
解释：我们有两个不相交的子集 {2,3,5} 和 {4,6}，它们具有相同的和 sum = 10。

示例 3：

输入：[1,2]
输出：0
解释：没法安装广告牌，所以返回 0。

__提示:__

0 <= rods.length <= 20
1 <= rods[i] <= 1000
钢筋的长度总和最多为 5000

__思路__:

动态规划
题目可以转化为每个数字乘以 -1, 0, 1 之后和为 0 的子数组中所有正数最大的和
比如 [1, 2, 3, 6] 可以转换为 [1, 2, 3, -6] 数组之和为 0, 正数部分为 1 + 2 + 3 = 6
用哈希表来存储每一步的结果, dp[i] 记录总和为 i 时正数的最大和
比如 [1, 2, 3], 初始化 dp[0] = 0
先计算 1, dp[0] = 0, dp[1] = 1, dp[-1] = 0
然后计算 2, 对 dp 中每一个键进行遍历, 比如 0 + 2 * -1/0/1 -> -2/0/2, 最后 dp[0] = 0, dp[1] = 2, dp[-1] = 1, dp[2] = 2, dp[-2] = 0, dp[3] = 3, dp[-3] = 0, 每个键都取最大值, 注意这里最大值应该取最新值, 键应该取之前的键
最后返回 dp[0]
时间复杂度为 O(sn), 空间复杂度为 O(sn), n 为 rods 数组的长度, s 为 数组中元素和

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int tallestBillboard(vector<int>& rods) 
    {
        unordered_map<int, int> m;
        m[0] = 0;
        for (const auto& x : rods)
        {
            unordered_map<int, int> cur(m);
            for (const auto& [k, v] : cur)
            {
                m[k + x] = max(m[k + x], cur[k] + x);
                m[k - x] = max(m[k - x], cur[k]);
            }
        }
        return m[0];
    }
};
```

__Java__:

```Java
class Solution {
    public int tallestBillboard(int[] rods) {
        int d = 0, n = rods.length, dp[][] = new int[n + 1][5001];
        for (int i = 0; i <= n; i++) Arrays.fill(dp[i], -5001);
        dp[0][0] = 0;
        for(int i = 0; i < n; ++i) {
            d += rods[i];
            for (int j = 0; j <= d; ++j) {
                dp[i + 1][j] = Math.max(dp[i + 1][j], dp[i][j]);
                if (j + rods[i] <= d) dp[i + 1][j + rods[i]] = Math.max(dp[i + 1][j + rods[i]], dp[i][j]);
                dp[i + 1][Math.abs(j - rods[i])] = Math.max(dp[i + 1][Math.abs(j - rods[i])], dp[i][j] + Math.min(rods[i], j));
            }
        }
        return dp[n][0];
    }
}
```

__Python__:

```Python
class Solution:
    def tallestBillboard(self, rods: List[int]) -> int:
        dp = { 0: 0 }
        for x in rods:
            for d, y in list(dp.items()):
                dp[d + x] = max(dp.get(x + d, 0), y)
                dp[abs(d - x)] = max(dp.get(abs(d - x), 0), y + min(d, x))
        return dp[0]
```
