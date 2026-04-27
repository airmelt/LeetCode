# 1780 Check if Number is a Sum of Powers of Three 判断一个数字是否可以表示成三的幂的和

__Description:__

Given an integer `n`, return `true` _if it is possible to represent_ `n` _as the sum of distinct powers of three._ Otherwise, return `false`.

An integer `y` is a power of three if there exists an integer `x` such that `y == 3 ^ x`.

__Example:__

Example 1:

```text
Input: n = 12
Output: true
Explanation: 12 = 31 + 32
```

Example 2:

```text
Input: n = 91
Output: true
Explanation: 91 = 30 + 32 + 34
```

Example 3:

```text
Input: n = 21
Output: false
```

__Constraints:__

- `1 <= n <= 10 ^ 7`

__题目描述:__

给你一个整数 `n` ，如果你可以将 `n` 表示成若干个不同的三的幂之和，请你返回 `true` ，否则请返回 `false` 。

对于一个整数 `y` ，如果存在整数 `x` 满足 `y == 3 ^ x` ，我们称这个整数 `y` 是三的幂。

__示例:__

示例 1：

```text
输入：n = 12
输出：true
解释：12 = 31 + 32
```

示例 2：

```text
输入：n = 91
输出：true
解释：91 = 30 + 32 + 34
```

示例 3：

```text
输入：n = 21
输出：false
```

__提示：__

- `1 <= n <= 10 ^ 7`

__思路:__

```text
数学
将这个数转化为 3 进制, 如果 3 进制中不包含 2, 那么这个数就可以表示成三的幂的和
时间复杂度为 O(lgN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool checkPowersOfThree(int n) 
    {
        return n % 3 < 2 and (!n or checkPowersOfThree(n / 3));
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkPowersOfThree(int n) {
        return !Integer.toString(n, 3).contains("2");
    }
}
```

__Python__:

```Python
class Solution {
    public boolean checkPowersOfThree(int n) {
        return !Integer.toString(n, 3).contains("2");
    }
}
```
