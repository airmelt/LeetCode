__Description__:
Given two strings S and T, return if they are equal when both are typed into empty text editors. # means a backspace character.

__Example:__
Example 1:

Input: S = "ab#c", T = "ad#c"
Output: true
Explanation: Both S and T become "ac".

Example 2:

Input: S = "ab##", T = "c#d#"
Output: true
Explanation: Both S and T become "".

Example 3:

Input: S = "a##c", T = "#a#c"
Output: true
Explanation: Both S and T become "c".

Example 4:

Input: S = "a#c", T = "b"
Output: false
Explanation: S becomes "c" while T becomes "b".

__Note:__

1 <= S.length <= 200
1 <= T.length <= 200
S and T only contain lowercase letters and '#' characters.

__Follow up:__

Can you solve it in O(N) time and O(1) space?

__题目描述__:
给定 S 和 T 两个字符串，当它们分别被输入到空白的文本编辑器后，判断二者是否相等，并返回结果。 # 代表退格字符。

__示例 :__
示例 1：

输入：S = "ab#c", T = "ad#c"
输出：true
解释：S 和 T 都会变成 “ac”。

示例 2：

输入：S = "ab##", T = "c#d#"
输出：true
解释：S 和 T 都会变成 “”。

示例 3：

输入：S = "a##c", T = "#a#c"
输出：true
解释：S 和 T 都会变成 “c”。

示例 4：

输入：S = "a#c", T = "b"
输出：false
解释：S 会变成 “c”，但 T 仍然是 “b”。
 
__提示：__

1 <= S.length <= 200
1 <= T.length <= 200
S 和 T 只含有小写字母以及字符 '#'。

__思路__:
1. 直接在字符串上使用 backspace
时间复杂度O(n), 空间复杂度O(1)
2. 使用栈记录 backspace的结果
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution {
public:

    bool backspaceCompare(string S, string T) {
        int s_index = backspace(S), t_index = backspace(T);
        if (s_index != t_index) return false;
        else {
            S = S.substr(0, s_index);
            T = T.substr(0, t_index);
            if (S == T) return true;
        }
        return false;
    }
private:
    int backspace (string& S) {
        int index = 0;
        for (auto i : S) {
            if (i != '#') S[index++]=i;
            else index == 0 ? 0 : --index;
        }
        return index;
    }
};
```

__Java__:
```Java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        Stack<Character> s = new Stack<>();
        Stack<Character> t = new Stack<>();
        backspaceString(S, s);
        backspaceString(T, t);
        if (s.size() != t.size()) return false;
        while (s.size() != 0) if (!s.isEmpty() && !t.isEmpty() && s.pop() != t.pop()) return false;
        return true;
    }
    
    private void backspaceString(String S, Stack<Character> s) {
         for (Character c : S.toCharArray()) {
            if (c == '#' && !s.isEmpty()) s.pop();
            else if (c != '#') s.push(c);
        }
    }
}
```

__Python__:
```Python
class Solution:
    def backspaceCompare(self, S: str, T: str) -> bool:
        s, t = [], []
        def backspace_string(S: str, s: List[str]) -> None:
            for i in range(len(S)):
                if S[i] != '#':
                    s.append(S[i])
                elif S[i] == '#' and s:
                    s.pop()
        backspace_string(S, s)
        backspace_string(T, t)
        return s == t
```