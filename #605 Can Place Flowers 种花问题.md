# 605 Can Place Flowers 种花问题

__Description__:
Suppose you have a long flowerbed in which some of the plots are planted and some are not. However, flowers cannot be planted in adjacent plots - they would compete for water and both would die.

Given a flowerbed (represented as an array containing 0 and 1, where 0 means empty and 1 means not empty), and a number n, return if n new flowers can be planted in it without violating the no-adjacent-flowers rule.

__Example:__

Example 1:
Input: flowerbed = [1,0,0,0,1], n = 1
Output: True

Example 2:
Input: flowerbed = [1,0,0,0,1], n = 2
Output: False

__Note:__
The input array won't violate no-adjacent-flowers rule.
The input array size is in the range of [1, 20000].
n is a non-negative integer which won't exceed the input array size.

__题目描述__:
假设你有一个很长的花坛，一部分地块种植了花，另一部分却没有。可是，花卉不能种植在相邻的地块上，它们会争夺水源，两者都会死去。

给定一个花坛（表示为一个数组包含0和1，其中0表示没种植花，1表示种植了花），和一个数 n 。能否在不打破种植规则的情况下种入 n 朵花？能则返回True，不能则返回False。

__示例 :__

示例 1:

输入: flowerbed = [1,0,0,0,1], n = 1
输出: True

示例 2:

输入: flowerbed = [1,0,0,0,1], n = 2
输出: False

__注意:__

数组内已种好的花不会违反种植规则。
输入的数组长度范围为 [1, 20000]。
n 是非负整数，且不会超过输入数组的大小。

__思路__:

1. 遍历花坛, 按照规则种树即可
2. 可以在花坛两端增加两个空的花盆, 减少判断量
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) 
    {
        for (int i = 0; i < flowerbed.size(); i++) 
        {
            if (!flowerbed[i] && (i == 0 or !flowerbed[i - 1]) and (i == flowerbed.size() - 1 or !flowerbed[i + 1])) 
            {
                n--;
                flowerbed[i] = 1;
            }
        }
        return n <= 0;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        for (int i = 0; i < flowerbed.length; i++) {
            if (flowerbed[i] == 0 && (i == 0 || flowerbed[i - 1] == 0) && (i == flowerbed.length - 1 || flowerbed[i + 1] == 0)) {
                n--;
                flowerbed[i] = 1;
            }
        }
        return n <= 0;
    }
}
```

__Python__:

```Python
class Solution:
    def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
        flowerbed = [0] + flowerbed + [0]
        for i in range(1, len(flowerbed) - 1):
            if not flowerbed[i] and not flowerbed[i + 1] and not flowerbed[i - 1]:
                flowerbed[i] = 1
                n -= 1
        return n <= 0
```
