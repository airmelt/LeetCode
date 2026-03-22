# 302 Smallest Rectangle Enclosing Black Pixels 包含全部黑色像素的最小矩形

__Description:__

You are given an `m x n` binary matrix `image` where `0` represents a white pixel and `1` represents a black pixel.

The black pixels are connected (i.e., there is only one black region). Pixels are connected horizontally and vertically.

Given two integers `x` and `y` that represents the location of one of the black pixels, return _the area of the smallest (axis-aligned) rectangle that encloses all black pixels._

You must write an algorithm with less than `O(mn)` runtime complexity

__Example:__

Example 1:

![302-1](https://assets.leetcode.com/uploads/2021/03/14/pixel-grid.jpg)

```text
Input: image = [["0","0","1","0"],["0","1","1","0"],["0","1","0","0"]], x = 0, y = 2
Output: 6
```

Example 2:

```text
Input: image = [["1"]], x = 0, y = 0
Output: 1
```

__Constraints:__

- `m == image.length`
- `n == image[i].length`
- `1 <= m, n <= 100`
- `image[i][j]` is either `'0'` or `'1'`.
- `0 <= x < m`
- `0 <= y < n`
- `image[x][y] == '1'.`
- The black pixels in the `image` only form __one component__.

__题目描述:__

图片在计算机处理中往往是使用二维矩阵来表示的。

给你一个大小为 `m x n` 的二进制矩阵 `image` 表示一张黑白图片，`0` 代表白色像素，`1` 代表黑色像素。

黑色像素相互连接，也就是说，图片中只会有一片连在一块儿的黑色像素。像素点是水平或竖直方向连接的。

给你两个整数 `x` 和 `y` 表示某一个黑色像素的位置，请你找出包含全部黑色像素的最小矩形（与坐标轴对齐），并返回该矩形的面积。

你必须设计并实现一个时间复杂度低于 `O(mn)` 的算法来解决此问题。

__示例:__

示例 1：

![302-2](https://assets.leetcode.com/uploads/2021/03/14/pixel-grid.jpg)

```text
输入：root = [1,null,3,2,4,null,null,null,5]
输出：3
解释：当中，最长连续序列是 3-4-5 ，所以返回结果为 3 。
```

示例 2：

```text
输入：root = [2,null,3,2,null,1]
输出：2
解释：当中，最长连续序列是 2-3 。注意，不是 3-2-1，所以返回 2 。
```

__提示：__

- `m == image.length`
- `n == image[i].length`
- `1 <= m, n <= 100`
- `image[i][j]` 为 `'0'` 或 `'1'`.
- `0 <= x < m`
- `0 <= y < n`
- `image[x][y] == '1'.`
- `image` 中的黑色像素仅形成一个 __组件__.

__思路:__

```text
1. 暴力法
分别从 x, y 出发，向四个方向遍历，找到最小的矩形
需要遍历所有的像素
时间复杂度为 O(MN), 空间复杂度为 O(1)
2. 二分查找
由于黑色区域是连通的，所以行和列分别是一个连续区域
即投影一定是连续的
以找 top 为例
需要从 0 到 x 找到一个行, 该列是一个全白的行
low = 0, high = x
mid 为 (low + high) / 2
如果 mid 行有黑色像素, 则 top <= mid, 区间变为 [low, mid]
否则, top > mid, 区间变为 [mid + 1, high]
找 bottom 正好相反
时间复杂度为 O(MlogN + NlogM), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minArea(vector<vector<char>>& image, int x, int y) 
    {
        return (binary_search(image, y + 1, image.front().size(), 0, image.size(), false, false) - binary_search(image, 0, y, 0, image.size(), true, false)) * (binary_search(image, x + 1, image.size(), 0, image.front().size(), false, true) - binary_search(image, 0, x, 0, image.front().size(), true, true));
    }

private:
    int binary_search(vector<vector<char>>& image, int i, int j, int low, int high, bool flag, bool row) 
    {
        while (i < j) 
        {
            int k = low, mid = (i + j) / 2;
            while (k < high && (row ? image[mid][k] : image[k][mid]) == '0') ++k;
            if (k < high == flag) j = mid;
            else i = mid + 1;
        }
        return i;
    }
};
```

__Java__:

```Java
class Solution {
    public int minArea(char[][] image, int x, int y) {
        int m = image.length, n = image[0].length, top = x, bottom = x, left = y, right = y;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (image[i][j] == '1') {
                    top = Math.min(i, top);
                    bottom = Math.max(i + 1, bottom);
                    left = Math.min(j, left);
                    right = Math.max(j + 1, right);
                }
            }
        }
        return (right - left) * (bottom - top);
    }
}
```

__Python__:

```Python
class Solution:
    def minArea(self, image: List[List[str]], x: int, y: int) -> int:
        return (max((j + 1 for i in range(len(image)) for j in range(len(image[0])) if image[i][j] == '1'), default=y) - min((j for i in range(len(image)) for j in range(len(image[0])) if image[i][j] == '1'), default=y)) * (max((i + 1 for i in range(len(image)) for j in range(len(image[0])) if image[i][j] == '1'), default=x) - min((i for i in range(len(image)) for j in range(len(image[0])) if image[i][j] == '1'), default=x))
```
