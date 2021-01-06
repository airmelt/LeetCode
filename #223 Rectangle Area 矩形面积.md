# 223 Rectangle Area 矩形面积

__Description__:
Find the total area covered by two rectilinear rectangles in a 2D plane.

Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.

__Example:__

![rectangle area](https://upload-images.jianshu.io/upload_images/16639143-40972ec071dfefb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Input: A = -3, B = 0, C = 3, D = 4, E = 0, F = -1, G = 9, H = 2
Output: 45

__Note:__

Assume that the total area is never beyond the maximum possible value of int.

__题目描述__:
在二维平面上计算出两个由直线构成的矩形重叠后形成的总面积。

每个矩形由其左下顶点和右上顶点坐标表示，如图所示。

__示例 :__

![矩形](https://upload-images.jianshu.io/upload_images/16639143-40972ec071dfefb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入: -3, 0, 3, 4, 0, -1, 9, 2
输出: 45

__说明：__
假设矩形面积不会超出 int 的范围。

__思路__:

两个矩形面积之和减去两个矩形面积重叠的部分
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) 
    {
        return (C - A) * (D - B) - (min(C, G) > max(A, E) ? min(C, G) - max(A, E) : 0) * (min(D, H) > max(B, F) ? min(D, H) - max(B, F) : 0) + (G - E) * (H - F);
    }
};
```

__Java__:

```Java
class Solution {
    public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        return (C - A) * (D - B) - (Math.min(C, G) > Math.max(A, E) ? Math.min(C, G) - Math.max(A, E) : 0) * (Math.min(D, H) > Math.max(B, F) ? Math.min(D, H) - Math.max(B, F) : 0) + (G - E) * (H - F);
    }
}
```

__Python__:

```Python
class Solution:
    def computeArea(self, A: int, B: int, C: int, D: int, E: int, F: int, G: int, H: int) -> int:
        return (C - A) * (D - B) + (G - E) * (H - F) - max(0, min(C, G) - max(A, E)) * max(0, min(D, H) - max(B, F))
```
