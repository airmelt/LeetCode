# 793 Preimage Size of Factorial Zeroes Function 阶乘函数后 K 个零

__Description__:
Let f(x) be the number of zeroes at the end of x!. Recall that x! = 1 *\ 2 \* 3 \* ... \* x and by convention, 0! = 1.

For example, f(3) = 0 because 3! = 6 has no zeroes at the end, while f(11) = 2 because 11! = 39916800 has two zeroes at the end.
Given an integer k, return the number of non-negative integers x have the property that f(x) = k.

__Example:__

Example 1:

Input: k = 0
Output: 5
Explanation: 0!, 1!, 2!, 3!, and 4! end with k = 0 zeroes.

Example 2:

Input: k = 5
Output: 0
Explanation: There is no x such that x! ends in k = 5 zeroes.

Example 3:

Input: k = 3
Output: 5

__Constraints:__

0 <= k <= 10^9

__题目描述__:
f(x) 是 x! 末尾是 0 的数量。（回想一下 x! = 1 \* 2 \* 3 \* ... \* x，且 0! = 1 ）

例如， f(3) = 0 ，因为 3! = 6 的末尾没有 0 ；而 f(11) = 2 ，因为 11!= 39916800 末端有 2 个 0 。给定 K，找出多少个非负整数 x ，能满足 f(x) = K 。

__示例 :__

示例 1：

输入：K = 0
输出：5
解释：0!, 1!, 2!, 3!, and 4! 均符合 K = 0 的条件。

示例 2：

输入：K = 5
输出：0
解释：没有匹配到这样的 x!，符合 K = 5 的条件。

__提示:__

K 是范围在 [0, 10^9] 的整数。

__思路__:

数学
阶乘后能得到 0 只有 2 \* 5, 因为 2 的数量远超过 5, 只用考虑含有 5 的因子的数即可
5 对应 1 个
25 对应 6 个, 实际上是 5 个 5 加上 25 自身
那么 f(5 ^ 1) = 1, f(5 ^ 2) = 5 * f(1) + 1, 以此类推即可
k =  x / 5 + x ^ 2 / 5 + ... + x ^ m / 5, 求出 x 的个数
结果不是 0 就是 5
时间复杂度为 O(1), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int preimageSizeFZF(int k) 
    {
        return k % 61035156 % 12207031 % 2441406 % 488281 % 97656 % 19531 % 3906 % 781 % 156 % 31 == 30 ? 0 : !(k % 61035156 % 12207031 % 2441406 % 488281 % 97656 % 19531 % 3906 % 781 % 156 % 31 % 6 / 5) * 5;
    }
};
```

__Java__:

```Java
class Solution {
    public int preimageSizeFZF(int k) {
        return k % 61035156 % 12207031 % 2441406 % 488281 % 97656 % 19531 % 3906 % 781 % 156 % 31 == 30 ? 0 : (k % 61035156 % 12207031 % 2441406 % 488281 % 97656 % 19531 % 3906 % 781 % 156 % 31 % 6 / 5 == 0 ? 1 : 0) * 5;
    }
}
```

__Python__:

```Python
class Solution:
    def preimageSizeFZF(self, k: int) -> int:
        return 0 if k % 61035156 % 12207031 % 2441406 % 488281 % 97656 % 19531 % 3906 % 781 % 156 % 31 == 30 else (not k % 61035156 % 12207031 % 2441406 % 488281 % 97656 % 19531 % 3906 % 781 % 156 % 31 % 6 // 5) * 5
```
