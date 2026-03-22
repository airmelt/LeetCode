# 1536 Minimum Swaps to Arrange a Binary Grid 排布二进制网格的最少交换次数

__Description:__

Given an `n x n` binary `grid`, in one step you can choose two __adjacent rows__ of the grid and swap them.

A grid is said to be valid if all the cells above the main diagonal are zeros.

Return the minimum number of steps needed to make the grid valid, or -1 if the grid cannot be valid.

The main diagonal of a grid is the diagonal that starts at cell `(1, 1)` and ends at cell `(n, n)`.

__Example:__

Example 1:

![1536-1](https://assets.leetcode.com/uploads/2020/07/28/fw.jpg)

```text
Input: grid = [[0,0,1],[1,1,0],[1,0,0]]
Output: 3
```

Example 2:

![1536-2](https://assets.leetcode.com/uploads/2020/07/16/e2.jpg)

```text
Input: grid = [[0,1,1,0],[0,1,1,0],[0,1,1,0],[0,1,1,0]]
Output: -1
Explanation: All rows are similar, swaps have no effect on the grid.
```

Example 3:

![1536-3](https://assets.leetcode.com/uploads/2020/07/16/e3.jpg)

```text
Input: grid = [[1,0,0],[1,1,0],[1,1,1]]
Output: 0
```

__Constraints:__

- `n == grid.length` `== grid[i].length`
- `1 <= n <= 200`
- `grid[i][j]` is either `0` or `1`

__题目描述:__

给你一个 `n x n` 的二进制网格 `grid`，每一次操作中，你可以选择网格的 __相邻两行__ 进行交换。

一个符合要求的网格需要满足主对角线以上的格子全部都是 0 。

请你返回使网格满足要求的最少操作次数，如果无法使网格符合要求，请你返回 -1 。

主对角线指的是从 `(1, 1)` 到 `(n, n)` 的这些格子。

__示例:__

示例 1：

![1536-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/02/fw.jpg)

```text
输入：grid = [[0,0,1],[1,1,0],[1,0,0]]
输出：3
```

示例 2：

![1536-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/02/e2.jpg)

```text
输入：grid = [[0,1,1,0],[0,1,1,0],[0,1,1,0],[0,1,1,0]]
输出：-1
解释：所有行都是一样的，交换相邻行无法使网格符合要求。
```

示例 3：

![1536-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/02/e3.jpg)

```text
输入：grid = [[1,0,0],[1,1,0],[1,1,1]]
输出：0
```

__提示：__

- `n == grid.length`
- `n == grid[i].length`
- `1 <= n <= 200`
- `grid[i][j]` 要么是 `0` 要么是 `1` 。

__思路:__

```text
贪心
每次找到当前近的满足当前行的最小下标, 通过将该行换到当前行可以获得最少交换次数
先预处理每行结尾的 0 的位置
然后从第 i 行开始往后找, 找到第一个满足可交换的位置, 将该行交换成可交换的行
如果找到可交换的位置, 需要把预处理的数组进行交换, 并累积交换次数
如果没找到可交换的位置, 返回 -1
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minSwaps(vector<vector<int>>& grid) 
    {
        int result = 0, n = grid.size();
        vector<int> pos(n, -1);
        for (int i = 0; i < n; i++) 
        {
            for (int j = n - 1; j > -1; j--) 
            {
                if (grid[i][j]) 
                {
                    pos[i] = j;
                    break;
                }
            }
        }
        for (int i = 0; i < n; i++) 
        {
            int k = -1;
            for (int j = i; j < n; j++) 
            {
                if (pos[j] <= i) 
                {
                    result += j - i;
                    k = j;
                    break;
                }
            }
            if (k != -1) for (int j = k; j > i; j--) swap(pos[j], pos[j - 1]);
            else return k;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minSwaps(int[][] grid) {
        int result = 0, n = grid.length, pos[] = new int[n];
        Arrays.fill(pos, -1);
        for (int i = 0; i < n; i++) {
            for (int j = n - 1; j > -1; j--) {
                if (grid[i][j] == 1) {
                    pos[i] = j;
                    break;
                }
            }
        }
        for (int i = 0; i < n; i++) {
            int k = -1;
            for (int j = i; j < n; j++) {
                if (pos[j] <= i) {
                    result += j - i;
                    k = j;
                    break;
                }
            }
            if (k != -1) {
                for (int j = k; j > i; j--) {
                    int temp = pos[j];
                    pos[j] = pos[j - 1];
                    pos[j - 1] = pos[j];
                }
            } else return k;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minSwaps(self, grid: List[List[int]]) -> int:
        result, n = 0, len(grid)
        for i in range(n - 1):
            for j in range(i, n):
                if len(list(takewhile((0).__eq__, reversed(grid[j])))) >= n - i - 1:
                    while i < j:
                        grid[j - 1], grid[j] = grid[j], grid[j - 1]
                        result += 1
                        j -= 1
                    break
            else:
                return -1
        return result
```
