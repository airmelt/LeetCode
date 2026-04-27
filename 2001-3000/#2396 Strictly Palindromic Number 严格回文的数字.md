# 2396 Strictly Palindromic Number 严格回文的数字

__Description:__

An integer `n` is __strictly palindromic__ if, for __every__ base `b` between `2` and `n - 2` (__inclusive__), the string representation of the integer `n` in base `b` is __palindromic__.

Given an integer `n`, return `true` _if_ `n` _is __strictly palindromic__ and_ `false` _otherwise_.

A string is palindromic if it reads the same forward and backward.

__Example:__

Example 1:

```text
Input: n = 9
Output: false
Explanation: In base 2: 9 = 1001 (base 2), which is palindromic.
In base 3: 9 = 100 (base 3), which is not palindromic.
Therefore, 9 is not strictly palindromic so we return false.
Note that in bases 4, 5, 6, and 7, n = 9 is also not palindromic.
```

Example 2:

```text
Input: n = 4
Output: false
Explanation: We only consider base 2: 4 = 100 (base 2), which is not palindromic.
Therefore, we return false.
```

__Constraints:__

- `4 <= n <= 10 ^ 5`

__题目描述:__

如果一个整数 `n` 在 `b` 进制下（ `b` 为 `2` 到 `n - 2` 之间的所有整数）对应的字符串 __全部__ 都是 __回文的__ ，那么我们称这个数 `n` 是 __严格回文__ 的。

给你一个整数 `n` ，如果 `n` 是 __严格回文__ 的，请返回 `true` ，否则返回 `false` 。

如果一个字符串从前往后读和从后往前读完全相同，那么这个字符串是 回文的 。

__示例:__

示例 1：

```text
输入：n = 9
输出：false
解释：在 2 进制下：9 = 1001 ，是回文的。
在 3 进制下：9 = 100 ，不是回文的。
所以，9 不是严格回文数字，我们返回 false 。
注意在 4, 5, 6 和 7 进制下，n = 9 都不是回文的。
```

示例 2：

```text
输入：n = 4
输出：false
解释：我们只考虑 2 进制：4 = 100 ，不是回文的。
所以我们返回 false 。
```

__提示：__

- `4 <= n <= 10 ^ 5`

__思路:__

```text
脑筋急转弯
史上最简单的中等题目
当 n >= 4 时
n 在 n - 2 进制下一定是 12
这是由于 n - (n - 2) = 2, 所以 n = (n - 2) * 1 + 2, n >= 4 时都成立
而 12 不是回文数
直接返回 false
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isStrictlyPalindromic(int n) 
    {
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isStrictlyPalindromic(int n) {
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def isStrictlyPalindromic(self, n: int) -> bool:
        return False
```
