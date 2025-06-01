# 2549 Count Distinct Numbers on Board 统计桌面上的不同数字

__Description:__

You are given a positive integer `n`, that is initially placed on a board. Every day, for `10 ^ 9` days, you perform the following procedure:

- For each number `x` present on the board, find all numbers `1 <= i <= n` such that `x % i == 1`.
- Then, place those numbers on the board.

Return _the number of __distinct__ integers present on the board after_ `10 ^ 9` _days have elapsed_.

Note:

- Once a number is placed on the board, it will remain on it until the end.
- `%` stands for the modulo operation. For example, `14 % 3` is `2`.

__Example:__

Example 1:

```text
Input: n = 5
Output: 4
Explanation: Initially, 5 is present on the board. 
The next day, 2 and 4 will be added since 5 % 2 == 1 and 5 % 4 == 1. 
After that day, 3 will be added to the board because 4 % 3 == 1. 
At the end of a billion days, the distinct numbers on the board will be 2, 3, 4, and 5.
```

Example 2:

```text
Input: n = 3
Output: 2
Explanation: 
Since 3 % 2 == 1, 2 will be added to the board. 
After a billion days, the only two distinct numbers on the board are 2 and 3.
```

__Constraints:__

- `1 <= n <= 100`

__题目描述:__

给你一个正整数 `n` ，开始时，它放在桌面上。在 `10 ^ 9` 天内，每天都要执行下述步骤:

- 对于出现在桌面上的每个数字 `x` ，找出符合 `1 <= i <= n` 且满足 `x % i == 1` 的所有数字 `i` 。
- 然后，将这些数字放在桌面上。

返回在 `10 ^ 9` 天之后，出现在桌面上的 __不同__ 整数的数目。

注意：

- 一旦数字放在桌面上，则会一直保留直到结束。
- `%` 表示取余运算。例如， `14 % 3` 等于 `2` 。

__示例:__

示例 1：

```text
输入：n = 5
输出：4
解释：最开始，5 在桌面上。 
第二天，2 和 4 也出现在桌面上，因为 5 % 2 == 1 且 5 % 4 == 1 。 
再过一天 3 也出现在桌面上，因为 4 % 3 == 1 。 
在十亿天结束时，桌面上的不同数字有 2 、3 、4 、5 。
```

示例 2：

```text
输入：n = 3 
输出：2
解释： 
因为 3 % 2 == 1 ，2 也出现在桌面上。 
在十亿天结束时，桌面上的不同数字只有两个：2 和 3 。
```

__提示：__

- `1 <= n <= 100`

__思路:__

```text
数学
由于天数足够大
所以 1 - n - 1 都会出现在桌面上
因此，桌面上不同数字的数量为 n - 1
如果 n = 1，则桌面上只有数字 1，所以返回 1
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int distinctIntegers(int n) 
    {
        return max(n - 1, 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int distinctIntegers(int n) {
        return Math.max(n - 1, 1);
    }
}
```

__Python__:

```Python
class Solution:
    def distinctIntegers(self, n: int) -> int:
        return max(n - 1, 1) 
```
