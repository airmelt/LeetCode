# 3099 Harshad Number 哈沙德数

__Description:__

An integer divisible by the __sum__ of its digits is said to be a __Harshad__ number. You are given an integer `x`. Return _the sum of the digits_ of `x` if `x` is a __Harshad__ number, otherwise, return `-1`_._

__Example:__

Example 1:

```text
Input: x = 18

Output: 9

Explanation:
```

The sum of digits of `x` is `9`. `18` is divisible by `9`. So `18` is a Harshad number and the answer is `9`.

Example 2:

```text
Input: x = 23

Output: -1

Explanation:
```

The sum of digits of `x` is `5`. `23` is not divisible by `5`. So `23` is not a Harshad number and the answer is `-1`.

__Constraints:__

- `1 <= x <= 100`

__题目描述:__

如果一个整数能够被其各个数位上的数字之和整除，则称之为 __哈沙德数__（Harshad number）。给你一个整数 `x` 。如果 `x` 是 __哈沙德数__ ，则返回 `x` 各个数位上的数字之和，否则，返回 `-1` 。

__示例:__

示例 1：

```text
输入： x = 18

输出： 9

解释：
```

`x` 各个数位上的数字之和为 `9` 。 `18` 能被 `9` 整除。因此 `18` 是哈沙德数，答案是 `9` 。

示例 2：

```text
输入： x = 23

输出： -1

解释：
```

`x` 各个数位上的数字之和为 `5` 。 `23` 不能被 `5` 整除。因此 `23` 不是哈沙德数，答案是 `-1` 。

__提示：__

- `1 <= x <= 100`

__思路:__

```text
数学
将每一位上的数字计算出总和
再用原值取模
如果能够除尽返回总和
否则返回 -1
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumOfTheDigitsOfHarshadNumber(int x) 
    {
        int a = 0;
        for (int v = x; v > 0; v /= 10) a += v % 10;
        return x % a ? -1 : a;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumOfTheDigitsOfHarshadNumber(int x) {
        int a = 0;
        for (int v = x; v > 0; v /= 10) a += v % 10;
        return x % a > 0 ? -1 : a;
    }
}
```

__Python__:

```Python
class Solution:
    def sumOfTheDigitsOfHarshadNumber(self, x: int) -> int:
        return a if not x % (a := sum(int(i) for i in str(x))) else -1
```
