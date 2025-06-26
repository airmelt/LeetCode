# 2575 Find the Divisibility Array of a String 找出字符串的可整除数组

__Description:__

You are given a __0-indexed__ string `word` of length `n` consisting of digits, and a positive integer `m`.

The __divisibility array__ `div` of `word` is an integer array of length `n` such that:

- `div[i] = 1` if the __numeric value__ of `word[0,...,i]` is divisible by `m`, or
- `div[i] = 0` otherwise.

Return _the divisibility array of_ `word`.

__Example:__

Example 1:

```text
Input: word = "998244353", m = 3
Output: [1,1,0,0,0,1,1,0,0]
Explanation: There are only 4 prefixes that are divisible by 3: "9", "99", "998244", and "9982443".
```

Example 2:

```text
Input: word = "1010", m = 10
Output: [0,1,0,1]
Explanation: There are only 2 prefixes that are divisible by 10: "10", and "1010".
```

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `word.length == n`
- `word` consists of digits from `0` to `9`
- `1 <= m <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `word` ，长度为 `n` ，由从 `0` 到 `9` 的数字组成。另给你一个正整数 `m` 。

`word` 的 __可整除数组__ `div` 是一个长度为 `n` 的整数数组，并满足:

- 如果 `word[0,...,i]` 所表示的 __数值__ 能被 `m` 整除， `div[i] = 1`
- 否则， `div[i] = 0`

返回 `word` 的可整除数组。

__示例:__

示例 1：

```text
输入：word = "998244353", m = 3
输出：[1,1,0,0,0,1,1,0,0]
解释：仅有 4 个前缀可以被 3 整除："9"、"99"、"998244" 和 "9982443" 。
```

示例 2：

```text
输入：word = "1010", m = 10
输出：[0,1,0,1]
解释：仅有 2 个前缀可以被 10 整除："10" 和 "1010" 。
```

__提示：__

- `1 <= n <= 10 ^ 5`
- `word.length == n`
- `word` 由数字 `0` 到 `9` 组成
- `1 <= m <= 10 ^ 9`

__思路:__

```text
数学
初始化 cur = 0
对每个字符 c, 将当前的数值 cur 更新为 (cur * 10 + c - '0') % m
如果 cur == 0, 则说明当前的数值能被 m 整除
将结果数组的对应位置设为 1
否则设为 0
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> divisibilityArray(string word, int m) 
    {
        int n = word.size();
        vector<int> result(n);
        long long cur = 0LL;
        for (int i = 0; i < n; i++) if (!(cur = (10LL * cur + word[i] - '0') % m)) result[i] = 1;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] divisibilityArray(String word, int m) {
        int n = word.length(), result[] = new int[n];
        long cur = 0L;
        for (int i = 0; i < n; i++) if ((cur = (10L * cur + word.charAt(i) - '0') % m) == 0) result[i] = 1;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def divisibilityArray(self, word: str, m: int) -> List[int]:
        result, cur = [0] * len(word), 0
        for i, c in enumerate(word):
            result[i] = int(not (cur := (cur * 10 + int(c)) % m))
        return result
```
