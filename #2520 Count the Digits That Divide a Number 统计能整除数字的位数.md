# 2520 Count the Digits That Divide a Number 统计能整除数字的位数

__Description:__

Given an integer `num`, return _the number of digits in `num` that divide_ `num`.

An integer `val` divides `nums` if `nums % val == 0`.

__Example:__

Example 1:

```text
Input: num = 7
Output: 1
Explanation: 7 divides itself, hence the answer is 1.
```

Example 2:

```text
Input: num = 121
Output: 2
Explanation: 121 is divisible by 1, but not 2. Since 1 occurs twice as a digit, we return 2.
```

Example 3:

```text
Input: num = 1248
Output: 4
Explanation: 1248 is divisible by all of its digits, hence the answer is 4.
```

__Constraints:__

- `1 <= num <= 10 ^ 9`
- `num` does not contain `0` as one of its digits.

__题目描述:__

给你一个整数 `num` ，返回 `num` 中能整除 `num` 的数位的数目。

如果满足 `nums % val == 0` ，则认为整数 `val` 可以整除 `nums` 。

__示例:__

示例 1：

```text
输入：num = 7
输出：1
解释：7 被自己整除，因此答案是 1 。
```

示例 2：

```text
输入：num = 121
输出：2
解释：121 可以被 1 整除，但无法被 2 整除。由于 1 出现两次，所以返回 2 。
```

示例 3：

```text
输入：num = 1248
输出：4
解释：1248 可以被它每一位上的数字整除，因此答案是 4 。
```

__提示：__

- `1 <= num <= 10 ^ 9`
- `num` 的数位中不含 `0`

__思路:__

```text
1. 字符串
将数字转化为字符串
遍历每一位数字
判断是否能整除
时间复杂度为 O(N), 空间复杂度为 O(N)
2. 数学
用对 10 取模取出每一位数字
检查该数字是否能整除
时间复杂度为 O(N), 空间复杂度为 O(1)
N 为数字的位数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countDigits(int num) 
    {
        int result = 0;
        for (int x = num; x; x /= 10) result += !(num % (x % 10));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countDigits(int num) {
        int result = 0;
        for (int x = num; x != 0; x /= 10) result += num % (x % 10) == 0 ? 1 : 0;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countDigits(self, num: int) -> int:
        return sum(not num % int(i) for i in str(num))
```
