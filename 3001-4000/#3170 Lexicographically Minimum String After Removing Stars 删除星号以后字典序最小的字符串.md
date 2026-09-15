# 3170 Lexicographically Minimum String After Removing Stars 删除星号以后字典序最小的字符串

__Description:__

You are given a string `s`. It may contain any number of `'*'` characters. Your task is to remove all `'*'` characters.

While there is a `'*'`, do the following operation:

- Delete the leftmost `'*'` and the __smallest__ non-`'*'` character to its _left_. If there are several smallest characters, you can delete any of them.

Return the __lexicographically smallest__ resulting string after removing all `'*'` characters.

__Example:__

Example 1:

```text
Input: s = "aaba*"

Output: "aab"

Explanation:

We should delete one of the 'a' characters with '*'. If we choose s[3], s becomes the lexicographically smallest.
```

Example 2:

```text
Input: s = "abc"

Output: "abc"

Explanation:

There is no '*' in the string.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists only of lowercase English letters and `'*'`.
- The input is generated such that it is possible to delete all `'*'` characters.

__题目描述:__

给你一个字符串 `s` 。它可能包含任意数量的 `'*'` 字符。你的任务是删除所有的 `'*'` 字符。

当字符串还存在至少一个 `'*'` 字符时，你可以执行以下操作：

- 删除最左边的 `'*'` 字符，同时删除该星号字符左边一个字典序 __最小__ 的字符。如果有多个字典序最小的字符，你可以删除它们中的任意一个。

请你返回删除所有 `'*'` 字符以后，剩余字符连接而成的 __字典序最小__ 的字符串。

__示例:__

示例 1：

```text
输入：s = "aaba*"

输出："aab"

解释：

删除 '*' 号和它左边的其中一个 'a' 字符。如果我们选择删除 s[3] ，s 字典序最小。
```

示例 2：

```text
输入：s = "abc"

输出："abc"

解释：

字符串中没有 '*' 字符。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 只含有小写英文字母和 `'*'` 字符。
- 输入保证操作可以删除所有的 `'*'` 字符。

__思路:__

```text
栈
按照贪心的思想
每次遇到 '*' 应该删除最右边的最小的字符
所以用 26 个栈来记录每个字符出现的位置
每次删除的时候可以将待删除字符改为 '*'
时间复杂度为 O(CN), 空间复杂度为 O(N), 其中 C 为字符种类, 本题为 26
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string clearStars(string s) 
    {
        stack<int> stacks[26];
        for (int i = 0, n = s.size(); i < n; i++) 
        {
            if (s[i] != '*') stacks[s[i] - 'a'].push(i);
            else
            {
                for (auto& stack : stacks) 
                {
                    if (!stack.empty()) 
                    {
                        s[stack.top()] = '*';
                        stack.pop();
                        break;
                    }
                }
            }
        }
        erase(s, '*');
        return s;
    }
};
```

__Java__:

```Java
class Solution {
    public String clearStars(String s) {
        char result[] = s.toCharArray();
        List<Integer>[] stacks = new ArrayList[26];
        Arrays.setAll(stacks, stack -> new ArrayList<>());
        for (int i = 0, n = result.length; i < n; i++) {
            if (result[i] != '*') stacks[result[i] - 'a'].add(i);
            else {
                for (var stack : stacks) {
                    if (!stack.isEmpty()) {
                        result[stack.removeLast()] = '*';
                        break;
                    }
                }
            }
        }
        return String.valueOf(result).replaceAll("\\*", "");
    }
}
```

__Python__:

```Python
class Solution:
    def clearStars(self, s: str) -> str:
        result, stacks = list(s), [[] for _ in range(26)]
        for i, c in enumerate(s):
            if c != '*':
                stacks[ord(c) - ord('a')].append(i)
            else:
                for stack in stacks:
                    if stack:
                        result[stack.pop()] = '*'
                        break
        return ''.join(c for c in result if c != '*')
```
