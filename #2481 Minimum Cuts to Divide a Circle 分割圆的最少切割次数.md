# 2481 Minimum Cuts to Divide a Circle 分割圆的最少切割次数

__Description:__

A valid cut in a circle can be:

- A cut that is represented by a straight line that touches two points on the edge of the circle and passes through its center, or
- A cut that is represented by a straight line that touches one point on the edge of the circle and its center.

Some valid and invalid cuts are shown in the figures below.

![2481-1](https://assets.leetcode.com/uploads/2022/10/29/alldrawio.png)

Given the integer `n`, return _the __minimum__ number of cuts needed to divide a circle into_ `n` _equal slices_.

__Example:__

Example 1:

![2481-2](https://assets.leetcode.com/uploads/2022/10/24/11drawio.png)

```text
Input: n = 4
Output: 2
Explanation: 
The above figure shows how cutting the circle twice through the middle divides it into 4 equal slices.
```

Example 2:

![2481-3](https://assets.leetcode.com/uploads/2022/10/24/22drawio.png)

```text
Input: n = 3
Output: 3
Explanation:
At least 3 cuts are needed to divide the circle into 3 equal slices. 
It can be shown that less than 3 cuts cannot result in 3 slices of equal size and shape.
Also note that the first cut will not divide the circle into distinct parts.
```

__Constraints:__

- `1 <= n <= 100`

__题目描述:__

圆内一个 有效切割 ，符合以下二者之一：

- 该切割是两个端点在圆上的线段，且该线段经过圆心。
- 该切割是一端在圆心另一端在圆上的线段。

一些有效和无效的切割如下图所示。

![2481-4](https://assets.leetcode.com/uploads/2022/10/29/alldrawio.png)

给你一个整数 `n` ，请你返回将圆切割成相等的 `n` 等分的 __最少__ 切割次数。

__示例:__

示例 1：

![2481-5](https://assets.leetcode.com/uploads/2022/10/24/11drawio.png)

```text
输入：n = 4
输出：2
解释：
上图展示了切割圆 2 次，得到四等分。
```

示例 2：

![2481-6](https://assets.leetcode.com/uploads/2022/10/24/22drawio.png)

```text
输入：n = 3
输出：3
解释：
最少需要切割 3 次，将圆切成三等分。
少于 3 次切割无法将圆切成大小相等面积相同的 3 等分。
同时可以观察到，第一次切割无法将圆切割开。
```

__提示：__

- `1 <= n <= 100`

__思路:__

```text
数学
因为合法的切割只能通过圆心
所以 n 是偶数时, 切割次数为 n / 2
n 是奇数时, 切割次数为 n
另外 n = 1 时, 切割次数为 0
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfCuts(int n) 
    {
        return n == 1 ? 0 : (n & 1 ? n : n >> 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfCuts(int n) {
        return n == 1 ? 0 : ((n & 1) == 1 ? n : n >> 1);
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfCuts(self, n: int) -> int:
        return 0 if n == 1 else n if n & 1 else n >> 1
```
