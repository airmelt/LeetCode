# 120 Triangle 三角形最小路径和

__Description__:
Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

__Example:__

For example, given the following triangle

```text
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```

The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).

__Note:__

Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.

__题目描述__:
给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。

相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。

__示例 :__

例如，给定三角形：

```text
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```

自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。

__说明：__

如果你可以只使用 O(n) 的额外空间（n 为三角形的总行数）来解决这个问题，那么你的算法会很加分。

__思路__:

从三角形的底部向上遍历
dp[i - 1][j] = min(dp[i][j], dp[i][j + 1])
每一步选择最小的路径直到顶部
时间复杂度O(n ^ 2), 空间复杂度O(1), n为三角形的层数

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minimumTotal(vector<vector<int>>& triangle) 
    {
        for (int i = triangle.size() - 1; i > -1; i--) for (int j = 0; j < i; j++) triangle[i - 1][j] += min(triangle[i][j], triangle[i][j + 1]);
        return triangle.front().front();
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        for (int i = triangle.size() - 1; i > -1; i--) for (int j = 0; j < i; j++) triangle.get(i - 1).set(j, triangle.get(i - 1).get(j) + Math.min(triangle.get(i).get(j), triangle.get(i).get(j + 1)));
        return triangle.get(0).get(0);
    }
}
```

__Python__:

```Python
class Solution:
    def minimumTotal(self, triangle: List[List[int]]) -> int:
        for i in range(len(triangle) - 1, 0, -1):
            for j in range(i):
                triangle[i - 1][j] += min(triangle[i][j], triangle[i][j + 1])
        return triangle[0][0]
```
