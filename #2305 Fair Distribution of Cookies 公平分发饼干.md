# 2305 Fair Distribution of Cookies 公平分发饼干

__Description:__

You are given an integer array `cookies`, where `cookies[i]` denotes the number of cookies in the `i ^ th` bag. You are also given an integer `k` that denotes the number of children to distribute __all__ the bags of cookies to. All the cookies in the same bag must go to the same child and cannot be split up.

The unfairness of a distribution is defined as the maximum total cookies obtained by a single child in the distribution.

Return the minimum unfairness of all distributions.

__Example:__

Example 1:

```text
Input: cookies = [8,15,10,20,8], k = 2
Output: 31
Explanation: One optimal distribution is [8,15,8] and [10,20]
```

- The 1st child receives [8,15,8] which has a total of 8 + 15 + 8 = 31 cookies.
- The 2nd child receives [10,20] which has a total of 10 + 20 = 30 cookies.

The unfairness of the distribution is max(31,30) = 31.
It can be shown that there is no distribution with an unfairness less than 31.

Example 2:

```text
Input: cookies = [6,1,3,2,2,4,1,2], k = 3
Output: 7
Explanation: One optimal distribution is [6,1], [3,2,2], and [4,1,2]
```

- The 1st child receives [6,1] which has a total of 6 + 1 = 7 cookies.
- The 2nd child receives [3,2,2] which has a total of 3 + 2 + 2 = 7 cookies.
- The 3rd child receives [4,1,2] which has a total of 4 + 1 + 2 = 7 cookies.

The unfairness of the distribution is max(7,7,7) = 7.
It can be shown that there is no distribution with an unfairness less than 7.

__Constraints:__

- `2 <= cookies.length <= 8`
- `1 <= cookies[i] <= 10 ^ 5`
- `2 <= k <= cookies.length`

__题目描述:__

给你一个整数数组 `cookies` ，其中 `cookies[i]` 表示在第 `i` 个零食包中的饼干数量。另给你一个整数 `k` 表示等待分发零食包的孩子数量，__所有__ 零食包都需要分发。在同一个零食包中的所有饼干都必须分发给同一个孩子，不能分开。

分发的 不公平程度 定义为单个孩子在分发过程中能够获得饼干的最大总数。

返回所有分发的最小不公平程度。

__示例:__

示例 1：

```text
输入：cookies = [8,15,10,20,8], k = 2
输出：31
解释：一种最优方案是 [8,15,8] 和 [10,20] 。
```

- 第 1 个孩子分到 [8,15,8] ，总计 8 + 15 + 8 = 31 块饼干。
- 第 2 个孩子分到 [10,20] ，总计 10 + 20 = 30 块饼干。

分发的不公平程度为 max(31,30) = 31 。
可以证明不存在不公平程度小于 31 的分发方案。

示例 2：

```text
输入：cookies = [6,1,3,2,2,4,1,2], k = 3
输出：7
解释：一种最优方案是 [6,1]、[3,2,2] 和 [4,1,2] 。
```

- 第 1 个孩子分到 [6,1] ，总计 6 + 1 = 7 块饼干。
- 第 2 个孩子分到 [3,2,2] ，总计 3 + 2 + 2 = 7 块饼干。
- 第 3 个孩子分到 [4,1,2] ，总计 4 + 1 + 2 = 7 块饼干。

分发的不公平程度为 max(7,7,7) = 7 。
可以证明不存在不公平程度小于 7 的分发方案。

__提示：__

- `2 <= cookies.length <= 8`
- `1 <= cookies[i] <= 10 ^ 5`
- `2 <= k <= cookies.length`

__思路:__

```text
动态规划 ➕ 状态压缩
设 dp[i][j] 表示前 i 个孩子分到饼干组合 j 的最小不公平程度
其中 j 为一个二进制数, 表示每个孩子分到的饼干组合, 1 表示分到, 0 表示未分到, 例如 101 表示分了第 0 和第 2 个饼干给前 i 个孩子
如果第 i 个孩子分到的饼干组合为 s, 则前 i - 1 个孩子分到的饼干组合为 s ^ j
如果 s 对应的饼干数为 pre[s]
如果 pre[s] > dp[i - 1][s ^ j], 说明第 i 个孩子分配的饼干比前 i - 1 个孩子分配的饼干多, 则 dp[i][j] = pre[s]
否则 dp[i][j] = dp[i - 1][s ^ j]
所以 dp[i][j] = max(dp[i - 1][s ^ j], pre[s])
综上 dp[i][j] = min(dp[i][j], max(dp[i - 1][j ^ cur], pre[cur])), 其中 cur 为 j 的子集
子集可以用 (cur - 1) & j 来遍历
由于 dp[i] 只与 dp[i - 1] 有关, 可以用滚动数组优化空间复杂度
pre 数组用来记录每个子集的饼干数, 可以提前预处理
pre[bit | j] = pre[j] + cookies[i], 其中 bit 为 1 << i
时间复杂度为 O(K * 3 ^ N), 空间复杂度为 O(2 ^ N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int distributeCookies(vector<int>& cookies, int k) 
    {
        int n = cookies.size(), total = 1 << n;
        vector<int> pre(total);
        for (int i = 0; i < n; i++) for (int j = 0, bit = 1 << i; j < bit; j++) pre[bit | j] = pre[j] + cookies[i];
        vector<int> dp(pre);
        while (--k) 
        {
            for (int j = total - 1, cur = j; j > 0; cur = --j) 
            {
                while (cur) 
                {
                    dp[j] = min(dp[j], max(dp[j ^ cur], pre[cur]));
                    cur = (cur - 1) & j;
                }
                
            }
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int distributeCookies(int[] cookies, int k) {
        int n = cookies.length, total = 1 << n, pre[] = new int[total];
        for (int i = 0; i < n; i++) for (int j = 0, bit = 1 << i; j < bit; j++) pre[bit | j] = pre[j] + cookies[i];
        int[] dp = pre.clone();
        while (--k > 0) {
            for (int j = total - 1, cur = j; j > 0; cur = --j) {
                while (cur > 0) {
                    dp[j] = Math.min(dp[j], Math.max(dp[j ^ cur], pre[cur]));
                    cur = (cur - 1) & j;
                }
                
            }
        }
        return dp[total - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def distributeCookies(self, cookies: List[int], k: int) -> int:
        pre = [0] * (total := 1 << (n := len(cookies)))
        for i, v in enumerate(cookies):
            for j in range(1 << i):
                pre[(1 << i) | j] = pre[j] + v
        dp = pre[:]
        while (k := k - 1):
            for j in range(total - 1, 0, -1):
                cur = j
                while cur:
                    dp[j] = min(dp[j], max(dp[j ^ cur], pre[cur]))
                    cur = (cur - 1) & j
        return dp[-1]
```
