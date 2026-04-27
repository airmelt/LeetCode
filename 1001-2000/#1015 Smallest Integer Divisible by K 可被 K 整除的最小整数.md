# 1015 Smallest Integer Divisible by K 可被 K 整除的最小整数

__Description__:
Given a positive integer k, you need to find the length of the smallest positive integer n such that n is divisible by k, and n only contains the digit 1.

Return the length of n. If there is no such n, return -1.

__Note:__
n may not fit in a 64-bit signed integer.

__Example:__

Example 1:

Input: k = 1
Output: 1
Explanation: The smallest answer is n = 1, which has length 1.

Example 2:

Input: k = 2
Output: -1
Explanation: There is no such positive integer n divisible by 2.

Example 3:

Input: k = 3
Output: 3
Explanation: The smallest answer is n = 111, which has length 3.

__Constraints:__

1 <= k <= 10^5

__题目描述__:
给定正整数 k ，你需要找出可以被 k 整除的、仅包含数字 1 的最 小 正整数 n 的长度。

返回 n 的长度。如果不存在这样的 n ，就返回-1。

__注意：__
n 不符合 64 位带符号整数。

__示例 :__

示例 1：

输入：k = 1
输出：1
解释：最小的答案是 n = 1，其长度为 1。

示例 2：

输入：k = 2
输出：-1
解释：不存在可被 2 整除的正整数 n 。

示例 3：

输入：k = 3
输出：3
解释：最小的答案是 n = 111，其长度为 3。

__提示:__

1 <= k <= 10^5

__思路__:

数学模拟
cur 从 1 开始寻找, 下一个 next = cur \* 10 + 1
因为 cur % k != 0, 令 cur = p \* k + q, 其中 p, q 为整数
由于 next % k = (cur \* 10 + 1) % k = ((pk + q) \* 10 + 1) % k = (10pk + 10q + 1) % k
又由于 10pk % k 恒等于 0
所以 next % k = (10q + 1) % k, 注意到 cur = pk + q, 所以 cur % k = (pk + q) % k = q
所以 next % k = (10q + 1) % k = (10 \* cur) % k + 1
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int smallestRepunitDivByK(int k)
    {
        if (!(k & 1) or !(k % 5)) return -1;
        int cur = 1, result = 1;
        while (cur % k) 
        {
            ++result;
            cur = (cur * 10) % k + 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int smallestRepunitDivByK(int k) {
        if ((k & 1) == 0 || k % 5 == 0) return -1;
        int cur = 1, result = 1;
        while (cur % k != 0) {
            ++result;
            cur = (cur * 10) % k + 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def smallestRepunitDivByK(self, k: int) -> int:
        if str(k % 10) not in "1379": 
            return -1
        num = "1" * len(str(k))
        result, remainder = len(num), int(num) % k
        while remainder:
            remainder = (10 * remainder + 1) % k
            result += 1
        return result
```
