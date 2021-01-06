# 365 Water and Jug Problem 水壶问题

__Description__:
You are given two jugs with capacities x and y litres. There is an infinite amount of water supply available. You need to determine whether it is possible to measure exactly z litres using these two jugs.

If z liters of water is measurable, you must have z liters of water contained within one or both buckets by the end.

Operations allowed:

Fill any of the jugs completely with water.
Empty any of the jugs.
Pour water from one jug into another till the other jug is completely full or the first jug itself is empty.

__Example:__

Example 1: (From the famous "Die Hard" example)

Input: x = 3, y = 5, z = 4
Output: True

Example 2:

Input: x = 2, y = 6, z = 5
Output: False

__Constraints:__

0 <= x <= 10^6
0 <= y <= 10^6
0 <= z <= 10^6

__题目描述__:
有两个容量分别为 x升 和 y升 的水壶以及无限多的水。请判断能否通过使用这两个水壶，从而可以得到恰好 z升 的水？

如果可以，最后请用以上水壶中的一或两个来盛放取得的 z升 水。

你允许：

装满任意一个水壶
清空任意一个水壶
从一个水壶向另外一个水壶倒水，直到装满或者倒空

__示例 :__

示例 1: (From the famous "Die Hard" example)

输入: x = 3, y = 5, z = 4
输出: True

示例 2:

输入: x = 2, y = 6, z = 5
输出: False

__思路__:

只要 z能整除 x和 y的最大公约数就为真
时间复杂度O(lgmin(x, y)), 空间复杂度O(1), 主要时间用于计算 gcd(x, y)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool canMeasureWater(int x, int y, int z) 
    {
        if (z == 0) return true;
        if (x + y < z) return false;
        return z % gcd(x, y) == 0;
    }
private:
    int gcd(int x, int y) 
    {
        return y == 0 ? x : gcd(y, x % y);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canMeasureWater(int x, int y, int z) {
        if (z == 0) return true;
        if (x + y < z) return false;
        return z % gcd(x, y) == 0;
    }
    
    private int gcd(int x, int y) {
        return y == 0 ? x : gcd(y, x % y);
    }
}
```

__Python__:

```Python
class Solution:
    def canMeasureWater(self, x: int, y: int, z: int) -> bool:
        return not z or x + y >= z and not z % math.gcd(x, y)
```
