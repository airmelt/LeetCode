# 441 Arranging Coins 排列硬币

__Description__:
You have a total of n coins that you want to form in a staircase shape, where every k-th row must have exactly k coins.

Given n, find the total number of full staircase rows that can be formed.

n is a non-negative integer and fits within the range of a 32-bit signed integer.

__Example:__

Example 1:

n = 5

The coins can form the following rows:

```text
¤
¤ ¤
¤ ¤
```

Because the 3rd row is incomplete, we return 2.
Example 2:

n = 8

The coins can form the following rows:

```text
¤
¤ ¤
¤ ¤ ¤
¤ ¤
```

Because the 4th row is incomplete, we return 3.

__题目描述__:
你总共有 n 枚硬币，你需要将它们摆成一个阶梯形状，第 k 行就必须正好有 k 枚硬币。

给定一个数字 n，找出可形成完整阶梯行的总行数。

n 是一个非负整数，并且在32位有符号整型的范围内。

__示例：__

示例 1:

n = 5

硬币可排列成以下几行:

```text
¤
¤ ¤
¤ ¤
```

因为第三行不完整，所以返回2.
示例 2:

n = 8

硬币可排列成以下几行:

```text
¤
¤ ¤
¤ ¤ ¤
¤ ¤
```

因为第四行不完整，所以返回3.

__思路__:

返回值是找到 (i + 1) * i / 2 >= n的最小的 i
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int arrangeCoins(int n) 
    {
        return (int)(-1 + sqrt(1 + ((long)n << 3))) >> 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int arrangeCoins(int n) {
        return (int)(-1 + Math.sqrt(1 + ((long)n << 3))) >> 1;
    }
}
```

__Python__:

```Python
class Solution:
    def arrangeCoins(self, n: int) -> int:
        return int((-1 + (1 + (n << 3)) ** 0.5)) >> 1
```
