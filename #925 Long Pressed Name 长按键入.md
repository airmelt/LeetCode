__Description__:
Your friend is typing his name into a keyboard.  Sometimes, when typing a character c, the key might get long pressed, and the character will be typed 1 or more times.

You examine the typed characters of the keyboard.  Return True if it is possible that it was your friends name, with some characters (possibly none) being long pressed.

__Example:__
Example 1:

Input: name = "alex", typed = "aaleex"
Output: true
Explanation: 'a' and 'e' in 'alex' were long pressed.

Example 2:

Input: name = "saeed", typed = "ssaaedd"
Output: false
Explanation: 'e' must have been pressed twice, but it wasn't in the typed output.

Example 3:

Input: name = "leelee", typed = "lleeelee"
Output: true

Example 4:

Input: name = "laiden", typed = "laiden"
Output: true
Explanation: It's not necessary to long press any character.
 
__Note:__

name.length <= 1000
typed.length <= 1000
The characters of name and typed are lowercase letters.

__题目描述__:
你的朋友正在使用键盘输入他的名字 name。偶尔，在键入字符 c 时，按键可能会被长按，而字符可能被输入 1 次或多次。

你将会检查键盘输入的字符 typed。如果它对应的可能是你的朋友的名字（其中一些字符可能被长按），那么就返回 True。

__示例 :__
示例 1：

输入：name = "alex", typed = "aaleex"
输出：true
解释：'alex' 中的 'a' 和 'e' 被长按。

示例 2：

输入：name = "saeed", typed = "ssaaedd"
输出：false
解释：'e' 一定需要被键入两次，但在 typed 的输出中不是这样。

示例 3：

输入：name = "leelee", typed = "lleeelee"
输出：true

示例 4：

输入：name = "laiden", typed = "laiden"
输出：true
解释：长按名字中的字符并不是必要的。
 
__提示：__

name.length <= 1000
typed.length <= 1000
name 和 typed 的字符都是小写字母。

__思路__:
双指针法
__注意!__
typed的末尾可能和name的末尾不相等
如alex和alexfowefoiwofnewo
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution {
public:
    bool isLongPressedName(string name, string typed) 
    {
        if (name.size() > typed.size()) return false;
        int i = 0, j = 0;
        while (i < name.size() && j < typed.size()) 
        {
            if (name[i] == typed[j]) 
            {
                i++;
                j++;
            }
            else if (j > 0 && typed[j] == typed[j - 1]) j++;
            else return false;
        }
        if (i == name.size()) {
            while (j < typed.size()) {
                if (typed[j] != name[i - 1]) return false;
                j++;
            }
        } else return false;
        return true;
    }
};
```

__Java__:
```Java
class Solution {
    public boolean isLongPressedName(String name, String typed) {
        if (name.length() > typed.length()) return false;
        int i = 0, j = 0;
        while (i < name.length() && j < typed.length()) {
            if (name.charAt(i) == typed.charAt(j)) {
                i++;
                j++;
            }
            else if (j > 0 && typed.charAt(j) == typed.charAt(j - 1)) j++;
            else return false;
        }
        if (i == name.length()) {
            while (j < typed.length()) {
                if (typed.charAt(j) != name.charAt(i - 1)) return false;
                j++;
            }
        } else return false;
        return true;
    }
}
```

__Python__:
```Python
class Solution:
    def isLongPressedName(self, name: str, typed: str) -> bool:
        if len(name) > len(typed):
            return False
        i, j = 0, 0
        while i < len(name) and j < len(typed):
            if name[i] == typed[j]:
                i += 1
                j += 1
            elif j > 0 and typed[j] == typed[j - 1]:
                j += 1
            else:
                return False;
        if i == len(name):
            while j < len(typed):
                if typed[j] != name[-1]:
                    return False
                j += 1
        else:
            return False
        return True
```