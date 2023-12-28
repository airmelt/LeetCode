# 1931 Painting a Grid With Three Different Colors 用三种不同颜色为网格涂色

__Description:__

You are given two integers `m` and `n`. Consider an `m x n` grid where each cell is initially white. You can paint each cell __red__, __green__, or __blue__. All cells __must__ be painted.

Return _the number of ways to color the grid with __no two adjacent cells having the same color___. Since the answer can be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

![1931-1](https://assets.leetcode.com/uploads/2021/06/22/colorthegrid.png)

```text
Input: m = 1, n = 1
Output: 3
Explanation: The three possible colorings are shown in the image above.
```

Example 2:

![1931-2](https://assets.leetcode.com/uploads/2021/06/22/copy-of-colorthegrid.png)

```text
Input: m = 1, n = 2
Output: 6
Explanation: The six possible colorings are shown in the image above.
```

Example 3:

```text
Input: m = 5, n = 5
Output: 580986
```

__Constraints:__

- `1 <= m <= 5`
- `1 <= n <= 1000`

__题目描述:__

给你两个整数 `m` 和 `n` 。构造一个 `m x n` 的网格，其中每个单元格最开始是白色。请你用 __红、绿、蓝__ 三种颜色为每个单元格涂色。所有单元格都需要被涂色。

涂色方案需要满足:__不存在相邻两个单元格颜色相同的情况__ 。返回网格涂色的方法数。因为答案可能非常大， 返回 __对__ `10 ^ 9 + 7` __取余__ 的结果。

__示例:__

示例 1：

![1931-3](https://assets.leetcode.com/uploads/2021/06/22/colorthegrid.png)

```text
输入：m = 1, n = 1
输出：3
解释：如上图所示，存在三种可能的涂色方案。
```

示例 2：

![1931-4](https://assets.leetcode.com/uploads/2021/06/22/copy-of-colorthegrid.png)

```text
输入：m = 1, n = 2
输出：6
解释：如上图所示，存在六种可能的涂色方案。
```

示例 3：

```text
输入：m = 5, n = 5
输出：580986
```

__提示：__

- `1 <= m <= 5`
- `1 <= n <= 1000`

__思路:__

```text
状态压缩 ➕ 动态规划
由于只有 3 种颜色, 用一个三进制数记录颜色种类
首先先搜索一行可以放置的颜色的种类
然后搜索邻接行能够放置的颜色的种类

时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int colorTheGrid(int m, int n) 
    {
        int MOD = 1e9 + 7, result = 0, total = pow(3, m);
        unordered_map<int, vector<int>> valid, adjacent;
        for (int mask = 0; mask < total; ++mask) 
        {
            vector<int> color;
            for (int i = 0, cur = mask; i < m; i++) 
            {
                color.push_back(cur % 3);
                cur /= 3;
            }
            bool check = true;
            for (int i = 0; i < m - 1; i++) 
            {
                if (color[i] == color[i + 1]) 
                {
                    check = false;
                    break;
                }
            }
            if (check) valid[mask] = move(color);
        }
        for (const auto& [mask1, color1]: valid) 
        {
            for (const auto& [mask2, color2]: valid) 
            {
                bool check = true;
                for (int i = 0; i < m; i++) 
                {
                    if (color1[i] == color2[i]) 
                    {
                        check = false;
                        break;
                    }
                }
                if (check) adjacent[mask1].push_back(mask2);
            }
        }
        vector<int> dp(total);
        for (const auto& [mask, _]: valid) dp[mask] = 1;
        for (int i = 1; i < n; i++) 
        {
            vector<int> next_dp(total);
            for (const auto& [mask2, _]: valid) 
            {
                for (int mask1: adjacent[mask2]) 
                {
                    next_dp[mask2] += dp[mask1];
                    next_dp[mask2] %= MOD;
                }
            }
            dp = move(next_dp);
        }
        for (const auto& num: dp) 
        {
            result += num;
            result %= MOD;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int colorTheGrid(int m, int n) {
        int MOD = 1_000_000_007, result = 0, total = (int)Math.pow(3, m), dp[] = new int[total];
        Map<Integer, List<Integer>> valid = new HashMap<>(), adjacent = new HashMap<>();
        for (int mask = 0; mask < total; mask++) {
            List<Integer> color = new ArrayList<>();
            for (int i = 0, cur = mask; i < m; i++) {
                color.add(cur % 3);
                cur /= 3;
            }
            boolean check = true;
            for (int i = 0; i < m - 1; i++) {
                if (color.get(i) == color.get(i + 1)) {
                    check = false;
                    break;
                }
            }
            if (check) valid.put(mask, color);
        }
        for (Integer mask1 : valid.keySet()) {
            List<Integer> list = new ArrayList<>();
            for (Integer mask2 : valid.keySet()) {
                boolean check = true;
                for (int i = 0; i < m; i++) {
                    if (valid.get(mask1).get(i) == valid.get(mask2).get(i)) {
                        check = false;
                        break;
                    }
                }
                if (check) list.add(mask2);
            }
            adjacent.put(mask1, list);
        }
        for (Integer mask : valid.keySet()) dp[mask] = 1;
        for (int i = 1; i < n; i++) {
            int[] nextDp = new int[total];
            for (Integer mask1 : valid.keySet()) {
                for (Integer mask2 : adjacent.get(mask1)) {
                    nextDp[mask1] += dp[mask2];
                    nextDp[mask1] %= MOD;
                }
            }
            dp = nextDp;
        }
        for (Integer mask : valid.keySet()) {
            result += dp[mask];
            result %= MOD;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def colorTheGrid(self, m: int, n: int) -> int:
        if m == 1:
            return 3 * pow(2, n - 1, (10 ** 9 + 7)) % (10 ** 9 + 7)
        if m == 2:
            return 6 * pow(3, n - 1, (10 ** 9 + 7)) % (10 ** 9 + 7)
        if m == 3:
            a = b = 6
            for i in range(n - 1):
                a, b = (a + b) << 1, (a << 1) + b * 3
            return (a + b) % (10 ** 9 + 7)
        if m == 4:
            a, b, c = 6, 12, 6
            for i in range(n - 1):
                a, b, c = (a << 1) + b + c, (a << 1) + (b << 2) + (c << 2), a + (b << 1) + c * 3
            return (a + b + c) % (10 ** 9 + 7)
        a, b, c, d, e, f = 6, 12, 6, 6, 12, 6
        for i in range(n - 1):
            a, b, c, d, e, f = (a << 1) + b + e, (a << 1) + b * 3 + (c << 1) + (d << 1) + (e << 1) + (f << 1), b + (c << 1) + (d << 1) + e + (f << 1), b + (c << 1) + d * 3 + (e << 1) + (f << 1), (a << 1) + (b << 1) + (c << 1) + (d << 2) + e * 3 + (f << 2), b + (c << 1) + (d << 1) + (e << 1) + (f << 1)
        return (a + b + c + d + e + f) % (10 ** 9 + 7)
```
