# 2579 Count Total Number of Colored Cells 统计染色格子数

__Description:__

There exists an infinitely large two-dimensional grid of uncolored unit cells. You are given a positive integer `n`, indicating that you must do the following routine for `n` minutes:

- At the first minute, color __any__ arbitrary unit cell blue.
- Every minute thereafter, color blue __every__ uncolored cell that touches a blue cell.

Below is a pictorial representation of the state of the grid after minutes 1, 2, and 3.

![2579-1](https://assets.leetcode.com/uploads/2023/01/10/example-copy-2.png)

Return _the number of __colored cells__ at the end of_ `n` _minutes_.

__Example:__

Example 1:

```text
Input: n = 1
Output: 1
Explanation: After 1 minute, there is only 1 blue cell, so we return 1.
```

Example 2:

```text
Input: n = 2
Output: 5
Explanation: After 2 minutes, there are 4 colored cells on the boundary and 1 in the center, so we return 5.
```

__Constraints:__

- `1 <= n <= 10 ^ 5`

__题目描述:__

有一个无穷大的二维网格图，一开始所有格子都未染色。给你一个正整数 `n` ，表示你需要执行以下步骤 `n` 分钟:

- 第一分钟，将 __任一__ 格子染成蓝色。
- 之后的每一分钟，将与蓝色格子相邻的 __所有__ 未染色格子染成蓝色。

下图分别是 1、2、3 分钟后的网格图。

![2579-2](https://assets.leetcode.com/uploads/2023/01/10/example-copy-2.png)

请你返回 `n` 分钟之后 __被染色的格子__ 数目。

__示例:__

示例 1：

```text
输入：n = 1
输出：1
解释：1 分钟后，只有 1 个蓝色的格子，所以返回 1 。
```

示例 2：

```text
输入：n = 2
输出：5
解释：2 分钟后，有 4 个在边缘的蓝色格子和 1 个在中间的蓝色格子，所以返回 5 。
```

__提示：__

- `1 <= n <= 10 ^ 5`

__思路:__

```text
数学
第二分钟之后每分钟染色的格子数为 4 * (n - 1)
第 1 分钟染色 1 个格子, 第 2 分钟染色 4 个格子, 第 3 分钟染色 8 个格子, 第 4 分钟染色 12 个格子, ...
因此总的染色格子数为
1 + 4 * (n - 1) + 4 * (n - 2) + ... + 4 * 1
= 1 + 4 * (n - 1) * n / 2
= 1 + 2 * (n - 1) * n
= 1 + 2 * n * (n - 1)
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long coloredCells(int n) 
    {
        return 1LL + (n * (n - 1LL) << 1LL);
    }
};
```

__Java__:

```Java
class Solution {
    public long coloredCells(int n) {
        return 1L + (n * (n - 1L) << 1L);
    }
}
```

__Python__:

```Python
class Solution:
    def coloredCells(self, n: int) -> int:
        return 1 + (n * (n - 1) << 1)
```
