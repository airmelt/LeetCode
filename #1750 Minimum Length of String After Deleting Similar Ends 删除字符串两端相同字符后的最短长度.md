# 1750 Minimum Length of String After Deleting Similar Ends 删除字符串两端相同字符后的最短长度

__Description:__

Given a string `s` consisting only of characters `'a'`, `'b'`, and `'c'`. You are asked to apply the following algorithm on the string any number of times:

Return _the __minimum length__ of_ `s` _after performing the above operation any number of times (possibly zero times)_.

__Example:__

Example 1:

```text
Input: s = "ca"
Output: 2
Explanation: You can't remove any characters, so the string stays as is.
```

Example 2:

```text
Input: s = "cabaabac"
Output: 0
Explanation: An optimal sequence of operations is:
- Take prefix = "c" and suffix = "c" and remove them, s = "abaaba".
- Take prefix = "a" and suffix = "a" and remove them, s = "baab".
- Take prefix = "b" and suffix = "b" and remove them, s = "aa".
- Take prefix = "a" and suffix = "a" and remove them, s = "".
```

Example 3:

```text
Input: s = "aabccabba"
Output: 3
Explanation: An optimal sequence of operations is:
- Take prefix = "aa" and suffix = "a" and remove them, s = "bccabb".
- Take prefix = "b" and suffix = "bb" and remove them, s = "cca".
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` only consists of characters `'a'`, `'b'`, and `'c'`.

__题目描述:__

给你一个只包含字符 `'a'`， `'b'` 和 `'c'` 的字符串 `s` ，你可以执行下面这个操作（5 个步骤）任意次:

请你返回对字符串 `s` 执行上面操作任意次以后（可能 0 次），能得到的 __最短长度__ 。

__示例:__

示例 1：

```text
输入：s = "ca"
输出：2
解释：你没法删除任何一个字符，所以字符串长度仍然保持不变。
```

示例 2：

```text
输入：s = "cabaabac"
输出：0
解释：最优操作序列为：
- 选择前缀 "c" 和后缀 "c" 并删除它们，得到 s = "abaaba" 。
- 选择前缀 "a" 和后缀 "a" 并删除它们，得到 s = "baab" 。
- 选择前缀 "b" 和后缀 "b" 并删除它们，得到 s = "aa" 。
- 选择前缀 "a" 和后缀 "a" 并删除它们，得到 s = "" 。
```

示例 3：

```text
输入：s = "aabccabba"
输出：3
解释：最优操作序列为：
- 选择前缀 "aa" 和后缀 "a" 并删除它们，得到 s = "bccabb" 。
- 选择前缀 "b" 和后缀 "bb" 并删除它们，得到 s = "cca" 。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 只包含字符 `'a'`， `'b'` 和 `'c'` 。

__思路:__

```text
双指针 ➕ 贪心
从首尾两端开始遍历, 如果两端相同则进入循环
如果左边界和右边界相同, 则左边界右移, 直到左边界和右边界不同
如果右边界和左边界的左边相同, 则右边界左移, 直到左边界和右边界不同
注意两次循环的边界条件
最后返回区间长度即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumLength(string s) 
    {
        int l = 0, r = s.size() - 1;
        while (l < r and s[l] == s[r])
        {
            while (l <= r and s[l] == s[r]) ++l;
            while (l < r and s[l - 1] == s[r]) --r;
        }
        return r - l + 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumLength(String s) {
        int l = 0, r = s.length() - 1;
        while (l < r && s.charAt(l) == s.charAt(r)) {
            while (l <= r && s.charAt(l) == s.charAt(r)) ++l;
            while (l < r && s.charAt(l - 1) == s.charAt(r)) --r;
        }
        return r - l + 1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumLength(self, s: str) -> int:
        while len(s) > 1 and s[0] == s[-1]: 
            s = s.strip(s[0])
        return len(s)
```
