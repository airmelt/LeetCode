# 1869 Longer Contiguous Segments of Ones than Zeros 哪种连续子字符串更长

__Description:__

Given a binary string `s`, return `true` _if the __longest__ contiguous segment of_ `1`'_s is __strictly longer__ than the __longest__ contiguous segment of_ `0`'_s in_ `s`, or return `false` _otherwise_.

- For example, in `s = "110100010"` the longest continuous segment of `1`s has length `2`, and the longest continuous segment of `0`s has length `3`.

Note that if there are no `0`'s, then the longest continuous segment of `0`'s is considered to have a length `0`. The same applies if there is no `1`'s.

__Example:__

Example 1:

```text
Input: s = "1101"
Output: true
Explanation:
The longest contiguous segment of 1s has length 2: "1101"
The longest contiguous segment of 0s has length 1: "1101"
The segment of 1s is longer, so return true.
```

Example 2:

```text
Input: s = "111000"
Output: false
Explanation:
The longest contiguous segment of 1s has length 3: "111000"
The longest contiguous segment of 0s has length 3: "111000"
The segment of 1s is not longer, so return false.
```

Example 3:

```text
Input: s = "110100010"
Output: false
Explanation:
The longest contiguous segment of 1s has length 2: "110100010"
The longest contiguous segment of 0s has length 3: "110100010"
The segment of 1s is not longer, so return false.
```

__Constraints:__

- `1 <= s.length <= 100`
- `s[i]` is either `'0'` or `'1'`.

__题目描述:__

给你一个二进制字符串 `s` 。如果字符串中由 `1` 组成的 __最长__ 连续子字符串 __严格长于__ 由 `0` 组成的 __最长__ 连续子字符串，返回 `true` ；否则，返回 `false` 。

- 例如， `s = "__11__01__000__10"` 中，由 `1` 组成的最长连续子字符串的长度是 `2` ，由 `0` 组成的最长连续子字符串的长度是 `3` 。

注意，如果字符串中不存在 `0` ，此时认为由 `0` 组成的最长连续子字符串的长度是 `0` 。字符串中不存在 `1` 的情况也适用此规则。

__示例:__

示例 1：

```text
输入: s = "1101"
输出: true
解释: 
由 `1` 组成的最长连续子字符串的长度是 2:"1101"
由 `0` 组成的最长连续子字符串的长度是 1:"1101"
由 1 组成的子字符串更长，故返回 true 。
```

示例 2：

```text
输入: s = "111000"
输出: false
解释: 
由 `1` 组成的最长连续子字符串的长度是 3:"111000"
由 `0` 组成的最长连续子字符串的长度是 3:"111000"
由 1 组成的子字符串不比由 0 组成的子字符串长，故返回 false 。
```

示例 3：

```text
输入: s = "110100010"
输出: false
解释: 
由 `1` 组成的最长连续子字符串的长度是 2:"110100010"
由 `0` 组成的最长连续子字符串的长度是 3:"110100010"
由 1 组成的子字符串不比由 0 组成的子字符串长，故返回 false 。
```

__提示：__

- `1 <= s.length <= 100`
- `s[i]` 不是 `'0'` 就是 `'1'`

__思路:__

```text
模拟
用两个变量 cur_zero, cur_one 分别统计连续的 '0' 和 '1'
如果当前为 '0', cur_zero 自增, cur_one 清零
如果当前为 '1', cur_one 自增, cur_zero 清零
求出累计最大的 zero 和 one
比较这两个的大小即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool checkZeroOnes(string s) 
    {
        int one = 0, zero = 0, cur_one = 0, cue_zero = 0;
        for (const auto& c : s)
        {
            if (c == '1') 
            {
                ++cur_one;
                cue_zero = 0;
            } 
            else 
            {
                ++cue_zero;
                cur_one = 0;
            }
            one = max(one, cur_one);
            zero = max(zero, cue_zero);
        }
        return one > zero;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkZeroOnes(String s) {
        int one = 0, zero = 0, curOne = 0, curZero = 0;
        for (char c : s.toCharArray()) {
            if (c == '1') {
                ++curOne;
                curZero = 0;
            } else {
                ++curZero;
                curOne = 0;
            }
            one = Math.max(one, curOne);
            zero = Math.max(zero, curZero);
        }
        return one > zero;
    }
}
```

__Python__:

```Python
class Solution:
    def checkZeroOnes(self, s: str) -> bool:
        return '0' not in s or ('1' in s and max(len(c) for c in s.split('0')) > max(len(c) for c in s.split('1')))
```
