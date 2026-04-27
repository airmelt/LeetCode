# 3000 Maximum Area of Longest Diagonal Rectangle 对角线最长的矩形的面积

__Description:__

You are given a 2D __0-indexed__ integer array `dimensions`.

For all indices `i`, `0 <= i < dimensions.length`, `dimensions[i][0]` represents the length and `dimensions[i][1]` represents the width of the rectangle `i`.

Return the area of the rectangle having the longest diagonal. If there are multiple rectangles with the longest diagonal, return the area of the rectangle having the maximum area.

__Example:__

Example 1:

```text
Input: dimensions = [[9,3],[8,6]]
Output: 48
Explanation: 
For index = 0, length = 9 and width = 3. Diagonal length = sqrt(9 * 9 + 3 * 3) = sqrt(90) ≈ 9.487.
For index = 1, length = 8 and width = 6. Diagonal length = sqrt(8 * 8 + 6 * 6) = sqrt(100) = 10.
So, the rectangle at index 1 has a greater diagonal length therefore we return area = 8 * 6 = 48.
```

Example 2:

```text
Input: dimensions = [[3,4],[4,3]]
Output: 12
Explanation: Length of diagonal is the same for both which is 5, so maximum area = 12.
```

__Constraints:__

- `1 <= dimensions.length <= 100`
- `dimensions[i].length == 2`
- `1 <= dimensions[i][0], dimensions[i][1] <= 100`

__题目描述:__

给你一个下标从 __0__ 开始的二维整数数组 `dimensions`。

对于所有下标 `i`（ `0 <= i < dimensions.length`）， `dimensions[i][0]` 表示矩形  `i` 的长度，而 `dimensions[i][1]` 表示矩形  `i` 的宽度。

返回对角线最 长 的矩形的 面积 。如果存在多个对角线长度相同的矩形，返回其中面积最 大 的矩形的面积。

__示例:__

示例 1：

```text
输入：dimensions = [[9,3],[8,6]]
输出：48
解释：
下标 = 0，长度 = 9，宽度 = 3。对角线长度 = sqrt(9 * 9 + 3 * 3) = sqrt(90) ≈ 9.487。
下标 = 1，长度 = 8，宽度 = 6。对角线长度 = sqrt(8 * 8 + 6 * 6) = sqrt(100) = 10。
因此，下标为 1 的矩形对角线更长，所以返回面积 = 8 * 6 = 48。
```

示例 2：

```text
输入：dimensions = [[3,4],[4,3]]
输出：12
解释：两个矩形的对角线长度相同，为 5，所以最大面积 = 12。
```

__提示：__

- `1 <= dimensions.length <= 100`
- `dimensions[i].length == 2`
- `1 <= dimensions[i][0], dimensions[i][1] <= 100`

__思路:__

```text
模拟
计算出对角线的平方
如果对角线更长或者面积更大就更新结果
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int areaOfMaxDiagonal(vector<vector<int>>& dimensions) 
    {
        int result = 0, mx = 0;
        for (const auto& d : dimensions) 
        {
            if (d.front() * d.front() + d.back() * d.back() > mx || d.front() * d.front() + d.back() * d.back() == mx && d.front() * d.back() > result) 
            {
                mx = d.front() * d.front() + d.back() * d.back();
                result = d.front() * d.back();
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int areaOfMaxDiagonal(int[][] dimensions) {
        int result = 0, mx = 0;
        for (var d : dimensions) {
            if (d[0] * d[0] + d[1] * d[1] > mx || d[0] * d[0] + d[1] * d[1] == mx && d[0] * d[1] > result) {
                mx = d[0] * d[0] + d[1] * d[1];
                result = d[0] * d[1];
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def areaOfMaxDiagonal(self, dimensions: List[List[int]]) -> int:
        return max((x * x + y * y, x * y) for x, y in dimensions)[1]
```
