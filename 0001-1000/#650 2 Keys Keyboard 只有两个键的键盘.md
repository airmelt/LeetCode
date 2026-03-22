# 650 2 Keys Keyboard 只有两个键的键盘

__Description__:
There is only one character 'A' on the screen of a notepad. You can perform two operations on this notepad for each step:

Copy All: You can copy all the characters present on the screen (a partial copy is not allowed).
Paste: You can paste the characters which are copied last time.
Given an integer n, return the minimum number of operations to get the character 'A' exactly n times on the screen.

__Example:__

Example 1:

Input: n = 3
Output: 3
Explanation: Intitally, we have one character 'A'.
In step 1, we use Copy All operation.
In step 2, we use Paste operation to get 'AA'.
In step 3, we use Paste operation to get 'AAA'.

Example 2:

Input: n = 1
Output: 0

__Constraints:__

1 <= n <= 1000

__题目描述__:
最初在一个记事本上只有一个字符 'A'。你每次可以对这个记事本进行两种操作：

Copy All (复制全部) : 你可以复制这个记事本中的所有字符(部分的复制是不允许的)。
Paste (粘贴) : 你可以粘贴你上一次复制的字符。
给定一个数字 n 。你需要使用最少的操作次数，在记事本中打印出恰好 n 个 'A'。输出能够打印出 n 个 'A' 的最少操作次数。

__示例 :__

示例 1:

输入: 3
输出: 3
解释:
最初, 我们只有一个字符 'A'。
第 1 步, 我们使用 Copy All 操作。
第 2 步, 我们使用 Paste 操作来获得 'AA'。
第 3 步, 我们使用 Paste 操作来获得 'AAA'。

__说明:__
n 的取值范围是 [1, 1000] 。

__思路__:

质因数分解
时间复杂度 O(n ^ 1 / 2), 空间复杂度 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minSteps(int n) 
    {
        int result = 0, factor = 2;
        while (n > 1)
        {
            while (!(n % factor))
            {
                result += factor;
                n /= factor;
            }
            ++factor;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minSteps(int n) {
        int result = 0, factor = 2;
        while (n > 1) {
            while (n % factor == 0) {
                result += factor;
                n /= factor;
            }
            ++factor;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minSteps(self, n: int) -> int:
        result, factor = 0, 2
        while n > 1:
            while not n % factor:
                result += factor
                n //= factor
            factor += 1
        return result
```
