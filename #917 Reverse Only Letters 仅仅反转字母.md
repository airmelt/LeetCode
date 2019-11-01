__Description__:
Given a string S, return the "reversed" string where all characters that are not a letter stay in the same place, and all letters reverse their positions.

__Example:__
Example 1:

Input: "ab-cd"
Output: "dc-ba"

Example 2:

Input: "a-bC-dEf-ghIj"
Output: "j-Ih-gfE-dCba"

Example 3:

Input: "Test1ng-Leet=code-Q!"
Output: "Qedo1ct-eeLg=ntse-T!"
 
__Note:__

S.length <= 100
33 <= S[i].ASCIIcode <= 122 
S doesn't contain \ or "

__题目描述__:
给定一个字符串 S，返回 “反转后的” 字符串，其中不是字母的字符都保留在原地，而所有字母的位置发生反转。

__示例 :__
示例 1：

输入："ab-cd"
输出："dc-ba"

示例 2：

输入："a-bC-dEf-ghIj"
输出："j-Ih-gfE-dCba"

示例 3：

输入："Test1ng-Leet=code-Q!"
输出："Qedo1ct-eeLg=ntse-T!"
 
__提示：__

S.length <= 100
33 <= S[i].ASCIIcode <= 122 
S 中不包含 \ or "

__思路__:
双指针法
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution {
public:
    string reverseOnlyLetters(string S) 
    {
        int i = 0, j = S.size() - 1;
        while (i < j)
        {
            while (!isalpha(S[i]) && i < j) i++;     
            while (!isalpha(S[j]) && i < j) j--;
            if (i < j) swap(S[i++], S[j--]);
        }
        return S;
    }
};
```

__Java__:
```Java
class Solution {
    public String reverseOnlyLetters(String S) {
        char[] c = S.toCharArray();
        int i = 0, j = c.length - 1;
        while (i < j) {
            while (!Character.isLetter(c[i]) && i < j) i++;
            while (!Character.isLetter(c[j]) && i < j) j--;
            if (i < j) {
                c[i] ^= c[j];
                c[j] ^= c[i];
                c[i++] ^= c[j--];
            }
        }
        return new String(c);
    }
}
```

__Python__:
```Python
class Solution:
    def reverseOnlyLetters(self, S: str) -> str:
        i, j, S = 0, len(S) - 1, list(S)
        while i < j:
            while not S[i].isalpha() and i < j:
                i += 1
            while not S[j].isalpha() and i < j:
                j -= 1
            if i < j:
                S[i], S[j] = S[j], S[i]
                i += 1
                j -= 1
        return ''.join(S)
```