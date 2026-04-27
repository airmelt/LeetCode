# 1616 Split Two Strings to Make Palindrome 分割两个字符串得到回文串

__Description:__

You are given two strings `a` and `b` of the same length. Choose an index and split both strings __at the same index__, splitting `a` into two strings: `aprefix` and `asuffix` where `a = aprefix + asuffix`, and splitting `b` into two strings: `bprefix` and `bsuffix` where `b = bprefix + bsuffix`. Check if `aprefix + bsuffix` or `bprefix + asuffix` forms a palindrome.

When you split a string `s` into `sprefix` and `ssuffix`, either `ssuffix` or `sprefix` is allowed to be empty. For example, if `s = "abc"`, then `"" + "abc"`, `"a" + "bc"`, `"ab" + "c"` , and `"abc" + ""` are valid splits.

Return `true` _if it is possible to form_ _a palindrome string, otherwise return_ `false`.

__Notice__ that `x + y` denotes the concatenation of strings `x` and `y`.

__Example:__

Example 1:

```text
Input: a = "x", b = "y"
Output: true
Explaination: If either a or b are palindromes the answer is true since you can split in the following way:
aprefix = "", asuffix = "x"
bprefix = "", bsuffix = "y"
Then, aprefix + bsuffix = "" + "y" = "y", which is a palindrome.
```

Example 2:

```text
Input: a = "xbdef", b = "xecab"
Output: false
```

Example 3:

```text
Input: a = "ulacfd", b = "jizalu"
Output: true
Explaination: Split them at index 3:
aprefix = "ula", asuffix = "cfd"
bprefix = "jiz", bsuffix = "alu"
Then, aprefix + bsuffix = "ula" + "alu" = "ulaalu", which is a palindrome.
```

__Constraints:__

- `1 <= a.length, b.length <= 10 ^ 5`
- `a.length == b.length`
- `a` and `b` consist of lowercase English letters

__题目描述:__

给你两个字符串 `a` 和 `b` ，它们长度相同。请你选择一个下标，将两个字符串都在 __相同的下标__ 分割开。由 `a` 可以得到两个字符串: `aprefix` 和 `asuffix` ，满足 `a = aprefix + asuffix` ，同理，由 `b` 可以得到两个字符串 `bprefix` 和 `bsuffix` ，满足 `b = bprefix + bsuffix` 。请你判断 `aprefix + bsuffix` 或者 `bprefix + asuffix` 能否构成回文串。

当你将一个字符串 `s` 分割成 `sprefix` 和 `ssuffix` 时， `ssuffix` 或者 `sprefix` 可以为空。比方说， `s = "abc"` 那么 `"" + "abc"` ， `"a" + "bc"` ， `"ab" + "c"` 和 `"abc" + ""` 都是合法分割。

如果 __能构成回文字符串__ ，那么请返回 `true`，否则返回 `false` 。

__注意__， `x + y` 表示连接字符串 `x` 和 `y` 。

__示例:__

示例 1：

```text
输入：a = "x", b = "y"
输出：true
解释：如果 a 或者 b 是回文串，那么答案一定为 true ，因为你可以如下分割：
aprefix = "", asuffix = "x"
bprefix = "", bsuffix = "y"
那么 aprefix + bsuffix = "" + "y" = "y" 是回文串。
```

示例 2：

```text
输入：a = "abdef", b = "fecab"
输出：true
```

示例 3：

```text
输入：a = "ulacfd", b = "jizalu"
输出：true
解释：在下标为 3 处分割：
aprefix = "ula", asuffix = "cfd"
bprefix = "jiz", bsuffix = "alu"
那么 aprefix + bsuffix = "ula" + "alu" = "ulaalu" 是回文串。
```

__提示：__

- `1 <= a.length, b.length <= 10 ^ 5`
- `a.length == b.length`
- `a` 和 `b` 都只包含小写英文字母

__思路:__

```text
双指针
首先检查 a 的后缀和 b 的前缀以及 b 的后缀和 a 的前缀是否能组成回文串
最后判断 a, b 自身是否为回文串
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    bool helper(const string& a, const string& b)
    {
        int n = a.size(), left = 0, right = n - 1;
        while (left < right and a[left] == b[right])
        {
            ++left;
            --right;
        }
        if (left >= right) return true;
        return helper(a, left, right) or helper(b, left, right);
    }
    bool helper(const string& a, int left, int right)
    {
        while (left < right and a[left] == a[right])
        {
            ++left;
            --right;
        }
        return left >= right;
    }
public:
    bool checkPalindromeFormation(string a, string b) 
    {
        return helper(a, b) or helper(b, a);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkPalindromeFormation(String a, String b) {
        return helper(a, b) || helper(b, a);
    }
    
    private boolean helper(String a, String b) {
        int n = a.length(), left = 0, right = n - 1;
        while (left < right && a.charAt(left) == b.charAt(right)) {
            ++left;
            --right;
        }
        if (left >= right) return true;
        return helper(a, left, right) || helper(b, left, right);
    }
    
    private boolean helper(String a, int left, int right) {
        while (left < right && a.charAt(left) == a.charAt(right)) {
            ++left;
            --right;
        }
        return left >= right;
    }
}
```

__Python__:

```Python
class Solution:
    def checkPalindromeFormation(self, a: str, b: str) -> bool:
        def check(s: str, left: int, right: int) -> bool:
            while left < right and s[left] == s[right]:
                left += 1
                right -= 1
            return left >= right

        def helper(s1: str, s2: str) -> bool:
            left, right = 0, (n := len(a)) - 1
            while left < right and s1[left] == s2[right]:
                left += 1
                right -= 1
            if left >= right:
                return True
            return check(s1, left, right) or check(s2, left, right)
        return helper(a, b) or helper(b, a)
```
