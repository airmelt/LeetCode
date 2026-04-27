# 2810 Faulty Keyboard 故障键盘

__Description:__

Your laptop keyboard is faulty, and whenever you type a character `'i'` on it, it reverses the string that you have written. Typing other characters works as expected.

You are given a __0-indexed__ string `s`, and you type each character of `s` using your faulty keyboard.

Return the final string that will be present on your laptop screen.

__Example:__

Example 1:

```text
Input: s = "string"
Output: "rtsng"
Explanation: 
After typing first character, the text on the screen is "s".
After the second character, the text is "st". 
After the third character, the text is "str".
Since the fourth character is an 'i', the text gets reversed and becomes "rts".
After the fifth character, the text is "rtsn". 
After the sixth character, the text is "rtsng". 
Therefore, we return "rtsng".
```

Example 2:

```text
Input: s = "poiinter"
Output: "ponter"
Explanation: 
After the first character, the text on the screen is "p".
After the second character, the text is "po". 
Since the third character you type is an 'i', the text gets reversed and becomes "op". 
Since the fourth character you type is an 'i', the text gets reversed and becomes "po".
After the fifth character, the text is "pon".
After the sixth character, the text is "pont". 
After the seventh character, the text is "ponte". 
After the eighth character, the text is "ponter". 
Therefore, we return "ponter".
```

__Constraints:__

- `1 <= s.length <= 100`
- `s` consists of lowercase English letters.
- `s[0] != 'i'`

__题目描述:__

你的笔记本键盘存在故障，每当你在上面输入字符 `'i'` 时，它会反转你所写的字符串。而输入其他字符则可以正常工作。

给你一个下标从 __0__ 开始的字符串 `s` ，请你用故障键盘依次输入每个字符。

返回最终笔记本屏幕上输出的字符串。

__示例:__

示例 1：

```text
输入：s = "string"
输出："rtsng"
解释：
输入第 1 个字符后，屏幕上的文本是："s" 。
输入第 2 个字符后，屏幕上的文本是："st" 。
输入第 3 个字符后，屏幕上的文本是："str" 。
因为第 4 个字符是 'i' ，屏幕上的文本被反转，变成 "rts" 。
输入第 5 个字符后，屏幕上的文本是："rtsn" 。
输入第 6 个字符后，屏幕上的文本是： "rtsng" 。
因此，返回 "rtsng" 。
```

示例 2：

```text
输入：s = "poiinter"
输出："ponter"
解释：
输入第 1 个字符后，屏幕上的文本是："p" 。
输入第 2 个字符后，屏幕上的文本是："po" 。
因为第 3 个字符是 'i' ，屏幕上的文本被反转，变成 "op" 。
因为第 4 个字符是 'i' ，屏幕上的文本被反转，变成 "po" 。
输入第 5 个字符后，屏幕上的文本是："pon" 。
输入第 6 个字符后，屏幕上的文本是："pont" 。
输入第 7 个字符后，屏幕上的文本是："ponte" 。
输入第 8 个字符后，屏幕上的文本是："ponter" 。
因此，返回 "ponter" 。
```

__提示：__

- `1 <= s.length <= 100`
- `s` 由小写英文字母组成
- `s[0] != 'i'`

__思路:__

```text
1. 模拟
每次遇到 i 就反转当前结果
其他字符直接加入结果
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 双端队列
每次遇到 i 将标志反转
标志为 true 从后端添加
否则从前端添加
最后根据标志反转整个字符串
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string finalString(string s) 
    {
        deque<char> q;
        bool flag = true;
        for (const auto& c : s) 
        {
            if (c == 'i') flag = !flag; 
            else if (flag) q.push_back(c); 
            else q.push_front(c);
        }
        return flag ? string(q.begin(), q.end()) : string(q.rbegin(), q.rend());
    }
};
```

__Java__:

```Java
class Solution {
    public String finalString(String s) {
        var result = new StringBuilder();
        for (var c : s.toCharArray()) {
            if (c == 'i') result.reverse();
            else result.append(c);
        }
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def finalString(self, s: str) -> str:
        return "" if not (s := s.split("i")) else ("".join(s[1::2]))[::-1] + ("".join(s[::2])) if len(s) & 1 else ("".join(s[::2]))[::-1] + ("".join(s[1::2]))
```
