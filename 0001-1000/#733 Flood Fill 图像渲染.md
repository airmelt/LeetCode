# 733 Flood Fill 图像渲染

__Description__:
An image is represented by a 2-D array of integers, each integer representing the pixel value of the image (from 0 to 65535).

Given a coordinate (sr, sc) representing the starting pixel (row and column) of the flood fill, and a pixel value newColor, "flood fill" the image.

To perform a "flood fill", consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color as the starting pixel), and so on. Replace the color of all of the aforementioned pixels with the newColor.

At the end, return the modified image.

__Example:__

Example 1:

Input:
image = [[1,1,1],[1,1,0],[1,0,1]]
sr = 1, sc = 1, newColor = 2
Output: [[2,2,2],[2,2,0],[2,0,1]]
Explanation:
From the center of the image (with position (sr, sc) = (1, 1)), all pixels connected
by a path of the same color as the starting pixel are colored with the new color.
Note the bottom corner is not colored 2, because it is not 4-directionally connected
to the starting pixel.

__Note:__

The length of image and image[0] will be in the range [1, 50].
The given starting pixel will satisfy 0 <= sr < image.length and 0 <= sc < image[0].length.
The value of each color in image[i][j] and newColor will be an integer in [0, 65535].

__题目描述__:
有一幅以二维整数数组表示的图画，每一个整数表示该图画的像素值大小，数值在 0 到 65535 之间。

给你一个坐标 (sr, sc) 表示图像渲染开始的像素值（行 ，列）和一个新的颜色值 newColor，让你重新上色这幅图像。

为了完成上色工作，从初始坐标开始，记录初始坐标的上下左右四个方向上像素值与初始坐标相同的相连像素点，接着再记录这四个方向上符合条件的像素点与他们对应四个方向上像素值与初始坐标相同的相连像素点，……，重复该过程。将所有有记录的像素点的颜色值改为新的颜色值。

最后返回经过上色渲染后的图像。

__示例 :__

示例 1:

输入:
image = [[1,1,1],[1,1,0],[1,0,1]]
sr = 1, sc = 1, newColor = 2
输出: [[2,2,2],[2,2,0],[2,0,1]]
解析:
在图像的正中间，(坐标(sr,sc)=(1,1)),
在路径上所有符合条件的像素点的颜色都被更改成2。
注意，右下角的像素没有更改为2，
因为它不是在上下左右四个方向上与初始点相连的像素点。

__注意：__

image 和 image[0] 的长度在范围 [1, 50] 内。
给出的初始点将满足 0 <= sr < image.length 和 0 <= sc < image[0].length。
image[i][j] 和 newColor 表示的颜色值在范围 [0, 65535]内。

__思路__:

可以用深度/广度优先搜索

1. 递归, 分别取上下左右且在数组中的元素递归修改颜色值
2. 迭代, 用队列/栈记录点
时间复杂度O(n ^ 2), 空间复杂度O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) 
    {
        if (newColor == image[sr][sc]) return image;
        int directions[2][4] = {{-1, 0, 1, 0}, {0, -1, 0, 1}}, old = image[sr][sc], row = image.size(), col = image[0].size();
        queue<pair<int, int>> q;
        q.push({sr, sc});
        while (q.size()) 
        {
            pair<int, int> cur = q.front();
            q.pop();
            image[cur.first][cur.second] = newColor;
            for (int index = 0; index < 4; index++) 
            {
                int i = directions[0][index] + cur.first, j = directions[1][index] + cur.second;
                if ((-1 < i and i < row) and (-1 < j and j < col) and (image[i][j] == old)) q.push({i, j});
            }
        }
        return image;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        if (image[sr][sc] != newColor) {
            int old = image[sr][sc], row = image.length, col = image[0].length, directions[][] = {{-1, 0, 1, 0}, {0, -1, 0, 1}};
            image[sr][sc] = newColor;
            for (int index = 0; index < 4; index++) {
                int i = directions[0][index] + sr, j = directions[1][index] + sc;
                if ((-1 < i && i < row) && (-1 < j && j < col) && (image[i][j] == old)) floodFill(image, i, j, newColor);
            }
        }
        return image;
    }
}
```

__Python__:

```Python
class Solution:
    def floodFill(self, image: List[List[int]], sr: int, sc: int, newColor: int) -> List[List[int]]:
        if image[sr][sc] != newColor:
            old, image[sr][sc], r, c = image[sr][sc], newColor, len(image), len(image[0])
            for i, j in zip((sr, sr + 1, sr, sr - 1), (sc + 1, sc, sc - 1, sc)):
                if -1 < i < r and -1 < j < c and image[i][j] == old:
                    self.floodFill(image, i, j, newColor)
        return image
```
