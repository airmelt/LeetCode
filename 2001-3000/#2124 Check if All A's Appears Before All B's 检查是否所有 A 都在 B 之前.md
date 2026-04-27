# 2124 Check if All A's Appears Before All B's 检查是否所有 A 都在 B 之前

__Description:__

Given a string `s` consisting of __only__ the characters `'a'` and `'b'`, return `true` _if __every___ `'a'` _appears before __every___ `'b'` _in the string_. Otherwise, return `false`.

__Example:__

Example 1:

```text
Input: s = "aaabbb"
Output: true
Explanation:
The 'a's are at indices 0, 1, and 2, while the 'b's are at indices 3, 4, and 5.
Hence, every 'a' appears before every 'b' and we return true.
```

Example 2:

```text
Input: s = "abab"
Output: false
Explanation:
There is an 'a' at index 2 and a 'b' at index 1.
Hence, not every 'a' appears before every 'b' and we return false.
```

Example 3:

```text
Input: s = "bbb"
Output: true
Explanation:
There are no 'a's, hence, every 'a' appears before every 'b' and we return true.
```

__Constraints:__

- `1 <= s.length <= 100`
- `s[i]` is either `'a'` or `'b'`.

__题目描述:__

给你一个 __仅__ 由字符 `'a'` 和 `'b'` 组成的字符串  `s` 。如果字符串中 __每个__  `'a'` 都出现在 __每个__ `'b'` 之前，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：s = "aaabbb"
输出：true
解释：
'a' 位于下标 0、1 和 2 ；而 'b' 位于下标 3、4 和 5 。
因此，每个 'a' 都出现在每个 'b' 之前，所以返回 true 。
```

示例 2：

```text
输入：s = "abab"
输出：false
解释：
存在一个 'a' 位于下标 2 ，而一个 'b' 位于下标 1 。
因此，不能满足每个 'a' 都出现在每个 'b' 之前，所以返回 false 。
```

示例 3：

```text
输入：s = "bbb"
输出：true
解释：
不存在 'a' ，因此可以视作每个 'a' 都出现在每个 'b' 之前，所以返回 true 。
```

__提示：__

- `1 <= s.length <= 100`
- `s[i]` 为 `'a'` 或 `'b'`

__思路:__

```text
模拟
由于字符串中只有 'a' 和 'b'
为使得 'b' 在 'a' 布之后
只需要不出现 "ba" 即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool checkString(string s) 
    {
        return s.find("ba") == string::npos;    
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkString(String s) {
        return s.indexOf("ba") == -1;
    }
}
```

__Python__:

```Python
class Solution:
    def checkString(self, s: str) -> bool:
        return s.find('ba') == -1
```
