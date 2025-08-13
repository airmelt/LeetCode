# 2643 Row With Maximum Ones 一最多的行

__Description:__

Given a `m x n` binary matrix `mat`, find the __0-indexed__ position of the row that contains the __maximum__ count of __ones,__ and the number of ones in that row.

In case there are multiple rows that have the maximum count of ones, the row with the smallest row number should be selected.

Return an array containing the index of the row, and the number of ones in it.

__Example:__

Example 1:

```text
Input:  mat = [[0,1],[1,0]]
Output:  [0,1]
Explanation:  Both rows have the same number of 1's. So we return the index of the smaller row, 0, and the maximum count of ones (1). So, the answer is [0,1].
```

Example 2:

```text
Input:  mat = [[0,0,0],[0,1,1]]
Output:  [1,2]
Explanation:  The row indexed 1 has the maximum count of ones (2). So we return its index, 1, and the count. So, the answer is [1,2].
```

Example 3:

```text
Input: mat = [[0,0],[1,1],[0,0]]
Output: [1,2]
Explanation: The row indexed 1 has the maximum count of ones (2). So the answer is [1,2].
```

__Constraints:__

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 100`
- `mat[i][j]` is either `0` or `1`.

__题目描述:__

给你一个大小为 `m x n` 的二进制矩阵 `mat` ，请你找出包含最多 __1__ 的行的下标（从 __0__ 开始）以及这一行中 __1__ 的数目。

如果有多行包含最多的 1 ，只需要选择 行下标最小 的那一行。

返回一个由行下标和该行中 1 的数量组成的数组。

__示例:__

示例 1：

```text
输入：mat = [[0,1],[1,0]]
输出：[0,1]
解释：两行中 1 的数量相同。所以返回下标最小的行，下标为 0 。该行 1 的数量为 1 。所以，答案为 [0,1] 。
```

示例 2：

```text
输入: mat = [[0,0,0],[0,1,1]]
输出: [1,2]
解释: 下标为 1 的行中 1 的数量最多 。该行 1 的数量为 2 。所以，答案为 [1,2] 。
```

示例 3：

```text
输入: mat = [[0,0],[1,1],[0,0]]
输出: [1,2]
解释: 下标为 1 的行中 1 的数量最多。该行 1 的数量为 2 。所以，答案为 [1,2] 。
```

__提示：__

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 100`
- `mat[i][j]` 为 `0` 或 `1`

__思路:__

```text
模拟
累计每一行的 1 的个数
只有当最大值更新时才更新结果
时间复杂度为 O(MN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> rowAndMaximumOnes(vector<vector<int>>& mat) 
    {
        int mx = 0, row = 0, n = mat.size(), total = 0;
        for (int i = 0; i < n; i++) {
            if ((total = accumulate(mat[i].begin(), mat[i].end(), 0)) > mx) 
            {
                mx = total;
                row = i;
            }
        }
        return { row, mx };
    }
};
```

__Java__:

```Java
class Solution {
    public int[] rowAndMaximumOnes(int[][] mat) {
        int mx = 0, row = 0, n = mat.length, total = 0;
        for (int i = 0; i < n; i++) {
            if ((total = (int)Arrays.stream(mat[i]).sum()) > mx) {
                mx = total;
                row = i;
            }
        }
        return new int[]{ row, mx };
    }
}
```

__Python__:

```Python
class Solution:
    def rowAndMaximumOnes(self, mat: List[List[int]]) -> List[int]:
        return [sorted_data[0][0], sum(sorted_data[0][1])] if (sorted_data := sorted(enumerate(mat), key=lambda x: sum(x[1]), reverse=True)) else [0, 0]
```
