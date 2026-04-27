# 1739 Building Boxes 放置盒子

__Description:__

You have a cubic storeroom where the width, length, and height of the room are all equal to `n` units. You are asked to place `n` boxes in this room where each box is a cube of unit side length. There are however some rules to placing the boxes:

- You can place the boxes anywhere on the floor.
- If box `x` is placed on top of the box `y`, then each side of the four vertical sides of the box `y` __must__ either be adjacent to another box or to a wall.

Given an integer `n`, return _the __minimum__ possible number of boxes touching the floor._

__Example:__

Example 1:

![1739-1](https://assets.leetcode.com/uploads/2021/01/04/3-boxes.png)

```text
Input: n = 3
Output: 3
Explanation: The figure above is for the placement of the three boxes.
These boxes are placed in the corner of the room, where the corner is on the left side.
```

Example 2:

![1739-2](https://assets.leetcode.com/uploads/2021/01/04/4-boxes.png)

```text
Input: n = 4
Output: 3
Explanation: The figure above is for the placement of the four boxes.
These boxes are placed in the corner of the room, where the corner is on the left side.
```

Example 3:

![1739-3](https://assets.leetcode.com/uploads/2021/01/04/10-boxes.png)

```text
Input: n = 10
Output: 6
Explanation: The figure above is for the placement of the ten boxes.
These boxes are placed in the corner of the room, where the corner is on the back side.
```

__Constraints:__

- `1 <= n <= 10 ^ 9`

__题目描述:__

有一个立方体房间，其长度、宽度和高度都等于 `n` 个单位。请你在房间里放置 `n` 个盒子，每个盒子都是一个单位边长的立方体。放置规则如下:

- 你可以把盒子放在地板上的任何地方。
- 如果盒子 `x` 需要放置在盒子 `y` 的顶部，那么盒子 `y` 竖直的四个侧面都 __必须__ 与另一个盒子或墙相邻。

给你一个整数 `n` ，返回接触地面的盒子的 __最少__ 可能数量。

__示例:__

示例 1：

![1739-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/24/3-boxes.png)

```text
输入：n = 3
输出：3
解释：上图是 3 个盒子的摆放位置。
这些盒子放在房间的一角，对应左侧位置。
```

示例 2：

![1739-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/24/4-boxes.png)

```text
输入：n = 4
输出：3
解释：上图是 3 个盒子的摆放位置。
这些盒子放在房间的一角，对应左侧位置。
```

示例 3：

![1739-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/24/10-boxes.png)

```text
输入：n = 10
输出：6
解释：上图是 10 个盒子的摆放位置。
这些盒子放在房间的一角，对应后方位置。
```

__提示：__

- `1 <= n <= 10 ^ 9`

__思路:__

```text
数学
从上往下
对于第一层至少需要 1 个盒子
对于第二层至少需要 3 个盒子, 实际上是 1 + 2
对于第 level 层至少需要 level(level + 1) / 2 个盒子
除去最后一层的数量都是固定的, 先减去这些盒子数剩下的就是最后一层的盒子数
前 level 层的箱子总数为 level(level + 1)(level + 2) / 6
可以用解方程的方式求出 level 的值
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumBoxes(int n) 
    {
        int result = 0, level = 1;
        while (n > 0) 
        {
            for (int i = 0; i < level; i++) 
            {
                n -= (i + 1);
                ++result;
                if (n < 1) return result;
            }
            ++level;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumBoxes(int n) {
        int result = 0, level = 1;
        while (n > 0) {
            for (int i = 0; i < level; i++) {
                n -= (i + 1);
                ++result;
                if (n < 1) return result;
            }
            ++level;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumBoxes(self, n: int) -> int:
        return ((k := bisect_left(range(ceil(n ** .5)), 1, key=lambda x: x * (x + 1) * (x + 2) // 6 > n)) * (k - 1) >> 1) + ceil(((8 * (n - k * (k - 1) * (k + 1) // 6)) ** .5 - 1) / 2)
```
