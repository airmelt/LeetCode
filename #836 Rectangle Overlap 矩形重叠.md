# 836 Rectangle Overlap 矩形重叠

__Description__:
A rectangle is represented as a list [x1, y1, x2, y2], where (x1, y1) are the coordinates of its bottom-left corner, and (x2, y2) are the coordinates of its top-right corner.

Two rectangles overlap if the area of their intersection is positive.  To be clear, two rectangles that only touch at the corner or edges do not overlap.

Given two (axis-aligned) rectangles, return whether they overlap.

__Example:__

Example 1:

Input: rec1 = [0,0,2,2], rec2 = [1,1,3,3]
Output: true

Example 2:

Input: rec1 = [0,0,1,1], rec2 = [1,0,2,1]
Output: false

__Notes:__

Both rectangles rec1 and rec2 are lists of 4 integers.
All coordinates in rectangles will be between -10^9 and 10^9.

__题目描述__:
矩形以列表 [x1, y1, x2, y2] 的形式表示，其中 (x1, y1) 为左下角的坐标，(x2, y2) 是右上角的坐标。

如果相交的面积为正，则称两矩形重叠。需要明确的是，只在角或边接触的两个矩形不构成重叠。

给出两个矩形，判断它们是否重叠并返回结果。

__示例 :__

示例 1：

输入：rec1 = [0,0,2,2], rec2 = [1,1,3,3]
输出：true

示例 2：

输入：rec1 = [0,0,1,1], rec2 = [1,0,2,1]
输出：false

__说明：__

两个矩形 rec1 和 rec2 都以含有四个整数的列表的形式给出。
矩形中的所有坐标都处于 -10^9 和 10^9 之间。

__思路__:

如果两个矩形不重合, 要么 1在 2的左边(即 1的 x2坐标小于等于 2的 x1坐标)、右边、上侧、下侧
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2) 
    {
        return !(rec1[2] <= rec2[0] or rec1[3] <= rec2[1] or rec1[0] >= rec2[2] or rec1[1] >= rec2[3]);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isRectangleOverlap(int[] rec1, int[] rec2) {
        return !(rec1[2] <= rec2[0] || rec1[3] <= rec2[1] || rec1[0] >= rec2[2] || rec1[1] >= rec2[3]);
    }
}
```

__Python__:

```Python
class Solution:
    def isRectangleOverlap(self, rec1: List[int], rec2: List[int]) -> bool:
        return not (rec1[2] <= rec2[0] or rec1[3] <= rec2[1] or rec1[0] >= rec2[2] or rec1[1] >= rec2[3])
```
