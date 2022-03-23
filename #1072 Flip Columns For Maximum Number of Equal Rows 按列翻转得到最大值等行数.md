# 1072 Flip Columns For Maximum Number of Equal Rows 按列翻转得到最大值等行数

__Description__:
You are given an m x n binary matrix matrix.

You can choose any number of columns in the matrix and flip every cell in that column (i.e., Change the value of the cell from 0 to 1 or vice versa).

Return the maximum number of rows that have all values equal after some number of flips.

__Example:__

Example 1:

Input: matrix = [[0,1],[1,1]]
Output: 1
Explanation: After flipping no values, 1 row has all values equal.

Example 2:

Input: matrix = [[0,1],[1,0]]
Output: 2
Explanation: After flipping values in the first column, both rows have equal values.

Example 3:

Input: matrix = [[0,0,0],[0,0,1],[1,1,0]]
Output: 2
Explanation: After flipping values in the first two columns, the last two rows have equal values.

__Constraints:__

m == matrix.length
n == matrix[i].length
1 <= m, n <= 300
matrix[i][j] is either 0 or 1.

__题目描述__:
给定 m x n 矩阵 matrix 。

你可以从中选出任意数量的列并翻转其上的 每个 单元格。（即翻转后，单元格的值从 0 变成 1，或者从 1 变为 0 。）

返回 经过一些翻转后，行与行之间所有值都相等的最大行数 。

__示例 :__

示例 1：

输入：matrix = [[0,1],[1,1]]
输出：1
解释：不进行翻转，有 1 行所有值都相等。
示例 2：

输入：matrix = [[0,1],[1,0]]
输出：2
解释：翻转第一列的值之后，这两行都由相等的值组成。

示例 3：

输入：matrix = [[0,0,0],[0,0,1],[1,1,0]]
输出：2
解释：翻转前两列的值之后，后两行由相等的值组成。

__提示:__

m == matrix.length
n == matrix[i].length
1 <= m, n <= 300
matrix[i][j] == 0 或 1

__思路__:

模拟
两行完全相同或者完全相反经过列的转换就能相同
只要找到模式行的最大值
时间复杂度O(mn), 空间复杂度O(m)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxEqualRowsAfterFlips(vector<vector<int>>& matrix) 
    {
        unordered_map<string, int> m;
        int result = 0;
        for (const auto& x : matrix) 
        {
            string a = "", b = "";
            for (const auto& y : x) 
            {
                a += to_string(y);
                b += to_string(1 ^ y);
            }
            ++m[a];
            ++m[b];
            result = max({result, m[b], m[a]});
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxEqualRowsAfterFlips(int[][] matrix) {
        Map<String, Integer> map = new HashMap<>();
        int result = 0;
        for (int[] x : matrix) {
            String a = "", b = "";
            for (int y : x) {
                a += y;
                b += (1 ^ y);
            }
            map.put(a, map.getOrDefault(a, 0) + 1);
            map.put(b, map.getOrDefault(b, 0) + 1);
            result = Math.max(result, Math.max(map.get(a), map.get(b)));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxEqualRowsAfterFlips(self, matrix: List[List[int]]) -> int:
        m, n, count = len(matrix), len(matrix[0]), defaultdict(int)
        for i in range(m):
            count[tuple(matrix[i]) if not matrix[i][0] else tuple([1 - j for j in matrix[i]])] += 1
        return max(count.values())
```
