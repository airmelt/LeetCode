# 2485 Find the Pivot Integer 找出中枢整数

__Description:__

Given a positive integer `n`, find the __pivot integer__ `x` such that:

- The sum of all elements between `1` and `x` inclusively equals the sum of all elements between `x` and `n` inclusively.

Return _the pivot integer_ `x`. If no such integer exists, return `-1`. It is guaranteed that there will be at most one pivot index for the given input.

__Example:__

Example 1:

```text
Input: n = 8
Output: 6
Explanation: 6 is the pivot integer since: 1 + 2 + 3 + 4 + 5 + 6 = 6 + 7 + 8 = 21.
```

Example 2:

```text
Input: n = 1
Output: 1
Explanation: 1 is the pivot integer since: 1 = 1.
```

Example 3:

```text
Input: n = 4
Output: -1
Explanation: It can be proved that no such integer exist.
```

__Constraints:__

- `1 <= n <= 1000`

__题目描述:__

给你一个正整数 `n` ，找出满足下述条件的 __中枢整数__ `x` :

- `1` 和 `x` 之间的所有元素之和等于 `x` 和 `n` 之间所有元素之和。

返回中枢整数 `x` 。如果不存在中枢整数，则返回 `-1` 。题目保证对于给定的输入，至多存在一个中枢整数。

__示例:__

示例 1：

```text
输入：n = 8
输出：6
解释：6 是中枢整数，因为 1 + 2 + 3 + 4 + 5 + 6 = 6 + 7 + 8 = 21 。
```

示例 2：

```text
输入：n = 1
输出：1
解释：1 是中枢整数，因为 1 = 1 。
```

示例 3：

```text
输入：n = 4
输出：-1
解释：可以证明不存在满足题目要求的整数。
```

__提示：__

- `1 <= n <= 1000`

__思路:__

```text
1. 数学
1 + 2 + ... + n = n * (n + 1) / 2
1 + 2 + ... + x = x * (x + 1) / 2
x + (x + 1) + ... + n = n * (n + 1) / 2 - x * (x - 1) / 2
x * (x + 1) / 2 = n * (n + 1) / 2 - x * (x - 1) / 2
x = sqrt(n * (n + 1) / 2)
x 是整数, 返回 x
否则返回 -1
时间复杂度为 O(1), 空间复杂度为 O(1)
2. 打表
1. 1, 8, 49, 288
2. 1, 6, 35, 204
只有 4 个数
可以直接打表
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int pivotInteger(int n) 
    {
        return (int)sqrt(n = (n * (n + 1) >> 1)) * (int)sqrt(n) == n ? sqrt(n) : -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int pivotInteger(int n) {
        return Map.of(1, 1, 8, 6, 49, 35, 288, 204).getOrDefault(n, -1);
    }
}
```

__Python__:

```Python
class Solution:
    def pivotInteger(self, n: int) -> int:
        return -1 if (x := int((m := (n * (n + 1) >> 1)) ** 0.5)) * x != m else x
```
