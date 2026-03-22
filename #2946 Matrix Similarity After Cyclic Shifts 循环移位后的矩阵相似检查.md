# 2946 Matrix Similarity After Cyclic Shifts 循环移位后的矩阵相似检查

__Description:__

You are given an `m x n` integer matrix `mat` and an integer `k`. The matrix rows are 0-indexed.

The following process happens `k` times:

- __Even-indexed__ rows (0, 2, 4, ...) are cyclically shifted to the left.

![2946-1](https://assets.leetcode.com/uploads/2024/05/19/lshift.jpg)

- __Odd-indexed__ rows (1, 3, 5, ...) are cyclically shifted to the right.

![2946-2](https://assets.leetcode.com/uploads/2024/05/19/rshift-stlone.jpg)

Return `true` if the final modified matrix after `k` steps is identical to the original matrix, and `false` otherwise.

__Example:__

Example 1:

```text
Input: mat = [[1,2,3],[4,5,6],[7,8,9]], k = 4

Output: false

Explanation:

In each step left shift is applied to rows 0 and 2 (even indices), and right shift to row 1 (odd index).
```

![2946-3](https://assets.leetcode.com/uploads/2024/05/19/t1-2.jpg)

Example 2:

```text
Input: mat = [[1,2,1,2],[5,5,5,5],[6,3,6,3]], k = 2

Output: true

Explanation:
```

![2946-4](https://assets.leetcode.com/uploads/2024/05/19/t1-3.jpg)

Example 3:

```text
Input: mat = [[2,2],[2,2]], k = 3

Output: true

Explanation:

As all the values are equal in the matrix, even after performing cyclic shifts the matrix will remain the same.
```

__Constraints:__

- `1 <= mat.length <= 25`
- `1 <= mat[i].length <= 25`
- `1 <= mat[i][j] <= 25`
- `1 <= k <= 50`

__题目描述:__

给你一个__下标从 0 开始__且大小为 `m x n` 的整数矩阵 `mat` 和一个整数 `k` 。请你将矩阵中的 __奇数__ 行循环 __右__ 移 `k` 次，__偶数__ 行循环 __左__ 移 `k` 次。

如果初始矩阵和最终矩阵完全相同，则返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

```text
输入：mat = [[1,2,1,2],[5,5,5,5],[6,3,6,3]], k = 2
输出：true
解释：
初始矩阵如图一所示。
图二表示对奇数行右移一次且对偶数行左移一次后的矩阵状态。
图三是经过两次循环移位后的最终矩阵状态，与初始矩阵相同。
因此，返回 true 。
```

![2946-5](https://assets.leetcode.com/uploads/2023/10/29/similarmatrix.png)

示例 2：

```text
输入：mat = [[2,2],[2,2]], k = 3
输出：true
解释：由于矩阵中的所有值都相等，即使进行循环移位，矩阵仍然保持不变。因此，返回 true 。
```

示例 3：

```text
输入：mat = [[1,2]], k = 1
输出：false
解释：循环移位一次后，mat = [[2,1]]，与初始矩阵不相等。因此，返回 false 。
```

__提示：__

- `1 <= mat.length <= 25`
- `1 <= mat[i].length <= 25`
- `1 <= mat[i][j] <= 25`
- `1 <= k <= 50`

__思路:__

```text
模拟
注意取模
对于奇数行, mat[i][j] 和 mat[i][((j - k) % n + n) % n] 必须相等
对于偶数行, mat[i][j] 和 mat[i][(j + k) % n] 必须相等
时间复杂度为 O(MN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool areSimilar(vector<vector<int>>& mat, int k) 
    {
        for (int m = mat.size(), n = mat.front().size(), i = 0; i < m; i += 2) for (int j = 0; j < n; j++) if (mat[i][j] != mat[i][(j + k) % n]) return false;
        for (int m = mat.size(), n = mat.front().size(), i = 1; i < m; i += 2) for (int j = 0; j < n; j++) if (mat[i][j] != mat[i][((j - k) % n + n) % n]) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean areSimilar(int[][] mat, int k) {
        for (int m = mat.length, n = mat[0].length, i = 0; i < m; i += 2) for (int j = 0; j < n; j++) if (mat[i][j] != mat[i][(j + k) % n]) return false;
        for (int m = mat.length, n = mat[0].length, i = 1; i < m; i += 2) for (int j = 0; j < n; j++) if (mat[i][j] != mat[i][((j - k) % n + n) % n]) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def areSimilar(self, mat: List[List[int]], k: int) -> bool:
        return all(row[k % len(mat[0]):] + row[:k % len(mat[0])] == row for row in mat[::2]) and all(row[-k % len(mat[0]):] + row[:-k % len(mat[0])] == row for row in mat[1::2])
```
