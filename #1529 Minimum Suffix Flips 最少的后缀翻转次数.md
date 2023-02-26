# 1529 Minimum Suffix Flips 最少的后缀翻转次数

__Description:__

You are given a __0-indexed__ binary string `target` of length `n`. You have another binary string `s` of length `n` that is initially set to all zeros. You want to make `s` equal to `target`.

In one operation, you can pick an index `i` where `0 <= i < n` and flip all bits in the __inclusive__ range `[i, n - 1]`. Flip means changing `'0'` to `'1'` and `'1'` to `'0'`.

Return _the minimum number of operations needed to make_ `s` _equal to_ `target`.

__Example:__

Example 1:

```text
Input: target = "10111"
Output: 3
Explanation: Initially, s = "00000".
Choose index i = 2: "00000" -> "00111"
Choose index i = 0: "00111" -> "11000"
Choose index i = 1: "11000" -> "10111"
We need at least 3 flip operations to form target.
```

Example 2:

```text
Input: target = "101"
Output: 3
Explanation: Initially, s = "000".
Choose index i = 0: "000" -> "111"
Choose index i = 1: "111" -> "100"
Choose index i = 2: "100" -> "101"
We need at least 3 flip operations to form target.
```

Example 3:

```text
Input: target = "00000"
Output: 0
Explanation: We do not need any operations since the initial s already equals target.
```

__Constraints:__

- `n == target.length`
- `1 <= n <= 10 ^ 5`
- `target[i]` is either `'0'` or `'1'`.

__题目描述:__

给你一个长度为 `n` 、下标从 __0__ 开始的二进制字符串 `target` 。你自己有另一个长度为 `n` 的二进制字符串 `s` ，最初每一位上都是 0 。你想要让 `s` 和 `target` 相等。

在一步操作，你可以选择下标 `i`（ `0 <= i < n`）并翻转在 __闭区间__ `[i, n - 1]` 内的所有位。翻转意味着 `'0'` 变为 `'1'` ，而 `'1'` 变为 `'0'` 。

返回使 `s` 与 `target` 相等需要的最少翻转次数。

__示例:__

示例 1：

```text
输入：target = "10111"
输出：3
解释：最初，s = "00000" 。
选择下标 i = 2: "00000" -> "00111"
选择下标 i = 0: "00111" -> "11000"
选择下标 i = 1: "11000" -> "10111"
要达成目标，需要至少 3 次翻转。
```

示例 2：

```text
输入：target = "101"
输出：3
解释：最初，s = "000" 。
选择下标 i = 0: "000" -> "111"
选择下标 i = 1: "111" -> "100"
选择下标 i = 2: "100" -> "101"
要达成目标，需要至少 3 次翻转。
```

示例 3：

```text
输入：target = "00000"
输出：0
解释：由于 s 已经等于目标，所以不需要任何操作
```

__提示：__

- `n == target.length`
- `1 <= n <= 10 ^ 5`
- `target[i]` 为 `'0'` 或 `'1'`

__思路:__

```text
贪心
从第一个数开始, 如果是 ‘1’, 需要一次翻转
两个相邻的字符如果相等, 可以一次翻转, 如果不等需要增加一次翻转
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minFlips(string target) 
    {
        int result = target.front() == '1', n = target.size();
        for (int i = 1; i < n; i++) result += target[i] != target[i - 1];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minFlips(String target) {
        int result = 0, n = target.length(), pre = (int)'0';
        for (int c: target.toCharArray()) {
            if (pre != c) ++result;
            pre = c;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minFlips(self, target: str) -> int:
        return sum(target[i] != target[i + 1] for i in range(len(target) - 1)) + (target[0] == '1')
```
