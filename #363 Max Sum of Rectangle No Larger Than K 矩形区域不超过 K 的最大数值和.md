__Description__:

Given a non-empty 2D matrix matrix and an integer k, find the max sum of a rectangle in the matrix such that its sum is no larger than k.

__Example:__

Input: matrix = [[1,0,1],[0,-2,3]], k = 2
Output: 2 
Explanation: Because the sum of rectangle [[0, 1], [-2, 3]] is 2,
             and 2 is the max number no larger than k (k = 2).

__Note:__

The rectangle inside the matrix must have an area > 0.
What if the number of rows is much larger than the number of columns?

__题目描述__:
给定一个非空二维矩阵 matrix 和一个整数 k，找到这个矩阵内部不大于 k 的最大矩形和。

__示例 :__

输入: matrix = [[1,0,1],[0,-2,3]], k = 2
输出: 2 
解释: 矩形区域 [[0, 1], [-2, 3]] 的数值和是 2，且 2 是不超过 k 的最大数字（k = 2）。

__说明:__

矩阵内的矩形区域面积必须大于 0。
如果行数远大于列数，你将如何解答呢？

__思路__:
前缀和➕二分
参考[LeetCode #304 Range Sum Query 2D - Immutable 二维区域和检索 - 矩阵不可变](https://www.jianshu.com/p/698d4825c808), 使用二维数组的前缀和
由于这里的行远远大于列, 使用列作为外层循环
固定一列, 对每一行的值进行二分插入, 找到最接近 k的 result就更新
本来应该寻找前缀和 s[j] - s[i] <= k, 转化成二分搜索 s[j] - k
时间复杂度O(mlgmn ^ 2), 空间复杂度O(mn), 其中 m为 matrix的行数, n为 matrix的列数

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int maxSumSubmatrix(vector<vector<int>>& matrix, int k) 
    {
        int m = matrix.size(), n = matrix[0].size(), result = INT_MIN;
        vector<vector<int>> pre(m + 1, vector<int>(n + 1));
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) pre[i + 1][j + 1] = matrix[i][j] + pre[i + 1][j] + pre[i][j + 1] - pre[i][j];
        auto range_sum = [&](int a, int b, int c, int d, vector<vector<int>> &pre) { return pre[c][d] - pre[c][b] - pre[a][d] + pre[a][b]; };
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n + 1; j++)
        {
            set<int> s{0};
            for (int r = 1; r < m + 1; r++)
            {
                int sum = range_sum(0, i, r, j, pre);
                auto it = s.lower_bound(sum - k);
                if (it != s.end()) result = max(result, sum - *it);
                s.insert(sum);
            }
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int maxSumSubmatrix(int[][] matrix, int k) {
        int m = matrix.length, n = matrix[0].length, pre[][] = new int[matrix.length + 1][matrix[0].length + 1], result = Integer.MIN_VALUE;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) pre[i + 1][j + 1] = pre[i][j + 1] + pre[i + 1][j] + matrix[i][j] - pre[i][j];
        for (int i = 0; i < n; i++) for(int j = i + 1; j < n + 1; j++) {
            TreeSet<Integer> set = new TreeSet<>();
            set.add(0);
            for (int l = 1; l < m + 1; l++) {
                int sum = sumRange(0, i, l, j, pre);
                Integer left = set.ceiling(sum - k);
                if (left != null) result = Math.max(result, sum - left);
                if (result == k) return result;
                set.add(sum);
            }
        }
        return result;
    }
    private int sumRange(int a, int b, int c, int d, int[][] pre) {
        return pre[c][d] - pre[a][d] - pre[c][b] + pre[a][b];
    }
}
```

__Python__:
```Python
class Solution:
    def maxSumSubmatrix(self, matrix: List[List[int]], k: int) -> int:
        row, col, result = len(matrix), len(matrix[0]), float("-inf")
        for left in range(col):
            row_sum = [0] * row
            for right in range(left, col):
                for j in range(row):
                    row_sum[j] += matrix[j][right]
                ordered_list, cur = [0], 0
                for num in row_sum:
                    cur += num
                    loc = bisect.bisect_left(ordered_list, cur - k)
                    if loc < len(ordered_list):
                        result = max(cur - ordered_list[loc], result)
                    bisect.insort(ordered_list, cur)
        return result
```