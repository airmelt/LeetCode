# 2209 Minimum White Tiles After Covering With Carpets 用地毯覆盖后的最少白色砖块

__Description:__

You are given a __0-indexed binary__ string `floor`, which represents the colors of tiles on a floor:

- `floor[i] = '0'` denotes that the `i ^ th` tile of the floor is colored __black__.
- On the other hand, `floor[i] = '1'` denotes that the `i ^ th` tile of the floor is colored __white__.

You are also given `numCarpets` and `carpetLen`. You have `numCarpets` __black__ carpets, each of length `carpetLen` tiles. Cover the tiles with the given carpets such that the number of __white__ tiles still visible is __minimum__. Carpets may overlap one another.

Return the minimum number of white tiles still visible.

__Example:__

Example 1:

![2209-1](https://assets.leetcode.com/uploads/2022/02/10/ex1-1.png)

```text
Input: floor = "10110101", numCarpets = 2, carpetLen = 2
Output: 2
Explanation: 
The figure above shows one way of covering the tiles with the carpets such that only 2 white tiles are visible.
No other way of covering the tiles with the carpets can leave less than 2 white tiles visible.
```

Example 2:

![2209-2](https://assets.leetcode.com/uploads/2022/02/10/ex2.png)

```text
Input: floor = "11111", numCarpets = 2, carpetLen = 3
Output: 0
Explanation: 
The figure above shows one way of covering the tiles with the carpets such that no white tiles are visible.
Note that the carpets are able to overlap one another.
```

__Constraints:__

- `1 <= carpetLen <= floor.length <= 1000`
- `floor[i]` is either `'0'` or `'1'`.
- `1 <= numCarpets <= 1000`

__题目描述:__

给你一个下标从 __0__ 开始的 __二进制__ 字符串 `floor` ，它表示地板上砖块的颜色。

- `floor[i] = '0'` 表示地板上第 `i` 块砖块的颜色是 __黑色__ 。
- `floor[i] = '1'` 表示地板上第 `i` 块砖块的颜色是 __白色__ 。

同时给你 `numCarpets` 和 `carpetLen` 。你有 `numCarpets` 条 __黑色__ 的地毯，每一条 __黑色__ 的地毯长度都为 `carpetLen` 块砖块。请你使用这些地毯去覆盖砖块，使得未被覆盖的剩余 __白色__ 砖块的数目 __最小__ 。地毯相互之间可以覆盖。

请你返回没被覆盖的白色砖块的 最少 数目。

__示例:__

示例 1：

![2209-3](https://assets.leetcode.com/uploads/2022/02/10/ex1-1.png)

```text
输入：floor = "10110101", numCarpets = 2, carpetLen = 2
输出：2
解释：
上图展示了剩余 2 块白色砖块的方案。
没有其他方案可以使未被覆盖的白色砖块少于 2 块。
```

示例 2：

![2209-4](https://assets.leetcode.com/uploads/2022/02/10/ex2.png)

```text
输入：floor = "11111", numCarpets = 2, carpetLen = 3
输出：0
解释：
上图展示了所有白色砖块都被覆盖的一种方案。
注意，地毯相互之间可以覆盖。
```

__提示：__

- `1 <= carpetLen <= floor.length <= 1000`
- `floor[i]` 要么是 `'0'` ，要么是 `'1'` 。
- `1 <= numCarpets <= 1000`

__思路:__

```text
动态规划
定义 dp[i][j] 表示使用 i 条地毯覆盖前 j 个砖块时, 未被覆盖的白色砖块的最少数量
初始化 dp[0][j] = pre[j] 表示未使用地毯时的情况
pre 为前缀和数组, pre[j] 表示前 j 个砖块中白色砖块的数量
如果不使用地毯 dp[i][j] = dp[i][j - 1] + (floor[j] == '1')
如果使用地毯 dp[i][j] = dp[i - 1][j - carpetLen], 这里需要注意 j >= carpetLen * i, 前 carpetLen * i 个砖块可以使用 i 条地毯完全覆盖
dp[i][j] = min(dp[i][j - 1] + (floor[j] == '1'), dp[i - 1][j - carpetLen])
最终返回 dp[numCarpets][n - 1]
可以使用滚动数组优化空间复杂度
时间复杂度为 O(NM), 空间复杂度为 O(N), 其中 N 为砖块的数量, M 为地毯的数量
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumWhiteTiles(string floor, int numCarpets, int carpetLen) 
    {
        int n = floor.size();
        if (numCarpets * carpetLen >= n) return 0;
        vector<int> pre(n, floor.front() == '1'), dp(n);
        for (int i = 1; i < n; i++) pre[i] = pre[i - 1] + (floor[i] == '1');
        for (int i = 1; i <= numCarpets; i++)
        {
            dp[carpetLen * i - 1] = 0;
            for (int j = carpetLen * i; j < n; j++) dp[j] = min(dp[j - 1] + (floor[j] == '1'), pre[j - carpetLen]);
            swap(pre, dp);
        }
        return pre.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumWhiteTiles(String floor, int numCarpets, int carpetLen) {
        int n = floor.length(), dp[][] = new int[numCarpets + 1][n];
        if (numCarpets * carpetLen >= n) return 0;
        dp[0][0] = floor.charAt(0) == '1' ? 1 : 0;
        for (int i = 1; i < n; i++) dp[0][i] = dp[0][i - 1] + (floor.charAt(i) == '1' ? 1 : 0);
        for (int i = 1; i <= numCarpets; i++) for (int j = carpetLen * i; j < n; j++) dp[i][j] = Math.min(dp[i][j - 1] + (floor.charAt(j) == '1' ? 1 : 0), dp[i - 1][j - carpetLen]);
        return dp[numCarpets][n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def minimumWhiteTiles(self, floor: str, numCarpets: int, carpetLen: int) -> int:
        if numCarpets * carpetLen >= (n := len(floor)):
            return 0
        pre, dp = list(accumulate(floor[i] == '1' for i in range(n))), [0] * n
        for i in range(1, numCarpets + 1):
            dp[carpetLen * i - 1] = 0
            for j in range(carpetLen * i, n):
                dp[j] = min(dp[j - 1] + (floor[j] == '1'), pre[j - carpetLen])
            pre, dp = dp, pre
        return pre[-1]
```
