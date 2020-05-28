__Description__:
Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

__Example:__

Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6

__题目描述__:
给定一个仅包含 0 和 1 的二维二进制矩阵，找出只包含 1 的最大矩形，并返回其面积。

__示例 :__

输入:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
输出: 6

__思路__:
1. 对每一行求[LeetCode #84 Largest Rectangle in Histogram 柱状图中最大的矩形](https://www.jianshu.com/p/154636b3e41b)
时间复杂度O(mn), 空间复杂度O(m)
2. 动态规划
对每一个元素, 向上寻找最高的高度, 将这个高度向左右延伸, 即可求出最大矩形
时间复杂度O(mn), 空间复杂度O(m)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int maximalRectangle(vector<vector<char>>& matrix) 
    {
        if (matrix.empty()) return 0;
        int m = matrix.size(), n = matrix[0].size(), result = 0;
        vector<int> left(n), right(n, n), height(n);
        for (int i = 0; i < m; i++) 
        {
            int cur_left = 0, cur_right = n;
            for (int j = 0; j < n; j++) 
            {
                if (matrix[i][j] == '1') ++height[j];
                else height[j] = 0;
            }
            for (int j = 0; j < n; j++) 
            {
                if (matrix[i][j]=='1') left[j] = max(left[j], cur_left);
                else 
                {
                    left[j] = 0; 
                    cur_left = j + 1;
                }
            }
            for (int j = n - 1; j > -1; j--) 
            {
                if (matrix[i][j] == '1') right[j] = min(right[j], cur_right);
                else 
                {
                    right[j] = n; 
                    cur_right = j;
                }    
            }
            for (int j = 0; j < n; j++) result = max(result, (right[j] - left[j]) * height[j]);
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        if (matrix.length == 0) return 0;
        int result = 0, heights[] = new int[matrix[0].length];
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) heights[j] = matrix[i][j] == '1' ? heights[j] + 1 : 0;
            result = Math.max(result, largestRectangleArea(heights));
        }
        return result;
    }
    
    private int largestRectangleArea(int[] heights) {
        int result = 0;
        Stack<Integer> stack = new Stack<>();
        int temp[] = new int[heights.length + 1];
        for (int i = 0; i < heights.length; i++) temp[i] = heights[i];
        for (int i = 0; i < temp.length; i++) {
            while (!stack.empty() && temp[stack.peek()] > temp[i]) {
                int cur = stack.pop();
                result = Math.max(result, (stack.empty() ? i : (i - stack.peek() - 1)) * temp[cur]);
            }
            stack.push(i);
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        if not matrix: 
            return 0
        result, dp = 0, [0] * len(matrix[0])
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                dp[j] = dp[j] + 1 if matrix[i][j] == '1' else 0
            result = max(result, self.largestRectangleArea(dp))
        return result

    def largestRectangleArea(self, heights: List[int]) -> int:
        heights += [0]
        stack, result = [], 0
        for i in range(len(heights)):
            while stack and heights[stack[-1]] > heights[i]:
                cur = stack.pop()
                result = max(result, (i - stack[-1] - 1 if stack else i) * heights[cur])
            stack.append(i)
        return result
```