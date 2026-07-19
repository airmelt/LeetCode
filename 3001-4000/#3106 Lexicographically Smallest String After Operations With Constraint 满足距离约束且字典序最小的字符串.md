# 3106 Lexicographically Smallest String After Operations With Constraint 满足距离约束且字典序最小的字符串

__Description:__

You are given a string `s` and an integer `k`.

Define a function `distance(s1, s2)` between two strings `s1` and `s2` of the same length `n` as:

- The __sum__ of the __minimum distance__ between `s1[i]` and `s2[i]` when the characters from `'a'` to `'z'` are placed in a __cyclic__ order, for all `i` in the range `[0, n - 1]`.

For example, `distance("ab", "cd") == 4`, and `distance("a", "z") == 1`.

You can __change__ any letter of `s` to __any__ other lowercase English letter, __any__ number of times.

Return a string denoting the __lexicographically smallest__ string `t` you can get after some changes, such that `distance(s, t) <= k`.

__Example:__

Example 1:

```text
Input: s = "zbbz", k = 3

Output: "aaaz"

Explanation:
```

Change `s` to `"aaaz"`. The distance between `"zbbz"` and `"aaaz"` is equal to `k = 3`.

Example 2:

```text
Input: s = "xaxcd", k = 4

Output: "aawcd"

Explanation:
```

The distance between "xaxcd" and "aawcd" is equal to k = 4.

Example 3:

```text
Input: s = "lol", k = 0

Output: "lol"

Explanation:
```

It's impossible to change any character as `k = 0`.

__Constraints:__

- `1 <= s.length <= 100`
- `0 <= k <= 2000`
- `s` consists only of lowercase English letters.

__题目描述:__

给你一个字符串 `s` 和一个整数 `k` 。

定义函数 `distance(s1, s2)` ，用于衡量两个长度为 `n` 的字符串 `s1` 和 `s2` 之间的距离，即:

- 字符 `'a'` 到 `'z'` 按 __循环__ 顺序排列，对于区间 `[0, n - 1]` 中的 `i` ，计算所有「 `s1[i]` 和 `s2[i]` 之间 __最小距离__」的 __和__ 。

例如， `distance("ab", "cd") == 4` ，且 `distance("a", "z") == 1` 。

你可以对字符串 `s` 执行 __任意次__ 操作。在每次操作中，可以将 `s` 中的一个字母 __改变__ 为 __任意__ 其他小写英文字母。

返回一个字符串，表示在执行一些操作后你可以得到的 __字典序最小__ 的字符串 `t` ，且满足 `distance(s, t) <= k` 。

__示例:__

示例 1：

```text
输入：s = "zbbz", k = 3
输出："aaaz"
解释：在这个例子中，可以执行以下操作：
将 s[0] 改为 'a' ，s 变为 "abbz" 。
将 s[1] 改为 'a' ，s 变为 "aabz" 。
将 s[2] 改为 'a' ，s 变为 "aaaz" 。
"zbbz" 和 "aaaz" 之间的距离等于 k = 3 。
可以证明 "aaaz" 是在任意次操作后能够得到的字典序最小的字符串。
因此，答案是 "aaaz" 。
```

示例 2：

```text
输入：s = "xaxcd", k = 4
输出："aawcd"
解释：在这个例子中，可以执行以下操作：
将 s[0] 改为 'a' ，s 变为 "aaxcd" 。
将 s[2] 改为 'w' ，s 变为 "aawcd" 。
"xaxcd" 和 "aawcd" 之间的距离等于 k = 4 。
可以证明 "aawcd" 是在任意次操作后能够得到的字典序最小的字符串。
因此，答案是 "aawcd" 。
```

示例 3：

```text
输入：s = "lol", k = 0
输出："lol"
解释：在这个例子中，k = 0，更改任何字符都会使得距离大于 0 。
因此，答案是 "lol" 。
```

__提示：__

- `1 <= s.length <= 100`
- `0 <= k <= 2000`
- `s` 只包含小写英文字母。

__思路:__

```text
贪心
如果能把当前字符改为 'a' 就改为 'a'
否则那一定是 k 不够
这时只需要把相应字符减少 k 即可
因为不能靠跨过 'z' 到 'a'
只能减少
k 每次减少 min(c - 'a', 'z' + 1 - c)
要么从正面减少到 'a', 要么跨过 'z' 到 'a'
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string getSmallestString(string s, int k) 
    {
        for (auto& c : s) 
        {
            int d = min(c - 'a', 'z' + 1 - c);
            if (d > k) 
            {
                c -= k;
                break;
            }
            c = 'a';
            k -= d;
        }
        return s;
    }
};
```

__Java__:

```Java
class Solution {
    public String getSmallestString(String s, int k) {
        var t = s.toCharArray();
        for (int i = 0, n = t.length, d = 0; i < n; i++) {
            if ((d = Math.min(t[i] - 'a', 'z' - t[i] + 1)) > k) {
                t[i] -= k;
                break;
            }
            t[i] = 'a';
            k -= d;
        }
        return new String(t);
    }
}
```

__Python__:

```Python
class Solution:
    def getSmallestString(self, s: str, k: int) -> str:
        s = list(s)
        for i, c in enumerate(map(ord, s)):
            if (d := min(c - ord('a'), ord('z') + 1 - c)) > k:
                s[i] = chr(c - k)
                break
            s[i] = 'a'
            k -= d
        return ''.join(s)
```
