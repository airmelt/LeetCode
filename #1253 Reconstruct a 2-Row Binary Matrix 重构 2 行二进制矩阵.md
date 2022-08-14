# 1253 Reconstruct a 2-Row Binary Matrix 重构 2 行二进制矩阵

__Description:__

Given the following details of a matrix with n columns and 2 rows :

The matrix is a binary matrix, which means each element in the matrix can be 0 or 1.
The sum of elements of the 0-th(upper) row is given as upper.
The sum of elements of the 1-st(lower) row is given as lower.
The sum of elements in the i-th column(0-indexed) is colsum[i], where colsum is given as an integer array with length n.
Your task is to reconstruct the matrix with upper, lower and colsum.

Return it as a 2-D integer array.

If there are more than one valid solution, any of them will be accepted.

If no valid solution exists, return an empty 2-D array.

__Example:__

Example 1:

Input: upper = 2, lower = 1, colsum = [1,1,1]
Output: [[1,1,0],[0,0,1]]
Explanation: [[1,0,1],[0,1,0]], and [[0,1,1],[1,0,0]] are also correct answers.

Example 2:

Input: upper = 2, lower = 3, colsum = [2,2,1,1]
Output: []

Example 3:

Input: upper = 5, lower = 5, colsum = [2,1,2,0,1,0,1,2,0,1]
Output: [[1,1,1,0,1,0,0,1,0,0],[1,0,1,0,0,0,1,1,0,1]]

__Constraints:__

1 <= colsum.length <= 10^5
0 <= upper, lower <= colsum.length
0 <= colsum[i] <= 2

__题目描述：__

给你一个 2 行 n 列的二进制数组：

矩阵是一个二进制矩阵，这意味着矩阵中的每个元素不是 0 就是 1。
第 0 行的元素之和为 upper。
第 1 行的元素之和为 lower。
第 i 列（从 0 开始编号）的元素之和为 colsum[i]，colsum 是一个长度为 n 的整数数组。
你需要利用 upper，lower 和 colsum 来重构这个矩阵，并以二维整数数组的形式返回它。

如果有多个不同的答案，那么任意一个都可以通过本题。

如果不存在符合要求的答案，就请返回一个空的二维数组。

__示例：__

示例 1：

输入：upper = 2, lower = 1, colsum = [1,1,1]
输出：[[1,1,0],[0,0,1]]
解释：[[1,0,1],[0,1,0]] 和 [[0,1,1],[1,0,0]] 也是正确答案。

示例 2：

输入：upper = 2, lower = 3, colsum = [2,2,1,1]
输出：[]

示例 3：

输入：upper = 5, lower = 5, colsum = [2,1,2,0,1,0,1,2,0,1]
输出：[[1,1,1,0,1,0,0,1,0,0],[1,0,1,0,0,0,1,1,0,1]]

__提示：__

1 <= colsum.length <= 10^5
0 <= upper, lower <= colsum.length
0 <= colsum[i] <= 2

__思路：__

模拟
先统计 colsum 数组中 2 出现的次数
2 出现意味着结果数组中对应的位置都必须为 1
如果 upper 和 lower 的和比 colsum 的总和要小, 说明结果数组中 1 的个数不够
如果 upper 和 lower 任何 1 个比 2 出现的次数要少, 说明对应结果数组中 1 的个数不够
以上两种情况都返回空数组
否则是肯定能够构造数组的
当 colsum[i] 为 0 时, 结果数组都置 0
当 colsum[i] 为 1 时, 选择当前数量更多的结果数组置 1, 并将对应的值减 1
当 colsum[i] 为 2 时, 结果数组都置 1, 并将 upper 和 lower 的值减 1
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> reconstructMatrix(int upper, int lower, vector<int>& colsum) 
    {
        int n = colsum.size(), s = accumulate(colsum.begin(), colsum.end(), 0), two = 0;
        for (int i = 0; i < n; i++) two += colsum[i] == 2;
        if (upper + lower != s or upper < two or lower < two) return vector<vector<int>>{};
        vector<vector<int>> result(2, vector<int>(n, 0));
        for (int i = 0; i < n; i++)
        {
            if (!colsum[i]) continue;
            else if (colsum[i] == 2)
            {
                result.front()[i] = result.back()[i] = 1;
                --lower;
                --upper;
            }
            else if (upper and upper > lower)
            {
                result.front()[i] = 1;
                --upper;
            }
            else
            {
                result.back()[i] = 1;
                --lower;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> reconstructMatrix(int upper, int lower, int[] colsum) {
        int n = colsum.length, two = 0, s = 0;
        for (int i = 0; i < n; i++) {
            if (colsum[i] == 2) ++two;
            s += colsum[i];
        }
        if (upper + lower != s || upper < two || lower < two) return new ArrayList<>();
        List<List<Integer>> result = new ArrayList<>(){{
            add(new ArrayList<>());
            add(new ArrayList<>());
        }};
        for (int i = 0; i < n; i++) {
            if (colsum[i] == 2) {
                result.get(0).add(1);
                result.get(1).add(1);
                --upper;
                --lower;
            } else if (colsum[i] == 0) {
                result.get(0).add(0);
                result.get(1).add(0);
            } else if (upper != 0 && upper > lower) {
                result.get(0).add(1);
                result.get(1).add(0);
                --upper;
            } else {
                result.get(0).add(0);
                result.get(1).add(1);
                --lower;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def reconstructMatrix(self, upper: int, lower: int, colsum: List[int]) -> List[List[int]]:
        if upper + lower != sum(colsum) or upper < (two := colsum.count(2)) or lower < two:
            return []
        result = [[0] * (n := len(colsum)), [0] * n]
        for i, num in enumerate(colsum):
            if not num:
                continue
            elif num == 2:
                result[0][i] = result[1][i] = 1
                upper -= 1
                lower -= 1
            elif upper and upper > lower:
                result[0][i] = 1
                upper -= 1
            else:
                result[1][i] = 1
                lower -= 1
        return result
```
