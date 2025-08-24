# 2661 First Completely Painted Row or Column 找出叠涂元素

__Description:__

You are given a __0-indexed__ integer array `arr`, and an `m x n` integer __matrix__ `mat`. `arr` and `mat` both contain __all__ the integers in the range `[1, m * n]`.

Go through each index `i` in `arr` starting from index `0` and paint the cell in `mat` containing the integer `arr[i]`.

Return _the smallest index_ `i` _at which either a row or a column will be completely painted in_ `mat`.

__Example:__

Example 1:

![2661-1](https://assets.leetcode.com/uploads/2023/01/18/grid1.jpg)

```text
Input: arr = [1,3,4,2], mat = [[1,4],[2,3]]
Output: 2
Explanation: The moves are shown in order, and both the first row and second column of the matrix become fully painted at arr[2].
```

Example 2:

![2661-2](https://assets.leetcode.com/uploads/2023/01/18/grid2.jpg)

```text
Input: arr = [2,8,7,4,1,3,5,6,9], mat = [[3,2,5],[1,4,6],[8,7,9]]
Output: 3
Explanation: The second column becomes fully painted at arr[3].
```

__Constraints:__

- `m == mat.length`
- `n = mat[i].length`
- `arr.length == m * n`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `1 <= arr[i], mat[r][c] <= m * n`
- All the integers of `arr` are __unique__.
- All the integers of `mat` are __unique__.

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `arr` 和一个 `m x n` 的整数 __矩阵__ `mat` 。 `arr` 和 `mat` 都包含范围 `[1，m * n]` 内的 __所有__ 整数。

从下标 `0` 开始遍历 `arr` 中的每个下标 `i` ，并将包含整数 `arr[i]` 的 `mat` 单元格涂色。

请你找出 `arr` 中第一个使得 `mat` 的某一行或某一列都被涂色的元素，并返回其下标 `i` 。

__示例:__

示例 1：

![2661-3](https://assets.leetcode.com/uploads/2023/01/18/grid1.jpg)

```text
输入：arr = [1,3,4,2], mat = [[1,4],[2,3]]
输出：2
解释：遍历如上图所示，arr[2] 在矩阵中的第一行或第二列上都被涂色。
```

示例 2：

![2661-4](https://assets.leetcode.com/uploads/2023/01/18/grid2.jpg)

```text
输入：arr = [2,8,7,4,1,3,5,6,9], mat = [[3,2,5],[1,4,6],[8,7,9]]
输出：3
解释：遍历如上图所示，arr[3] 在矩阵中的第二列上都被涂色。
```

__提示：__

- `m == mat.length`
- `n = mat[i].length`
- `arr.length == m * n`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `1 <= arr[i], mat[r][c] <= m * n`
- `arr` 中的所有整数 __互不相同__
- `mat` 中的所有整数 __互不相同__

__思路:__

```text
模拟
先用哈希表记录每个数字在矩阵中的位置
idx[mat[i][j]] = [i, j]
分别用两个数组 row 和 col 记录每一行和每一列的涂色次数
如果 row[idx[arr[i]].first] == n 或 col[idx[arr[i]].second] == m，则返回 i
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int firstCompleteIndex(vector<int>& arr, vector<vector<int>>& mat) 
    {
        unordered_map<int, pair<int, int>> idx;
        int m = mat.size(), n = mat.front().size(), total = m * n;
        vector<int> row(m), col(n);
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) idx[mat[i][j]] = { i, j };
        for (int i = 0; i < total; i++) 
        {
            if (++row[idx[arr[i]].first] == n) return i;
            if (++col[idx[arr[i]].second] == m) return i;
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int firstCompleteIndex(int[] arr, int[][] mat) {
        var idx = new HashMap<Integer, int[]>();
        int m = mat.length, n = mat[0].length, total = m * n, row[] = new int[m], col[] = new int[n];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) idx.put(mat[i][j], new int[]{ i, j });
        for (int i = 0; i < total; i++) {
            if (++row[idx.get(arr[i])[0]] == n) return i;
            if (++col[idx.get(arr[i])[1]] == m) return i;
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def firstCompleteIndex(self, arr: List[int], mat: List[List[int]]) -> int:
        return min(min([max(d[mat[i][j]] for j in range(len(mat[0]))) for i in range(len(mat))]), min([max(d[mat[i][j]] for i in range(len(mat))) for j in range(len(mat[0]))])) if (d := {arr[i]: i for i in range(len(mat) * len(mat[0]))}) else -1
```
