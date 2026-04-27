# 1954 Minimum Garden Perimeter to Collect Enough Apples 收集足够苹果的最小花园周长

__Description:__

In a garden represented as an infinite 2D grid, there is an apple tree planted at __every__ integer coordinate. The apple tree planted at an integer coordinate `(i, j)` has `|i| + |j|` apples growing on it.

You will buy an axis-aligned __square plot__ of land that is centered at `(0, 0)`.

Given an integer `neededApples`, return _the __minimum perimeter__ of a plot such that __at least___ `neededApples` _apples are __inside or on__ the perimeter of that plot_.

The value of `|x|` is defined as:

- `x` if `x >= 0`
- `-x` if `x < 0`

__Example:__

Example 1:

![1954-1](https://assets.leetcode.com/uploads/2019/08/30/1527_example_1_2.png)

```text
Input: neededApples = 1
Output: 8
Explanation: A square plot of side length 1 does not contain any apples.
However, a square plot of side length 2 has 12 apples inside (as depicted in the image above).
The perimeter is 2 * 4 = 8.
```

Example 2:

```text
Input: neededApples = 13
Output: 16
```

Example 3:

```text
Input: neededApples = 1000000000
Output: 5040
```

__Constraints:__

- `1 <= neededApples <= 10 ^ 15`

__题目描述:__

给你一个用无限二维网格表示的花园，__每一个__ 整数坐标处都有一棵苹果树。整数坐标 `(i, j)` 处的苹果树有 `|i| + |j|` 个苹果。

你将会买下正中心坐标是 `(0, 0)` 的一块 __正方形土地__ ，且每条边都与两条坐标轴之一平行。

给你一个整数 `neededApples` ，请你返回土地的 __最小周长__ ，使得 __至少__ 有 `neededApples` 个苹果在土地 __里面或者边缘上__。

`|x|` 的值定义为:

- 如果 `x >= 0` ，那么值为 `x`
- 如果 `x < 0` ，那么值为 `-x`

__示例:__

示例 1：

![1954-2](https://pic.leetcode-cn.com/1627790803-qcBKFw-image.png)

```text
输入：neededApples = 1
输出：8
解释：边长长度为 1 的正方形不包含任何苹果。
但是边长为 2 的正方形包含 12 个苹果（如上图所示）。
周长为 2 * 4 = 8 。
```

示例 2：

```text
输入：neededApples = 13
输出：16
```

示例 3：

```text
输入：neededApples = 1000000000
输出：5040
```

__提示：__

- `1 <= neededApples <= 10 ^ 15`

__思路:__

```text
数学
先计算几个看一下规律
设边长为 n
n = 1 时, 苹果为 0
n = 2 时, 苹果为 12
n = 3 时, 苹果为 48
苹果数为 12 * n ^ 2
总数为 2 * n * (n + 1) * (2 * n + 1)
即解方程 2 * n * (n + 1) * (2 * n + 1) >= neededApples
令 n = cbrt(neededApples / 4), 其中 cbrt 为立方根
若 2 * n * (n + 1) * (2 * n + 1) < neededApples, 则 n 自增 1
时间复杂度为 O(1), 空间复杂度为 O(1), cbrt 为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minimumPerimeter(long long neededApples) 
    {
        return (((long long)cbrt(neededApples / 4.0)) * (((long long)cbrt(neededApples / 4.0)) + 1) * ((((long long)cbrt(neededApples / 4.0)) << 1) + 1) << 1) < neededApples ? ((((long long)cbrt(neededApples / 4.0)) + 1) << 3) : ((long long)cbrt(neededApples / 4.0) << 3);
    }
};
```

__Java__:

```Java
class Solution {
    public long minimumPerimeter(long neededApples) {
        return (((((long) Math.cbrt(neededApples / 4.0)) * (((long) Math.cbrt(neededApples / 4.0)) + 1) * ((((long) Math.cbrt(neededApples / 4.0)) << 1) + 1)) << 1) < neededApples) ? (((long) Math.cbrt(neededApples / 4.0) + 1) << 3) : ((long) Math.cbrt(neededApples / 4.0) << 3);
    }
}
```

__Python__:

```Python
class Solution:
    def minimumPerimeter(self, neededApples: int) -> int:
        return ((n + 1) << 3) if (n := int(cbrt(neededApples / 4))) * (n + 1) * ((n << 1) + 1) << 1 < neededApples else (n << 3)
```
