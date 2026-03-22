# 2729 Check if The Number is Fascinating 判断一个数是否迷人

__Description:__

You are given an integer `n` that consists of exactly `3` digits.

We call the number `n` __fascinating__ if, after the following modification, the resulting number contains all the digits from `1` to `9` __exactly__ once and does not contain any `0`'s:

- __Concatenate__ `n` with the numbers `2 * n` and `3 * n`.

Return `true` _if_ `n` _is fascinating, or_ `false` _otherwise_.

__Concatenating__ two numbers means joining them together. For example, the concatenation of `121` and `371` is `121371`.

__Example:__

Example 1:

```text
Input: n = 192
Output: true
Explanation: We concatenate the numbers n = 192 and 2 * n = 384 and 3 * n = 576. The resulting number is 192384576. This number contains all the digits from 1 to 9 exactly once.
```

Example 2:

```text
Input: n = 100
Output: false
Explanation: We concatenate the numbers n = 100 and 2 * n = 200 and 3 * n = 300. The resulting number is 100200300. This number does not satisfy any of the conditions.
```

__Constraints:__

- `100 <= n <= 999`

__题目描述:__

给你一个三位数整数 `n` 。

如果经过以下修改得到的数字 __恰好__ 包含数字 `1` 到 `9` 各一次且不包含任何 `0` ，那么我们称数字 `n` 是 __迷人的__ :

- 将 `n` 与数字 `2 * n` 和 `3 * n` __连接__ 。

如果 `n` 是迷人的，返回 `true`，否则返回 `false` 。

__连接__ 两个数字表示把它们首尾相接连在一起。比方说 `121` 和 `371` 连接得到 `121371` 。

__示例:__

示例 1：

```text
输入：n = 192
输出：true
解释：我们将数字 n = 192 ，2 * n = 384 和 3 * n = 576 连接，得到 192384576 。这个数字包含 1 到 9 恰好各一次。
```

示例 2：

```text
输入：n = 100
输出：false
解释：我们将数字 n = 100 ，2 * n = 200 和 3 * n = 300 连接，得到 100200300 。这个数字不符合上述条件。
```

__提示：__

- `100 <= n <= 999`

__思路:__

```text
1. 模拟
将 n 和 n * 2 和 n * 3 作为字符串记录下来
然后将每个数字映射到二进制上去
如果结果为 0b1111111110, 即 (1 << 10) - 2 即是迷人的
由于 n * 3 需要小于 1000, 所以大于 329 的都不用考虑, 330 以上的由于有重复数字也不用考虑
另外 n 也不能小于 123, 否则也会有重复数字
时间复杂度为 O(1), 空间复杂度为 O(1)
2. 打表
总共只有 4 个数字满足题意
[192, 219, 273, 327]
直接返回即可
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isFascinating(int n) 
    {
        int mask = 0;
        if (n < 330 and n > 122) for (const auto& c : to_string(n) + to_string(n << 1) + to_string(n * 3)) mask |= 1 << (c - '0');
        return mask == (1 << 10) - 2;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isFascinating(int n) {
        return n == 192 || n == 219 || n == 273 || n == 327;
    }
}
```

__Python__:

```Python
class Solution:
    def isFascinating(self, n: int) -> bool:
        return sorted(map(int, str(n) + str(n << 1) + str(n * 3))) == list(range(1, 10))
```
