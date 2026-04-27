# 1573 Number of Ways to Split a String 分割字符串的方案数

__Description:__

Given a binary string `s`, you can split `s` into 3 __non-empty__ strings `s1`, `s2`, and `s3` where `s1 + s2 + s3 = s`.

Return the number of ways `s` can be split such that the number of ones is the same in `s1`, `s2`, and `s3`. Since the answer may be too large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: s = "10101"
Output: 4
Explanation: There are four ways to split s in 3 parts where each part contain the same number of letters '1'.
"1|010|1"
"1|01|01"
"10|10|1"
"10|1|01"
```

Example 2:

```text
Input: s = "1001"
Output: 0
```

Example 3:

```text
Input: s = "0000"
Output: 3
Explanation: There are three ways to split s in 3 parts.
"0|0|00"
"0|00|0"
"00|0|0"
```

__Constraints:__

- `3 <= s.length <= 10 ^ 5`
- `s[i]` is either `'0'` or `'1'`.

__题目描述:__

给你一个二进制串 `s` （一个只包含 0 和 1 的字符串），我们可以将 `s` 分割成 3 个 __非空__ 字符串 s1, s2, s3 （s1 + s2 + s3 = s）。

请你返回分割 `s` 的方案数，满足 s1，s2 和 s3 中字符 '1' 的数目相同。

由于答案可能很大，请将它对 10 ^ 9 + 7 取余后返回。

__示例:__

示例 1：

```text
输入：s = "10101"
输出：4
解释：总共有 4 种方法将 s 分割成含有 '1' 数目相同的三个子字符串。
"1|010|1"
"1|01|01"
"10|10|1"
"10|1|01"
```

示例 2：

```text
输入：s = "1001"
输出：0
```

示例 3：

```text
输入：s = "0000"
输出：3
解释：总共有 3 种分割 s 的方法。
"0|0|00"
"0|00|0"
"00|0|0"
```

示例 4：

```text
输入：s = "100100010100110"
输出：12
```

__提示：__

- `s[i] == '0'` 或者 `s[i] == '1'`
- `3 <= s.length <= 10 ^ 5`

__思路:__

```text
模拟
统计 1 的位置信息
如果 1 的个数除 3 有余数, 返回 0
如果没有 1, 那么从任何一个地方划分都可以, 返回 ((n - 1) * (n - 2) >> 1) % mod, 其中 n = len(ones), 即 1 的个数, mod = 1e9 + 7
否则将所有的 1 分成了 3 个部分, 能够移动的空间为, 三等分点的两端区域, 将这两段长度相乘即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numWays(string s) 
    {
        long MOD = 1e9 + 7, n = s.size();
        vector<long> ones;
        for (int i = 0; i < n; i++) if (s[i] == '1') ones.emplace_back(i);
        return ones.size() % 3 ? 0 : (!ones.size() ? ((n - 1) * (n - 2) >> 1) % MOD : (ones[ones.size() / 3] - ones[ones.size() / 3 - 1]) * (ones[ones.size() / 3 << 1] - ones[(ones.size() / 3 << 1) - 1]) % MOD);
    }
};
```

__Java__:

```Java
class Solution {
    public int numWays(String s) {
        long MOD = 1_000_000_007L, n = s.length();
        List<Integer> ones = new ArrayList<>();
        for (int i = 0; i < n; i++) if (s.charAt(i) == '1') ones.add(i);
        return ones.size() % 3 != 0 ? 0 : (ones.size() == 0 ? (int)(((n - 1) * (n - 2) >> 1) % MOD) : (int)((long)(ones.get(ones.size() / 3) - ones.get(ones.size() / 3 - 1)) * (long)(ones.get(ones.size() / 3 << 1) - ones.get((ones.size() / 3 << 1) - 1)) % MOD));
    }
}
```

__Python__:

```Python
class Solution:
    def numWays(self, s: str) -> int:
        return 0 if (n := len(ones := [i for i, v in enumerate(s) if v == '1'])) % 3 else sum(range(len(s) - 1)) % (10 ** 9 + 7) if not n else ((ones[a := n // 3] - ones[a - 1]) * (ones[b := a << 1] - ones[b - 1])) % (10 ** 9 + 7)
```
