# 1975 Maximum Matrix Sum 最大方阵和

__Description:__

You are given an `n x n` integer `matrix`. You can do the following operation __any__ number of times:

- Choose any two __adjacent__ elements of `matrix` and __multiply__ each of them by `-1`.

Two elements are considered adjacent if and only if they share a border.

Your goal is to maximize the summation of the matrix's elements. Return the maximum sum of the matrix's elements using the operation mentioned above.

__Example:__

Example 1:

![1975-1](https://assets.leetcode.com/uploads/2021/07/16/pc79-q2ex1.png)

```text
Input: matrix = [[1,-1],[-1,1]]
Output: 4
Explanation: We can follow the following steps to reach sum equals 4:
```

- Multiply the 2 elements in the first row by -1.
- Multiply the 2 elements in the first column by -1.

Example 2:

![1975-2](https://assets.leetcode.com/uploads/2021/07/16/pc79-q2ex2.png)

```text
Input: matrix = [[1,2,3],[-1,-2,-3],[1,2,3]]
Output: 16
Explanation: We can follow the following step to reach sum equals 16:
```

- Multiply the 2 last elements in the second row by -1.

__Constraints:__

- `n == matrix.length == matrix[i].length`
- `2 <= n <= 250`
- `-10 ^ 5 <= matrix[i][j] <= 10 ^ 5`

__题目描述:__

给你一个 `n x n` 的整数方阵 `matrix` 。你可以执行以下操作 __任意次__ :

- 选择 `matrix` 中 __相邻__ 两个元素，并将它们都 __乘以__ `-1` 。

如果两个元素有 公共边 ，那么它们就是 相邻 的。

你的目的是 最大化 方阵元素的和。请你在执行以上操作之后，返回方阵的 最大 和。

__示例:__

示例 1：

![1975-3](https://assets.leetcode.com/uploads/2021/07/16/pc79-q2ex1.png)

```text
输入：matrix = [[1,-1],[-1,1]]
输出：4
解释：我们可以执行以下操作使和等于 4 ：
```

- 将第一行的 2 个元素乘以 -1 。
- 将第一列的 2 个元素乘以 -1 。

示例 2：

![1975-4](https://assets.leetcode.com/uploads/2021/07/16/pc79-q2ex2.png)

```text
输入：matrix = [[1,2,3],[-1,-2,-3],[1,2,3]]
输出：16
解释：我们可以执行以下操作使和等于 16 ：
```

- 将第二行的最后 2 个元素乘以 -1 。

__提示：__

- `n == matrix.length == matrix[i].length`
- `2 <= n <= 250`
- `-10 ^ 5 <= matrix[i][j] <= 10 ^ 5`

__思路:__

```text
贪心
对于两个负数, 总能使它们变成正数
对于奇数个负数, 总能保留其中一个最小的负数, 使其他负数变成正数
统计所有元素的绝对值之和, 如果负数个数为奇数, 则减去最小的负数绝对值的两倍
时间复杂度为 O(MN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maxMatrixSum(vector<vector<int>>& matrix) 
    {
        int n = matrix.size(), cnt = 0, mn = INT_MAX;
        long long total = 0;
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                mn = min(mn, abs(matrix[i][j]));
                if (matrix[i][j] < 0) ++cnt;
                total += abs(matrix[i][j]);
            }
        }
        return cnt & 1 ? total - (mn << 1) : total;
    }
};
```

__Java__:

```Java
class Solution {
    public long maxMatrixSum(int[][] matrix) {
        int n = matrix.length, cnt = 0, mn = Integer.MAX_VALUE;
        long total = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                mn = Math.min(mn, Math.abs(matrix[i][j]));
                if (matrix[i][j] < 0) ++cnt;
                total += Math.abs(matrix[i][j]);
            }
        }
        return (cnt & 1) == 1 ? total - (mn << 1) : total;
    }
}
```

__Python__:

```Python
class Solution:
    def maxMatrixSum(self, matrix: List[List[int]]) -> int:
        return sum(abs(item) for row in matrix for item in row) - (min(abs(item) for row in matrix for item in row) << 1) if sum(item < 0 for row in matrix for item in row) & 1 else sum(abs(item) for row in matrix for item in row)
```
