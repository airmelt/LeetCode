# 1784 Check if Binary String Has at Most One Segment of Ones 检查二进制字符串字段

__Description:__

Given a binary string `s` __​​​​​without leading zeros__, return `true`​​​ _if_ `s` _contains __at most one contiguous segment of ones___. Otherwise, return `false`.

__Example:__

Example 1:

```text
Input: s = "1001"
Output: false
Explanation: The ones do not form a contiguous segment.
```

Example 2:

```text
Input: s = "110"
Output: true
```

__Constraints:__

- `1 <= s.length <= 100`
- `s[i]`​​​​ is either `'0'` or `'1'`.
- `s[0]` is `'1'`.

__题目描述:__

给你一个二进制字符串 `s` ，该字符串 __不含前导零__ 。

如果 `s` 包含 __零个或一个由连续的 `'1'` 组成的字段__ ，返回 `true`​​​ 。否则，返回 `false` 。

__示例:__

示例 1：

```text
输入: s = "1001"
输出: false
解释: 由连续若干个 `'1'` 组成的字段数量为 2，返回 false
```

示例 2：

```text
输入：s = "110"
输出：true
```

__提示：__

- `1 <= s.length <= 100`
- `s[i]`​​​​ 为 `'0'` 或 `'1'`
- `s[0]` 为 `'1'`

__思路:__

```text
模拟
题目的英文是问最多只有 1 个连续的 '1' 的子串
由于字符串没有前导 0 , 也就是问有没有 '01' 这个子串
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool checkOnesSegment(string s) 
    {
        return s.find("01") == string::npos;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkOnesSegment(String s) {
        return  s.split("0").length == 1;
    }
}
```

__Python__:

```Python
class Solution:
    def checkOnesSegment(self, s: str) -> bool:
        return not '01' in s
```
