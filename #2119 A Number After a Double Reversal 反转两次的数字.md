# 2119 A Number After a Double Reversal 反转两次的数字

__Description:__

Reversing an integer means to reverse all its digits.

- For example, reversing `2021` gives `1202`. Reversing `12300` gives `321` as the __leading zeros are not retained__.

Given an integer `num`, __reverse__ `num` to get `reversed1`, __then reverse__ `reversed1` to get `reversed2`. Return `true` _if_ `reversed2` _equals_ `num`. Otherwise return `false`.

__Example:__

Example 1:

```text
Input: num = 526
Output: true
Explanation: Reverse num to get 625, then reverse 625 to get 526, which equals num.
```

Example 2:

```text
Input: num = 1800
Output: false
Explanation: Reverse num to get 81, then reverse 81 to get 18, which does not equal num.
```

Example 3:

```text
Input: num = 0
Output: true
Explanation: Reverse num to get 0, then reverse 0 to get 0, which equals num.
```

__Constraints:__

- `0 <= num <= 10 ^ 6`

__题目描述:__

反转 一个整数意味着倒置它的所有位。

- 例如，反转 `2021` 得到 `1202` 。反转 `12300` 得到 `321` ，__不保留前导零__ 。

给你一个整数 `num` ，__反转__ `num` 得到 `reversed1` ，__接着反转__ `reversed1` 得到 `reversed2` 。如果 `reversed2` 等于 `num` ，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：num = 526
输出：true
解释：反转 num 得到 625 ，接着反转 625 得到 526 ，等于 num 。
```

示例 2：

```text
输入：num = 1800
输出：false
解释：反转 num 得到 81 ，接着反转 81 得到 18 ，不等于 num 。
```

示例 3：

```text
输入：num = 0
输出：true
解释：反转 num 得到 0 ，接着反转 0 得到 0 ，等于 num 。
```

__提示：__

- `0 <= num <= 10 ^ 6`

__思路:__

```text
数学
要么 n 为 0
或者 n 的最后一位不能是 0
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isSameAfterReversals(int num) 
    {
        return !num or !!(num % 10);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isSameAfterReversals(int num) {
        return num == 0 || num % 10 != 0;
    }
}
```

__Python__:

```Python
class Solution:
    def isSameAfterReversals(self, num: int) -> bool:
        return not num or not not num % 10
```
