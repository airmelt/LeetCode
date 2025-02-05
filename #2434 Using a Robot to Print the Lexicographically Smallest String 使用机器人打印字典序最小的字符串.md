# 2434 Using a Robot to Print the Lexicographically Smallest String 使用机器人打印字典序最小的字符串

__Description:__

You are given a string `s` and a robot that currently holds an empty string `t`. Apply one of the following operations until `s` and `t` __are both empty__:

- Remove the __first__ character of a string `s` and give it to the robot. The robot will append this character to the string `t`.
- Remove the __last__ character of a string `t` and give it to the robot. The robot will write this character on paper.

Return the lexicographically smallest string that can be written on the paper.

__Example:__

Example 1:

```text
Input: s = "zza"
Output: "azz"
Explanation: Let p denote the written string.
Initially p="", s="zza", t="".
Perform first operation three times p="", s="", t="zza".
Perform second operation three times p="azz", s="", t="".
```

Example 2:

```text
Input: s = "bac"
Output: "abc"
Explanation: Let p denote the written string.
Perform first operation twice p="", s="c", t="ba". 
Perform second operation twice p="ab", s="c", t="". 
Perform first operation p="ab", s="", t="c". 
Perform second operation p="abc", s="", t="".
```

Example 3:

```text
Input: s = "bdda"
Output: "addb"
Explanation: Let p denote the written string.
Initially p="", s="bdda", t="".
Perform first operation four times p="", s="", t="bdda".
Perform second operation four times p="addb", s="", t="".
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists of only English lowercase letters.

__题目描述:__

给你一个字符串 `s` 和一个机器人，机器人当前有一个空字符串 `t` 。执行以下操作之一，直到 `s` 和 `t` __都变成空字符串:__

- 删除字符串 `s` 的 __第一个__ 字符，并将该字符给机器人。机器人把这个字符添加到 `t` 的尾部。
- 删除字符串 `t` 的 __最后一个__ 字符，并将该字符给机器人。机器人将该字符写到纸上。

请你返回纸上能写出的字典序最小的字符串。

__示例:__

示例 1：

```text
输入：s = "zza"
输出："azz"
解释：用 p 表示写出来的字符串。
一开始，p="" ，s="zza" ，t="" 。
执行第一个操作三次，得到 p="" ，s="" ，t="zza" 。
执行第二个操作三次，得到 p="azz" ，s="" ，t="" 。
```

示例 2：

```text
输入：s = "bac"
输出："abc"
解释：用 p 表示写出来的字符串。
执行第一个操作两次，得到 p="" ，s="c" ，t="ba" 。
执行第二个操作两次，得到 p="ab" ，s="c" ，t="" 。
执行第一个操作，得到 p="ab" ，s="" ，t="c" 。
执行第二个操作，得到 p="abc" ，s="" ，t="" 。
```

示例 3：

```text
输入：s = "bdda"
输出："addb"
解释：用 p 表示写出来的字符串。
一开始，p="" ，s="bdda" ，t="" 。
执行第一个操作四次，得到 p="" ，s="" ，t="bdda" 。
执行第二个操作四次，得到 p="addb" ，s="" ，t="" 。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 只包含小写英文字母。

__思路:__

```text
栈 ➕ 贪心
先将字符压入栈
若栈顶字符不大于后续字符的最小值
则将栈顶字符弹出并加入结果字符串
为了快速找到后续字符的最小值
可以先统计 s 中每个字符的个数
然后遍历 s 的时候同时更新最小值
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string robotWithString(string s) 
    {
        string result;
        int cnt[26], cur = 0;
        memset(cnt, 0, sizeof(cnt));
        for (const auto& c : s) ++cnt[c - 'a'];
        stack<char> st;
        for (const auto& c : s)
        {
            --cnt[c - 'a'];
            while (cur < 25 and !cnt[cur]) ++cur;
            st.push(c);
            while (!st.empty() and st.top() - 'a' <= cur)
            {
                result += st.top();
                st.pop();
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String robotWithString(String s) {
        var result = new StringBuilder();
        int count[] = new int[26], cur = 0;
        for (char c : s.toCharArray()) ++count[c - 'a'];
        var stack = new Stack<Character>();
        for (char c : s.toCharArray()) {
            --count[c - 'a'];
            while (cur < 25 && count[cur] == 0) ++cur;
            stack.push(c);
            while (!stack.isEmpty() && stack.peek() - 'a' <= cur) result.append(stack.pop());
        }
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def robotWithString(self, s: str) -> str:
        result, stack, f = deque(), deque(), [0] * (n := len(s)) + [ord('z') + 1]
        for i in range(n - 1, -1, -1):
            f[i] = min(f[i + 1], ord(s[i]))
        for i in range(n):
            stack.append(s[i])
            while stack and ord(stack[-1]) <= f[i + 1]:
                result.append(stack.pop())
        return ''.join(result)
```
