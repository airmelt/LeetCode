# 1525 Number of Good Ways to Split a String 字符串的好分割数目

__Description:__

You are given a string `s`.

A split is called __good__ if you can split `s` into two non-empty strings `sleft` and `sright` where their concatenation is equal to `s` (i.e., `sleft + sright = s`) and the number of distinct letters in `sleft` and `sright` is the same.

Return _the number of __good splits__ you can make in `s`_.

__Example:__

Example 1:

```text
__Input:__ s = "aacaba"
__Output:__ 2
__Explanation:__ There are 5 ways to split `"aacaba"` and 2 of them are good. 
("a", "acaba") Left string and right string contains 1 and 3 different letters respectively.
("aa", "caba") Left string and right string contains 1 and 3 different letters respectively.
("aac", "aba") Left string and right string contains 2 and 2 different letters respectively (good split).
("aaca", "ba") Left string and right string contains 2 and 2 different letters respectively (good split).
("aacab", "a") Left string and right string contains 3 and 1 different letters respectively.
```

Example 2:

```text
Input: s = "abcd"
Output: 1
Explanation: Split the string as follows ("ab", "cd").
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists of only lowercase English letters.

__题目描述:__

给你一个字符串 `s` ，一个分割被称为 「好分割」 当它满足：将 `s` 分割成 2 个字符串 `p` 和 `q` ，它们连接起来等于 `s` 且 `p` 和 `q` 中不同字符的数目相同。

请你返回 `s` 中好分割的数目。

__示例:__

示例 1：

```text
输入：s = "aacaba"
输出：2
解释：总共有 5 种分割字符串 "aacaba" 的方法，其中 2 种是好分割。
("a", "acaba") 左边字符串和右边字符串分别包含 1 个和 3 个不同的字符。
("aa", "caba") 左边字符串和右边字符串分别包含 1 个和 3 个不同的字符。
("aac", "aba") 左边字符串和右边字符串分别包含 2 个和 2 个不同的字符。这是一个好分割。
("aaca", "ba") 左边字符串和右边字符串分别包含 2 个和 2 个不同的字符。这是一个好分割。
("aacab", "a") 左边字符串和右边字符串分别包含 3 个和 1 个不同的字符。
```

示例 2：

```text
输入：s = "abcd"
输出：1
解释：好分割为将字符串分割成 ("ab", "cd") 。
```

示例 3：

```text
输入：s = "aaaaa"
输出：4
解释：所有分割都是好分割。
```

示例 4：

```text
输入：s = "acbadbaada"
输出：2
```

__提示：__

- `s` 只包含小写英文字母。
- `1 <= s.length <= 10 ^ 5`

__思路:__

```text
模拟
分割之后的两个字符串都需要非空, 且其字符串种类应该相等
我们用两个数组记录出现过的字符的种类及出现次数
用两个变量分别记录两个数组中非零元素出现的次数
累计两个变量的相等次数即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numSplits(string s) 
    {
        int count = 0;
        vector<int> left(26), right(26);
        for (const auto& c: s) if (!right[c - 'a']++) ++count;
        int result = 0, l = 0, r = count;
        for (const auto& c: s) 
        {
            if (!left[c - 'a']++) ++l;
            if (!--right[c - 'a']) --r;
            result += l == r;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numSplits(String s) {
        int count = 0, left[] = new int[26], right[] = new int[26];
        for (char c: s.toCharArray()) if (right[c - 'a']++ == 0) ++count;
        int result = 0, l = 0, r = count;
        for (char c: s.toCharArray()) {
            if (left[c - 'a']++ == 0) ++l;
            if (--right[c - 'a'] == 0) --r;
            if (l == r) ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numSplits(self, s: str) -> int:
        left, count, result, l = defaultdict(int), (r := len((right := Counter(s)).values())), 0, 0
        for c in s:
            if c not in left:
                l += 1
            left[c] += 1
            right[c] -= 1
            if c in right and not right[c]:
                r -= 1
            result += l == r
        return result
```
