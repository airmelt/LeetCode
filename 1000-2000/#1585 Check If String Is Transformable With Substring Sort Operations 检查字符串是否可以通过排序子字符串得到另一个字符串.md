# 1585 Check If String Is Transformable With Substring Sort Operations 检查字符串是否可以通过排序子字符串得到另一个字符串

__Description:__

Given two strings `s` and `t`, transform string `s` into string `t` using the following operation any number of times:

- Choose a __non-empty__ substring in `s` and sort it in place so the characters are in __ascending order__.

- For example, applying the operation on the underlined substring in "1 __4234__" results in "1 __2344__".

Return `true` if _it is possible to transform `s` into `t`_. Otherwise, return `false`.

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "84532", t = "34852"
Output: true
Explanation: You can transform s into t using the following sort operations:
"84532" (from index 2 to 3) -> "84352"
"84352" (from index 0 to 2) -> "34852"
```

Example 2:

```text
Input: s = "34521", t = "23415"
Output: true
Explanation: You can transform s into t using the following sort operations:
"34521" -> "23451"
"23451" -> "23415"
```

Example 3:

```text
Input: s = "12345", t = "12435"
Output: false
```

__Constraints:__

- `s.length == t.length`
- `1 <= s.length <= 10 ^ 5`
- `s` and `t` consist of only digits.

__题目描述:__

给你两个字符串 `s` 和 `t` ，请你通过若干次以下操作将字符串 `s` 转化成字符串 `t` :

- 选择 `s` 中一个 __非空__ 子字符串并将它包含的字符就地 __升序__ 排序。

比方说，对下划线所示的子字符串进行操作可以由 "1 __4234__" 得到 "1 __2344__" 。

如果可以将字符串 `s` 变成 `t` ，返回 `true` 。否则，返回 `false` 。

一个 子字符串 定义为一个字符串中连续的若干字符。

__示例:__

示例 1：

```text
输入：s = "84532", t = "34852"
输出：true
解释：你可以按以下操作将 s 转变为 t ：
"84532" （从下标 2 到下标 3）-> "84352"
"84352" （从下标 0 到下标 2） -> "34852"
```

示例 2：

```text
输入：s = "34521", t = "23415"
输出：true
解释：你可以按以下操作将 s 转变为 t ：
"34521" -> "23451"
"23451" -> "23415"
```

示例 3：

```text
输入：s = "12345", t = "12435"
输出：false
```

示例 4：

```text
输入：s = "1", t = "2"
输出：false
```

__提示：__

- `s.length == t.length`
- `1 <= s.length <= 10 ^ 5`
- `s` 和 `t` 都只包含数字字符，即 `'0'` 到 `'9'` 。

__思路:__

```text
冒泡排序
用 10 个队列记录 s 中所有数字出现的位置
遍历字符串 t, 对于 t 中的每一个字符
必须保证 s 中出现过
并且从小到大遍历数字出现的位置都要比 t 的字符更加靠前
判断完成之后将已经匹配的字符删除
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isTransformable(string s, string t) 
    {
        if (s == t) return true;
        int n = s.size();
        vector<queue<int>> pos(10);
        for (int i = 0; i < n; i++) pos[s[i] - '0'].push(i);
        for (int i = 0; i < n; i++)
        {
            int digit = t[i] - '0';
            if (pos[digit].empty()) return false;
            for (int j = 0; j < digit; j++) if (!pos[j].empty() and pos[j].front() < pos[digit].front()) return false;
            pos[digit].pop();
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isTransformable(String s, String t) {
        if (s.equals(t)) return true;
        int n = s.length();
        Queue<Integer>[] pos = new Queue[10];
        for (int i = 0; i < 10; i++) pos[i] = new ArrayDeque<>();
        for (int i = 0; i < n; i++) pos[s.charAt(i) - '0'].add(i);
        for (int i = 0; i < n; i++) {
            int digit = t.charAt(i) - '0';
            if (pos[digit].isEmpty()) return false;
            for (int j = 0; j < digit; j++) if (!pos[j].isEmpty() && pos[j].peek() < pos[digit].peek()) return false;
            pos[digit].poll();
        }
        return true;    
    }
}
```

__Python__:

```Python
class Solution:
    def isTransformable(self, s: str, t: str) -> bool:
        n, pos = len(s), {i: collections.deque() for i in range(10)}
        for i, digit in enumerate(s):
            pos[int(digit)].append(i)
        for i, digit in enumerate(t):
            if not pos[d := int(digit)] or any(pos[j] and pos[j][0] < pos[d][0] for j in range(d)):
                return False
            pos[d].popleft()
        return True
```
