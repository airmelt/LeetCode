# 2546 Apply Bitwise Operations to Make Strings Equal 执行逐位运算使字符串相等

__Description:__

You are given two __0-indexed binary__ strings `s` and `target` of the same length `n`. You can do the following operation on `s` __any__ number of times:

- Choose two __different__ indices `i` and `j` where `0 <= i, j < n`.
- Simultaneously, replace `s[i]` with ( `s[i]` __OR__ `s[j]`) and `s[j]` with ( `s[i]` __XOR__ `s[j]`).

For example, if `s = "0110"`, you can choose `i = 0` and `j = 2`, then simultaneously replace `s[0]` with ( `s[0]` __OR__ `s[2]` = `0` __OR__ `1` = `1`), and `s[2]` with ( `s[0]` __XOR__ `s[2]` = `0` __XOR__ `1` = `1`), so we will have `s = "1110"`.

Return `true` _if you can make the string_ `s` _equal to_ `target`_, or_ `false` _otherwise_.

__Example:__

Example 1:

```text
Input: s = "1010", target = "0110"
Output: true
Explanation: We can do the following operations:
```

- Choose i = 2 and j = 0. We have now s = "0010".
- Choose i = 2 and j = 1. We have now s = "0110".

Since we can make s equal to target, we return true.

Example 2:

```text
Input: s = "11", target = "00"
Output: false
Explanation: It is not possible to make s equal to target with any number of operations.
```

__Constraints:__

- `n == s.length == target.length`
- `2 <= n <= 10 ^ 5`
- `s` and `target` consist of only the digits `0` and `1`.

__题目描述:__

给你两个下标从 __0__ 开始的 __二元__ 字符串 `s` 和 `target` ，两个字符串的长度均为 `n` 。你可以对 `s` 执行下述操作 __任意__ 次:

- 选择两个 __不同__ 的下标 `i` 和 `j` ，其中 `0 <= i, j < n` 。
- 同时，将 `s[i]` 替换为 ( `s[i]` __OR__ `s[j]`) ， `s[j]` 替换为 ( `s[i]` __XOR__ `s[j]`) 。

例如，如果 `s = "0110"` ，你可以选择 `i = 0` 和 `j = 2`，然后同时将 `s[0]` 替换为 ( `s[0]` __OR__ `s[2]` = `0` __OR__ `1` = `1`)，并将 `s[2]` 替换为 ( `s[0]` __XOR__ `s[2]` = `0` __XOR__ `1` = `1`)，最终得到 `s = "1110"` 。

如果可以使 `s` 等于 `target` ，返回 `true` ，否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：s = "1010", target = "0110"
输出：true
解释：可以执行下述操作：
```

- 选择 i = 2 和 j = 0 ，得到 s = "0010".
- 选择 i = 2 和 j = 1 ，得到 s = "0110".

可以使 s 等于 target ，返回 true 。

示例 2：

```text
输入：s = "11", target = "00"
输出：false
解释：执行任意次操作都无法使 s 等于 target 。
```

__提示：__

- `n == s.length == target.length`
- `2 <= n <= 10 ^ 5`
- `s` 和 `target` 仅由数字 `0` 和 `1` 组成

__思路:__

```text
模拟
一共 4 种情况
11 -> 10
10 -> 11
01 -> 11
00 -> 00
所以要么 s 和 target 都有 1，要么都没有 1
否则无法使 s 等于 target
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool makeStringsEqual(string s, string target) 
    {
        return (s.find('1') == string::npos) == (target.find('1') == string::npos);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean makeStringsEqual(String s, String target) {
        return s.contains("1") == target.contains("1");
    }
}
```

__Python__:

```Python
class Solution:
    def makeStringsEqual(self, s: str, target: str) -> bool:
        return ('1' in s) == ('1' in target)
```
