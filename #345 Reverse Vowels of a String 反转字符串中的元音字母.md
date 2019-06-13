__Description__:
Write a function that takes a string as input and reverse only the vowels of a string.

**Example :**
Example 1:
Input: "hello"
Output: "holle"

Example 2:
Input: "leetcode"
Output: "leotcede"

__Note:__
The vowels does not include the letter "y".

__题目描述__:
编写一个函数，以字符串作为输入，反转该字符串中的元音字母。

**示例 :**
示例 1:
输入: "hello"
输出: "holle"

示例 2:
输入: "leetcode"
输出: "leotcede"

__说明:__
元音字母不包含字母"y"。

__思路__:
双指针循环交换, 判断是否是元音字母即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    string reverseVowels(string s) {
        int i = 0, j = s.size() - 1;
        string vowel = "aeiouAEIOU";
        while (i < j) {
            while (vowel.find(s[i]) == -1 && i < j) i++;
            while (vowel.find(s[j]) == -1 && i < j) j--;
            swap(s[i++], s[j--]);
        }
        return s;
    }
};
```

__Java__:
```
class Solution {
    public String reverseVowels(String s) {
        int i = 0, j = s.length() - 1;
        char[] c = s.toCharArray();
        String vowel = "aeiouAEIOU";
        while (i < j) {
            while (i < j && vowel.indexOf(c[i]) == -1) i++;
            while (i < j && vowel.indexOf(c[j]) == -1) j--;   
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
```
class Solution:
    def reverseVowels(self, s: str) -> str:
        i, j = 0, len(s) - 1
        vowel = ['a', 'e', 'i', 'o', 'u']
        s = list(s)
        while i < j:
            while not s[i].lower() in vowel and i < j:
                i += 1
            while not s[j].lower() in vowel and i < j:
                j -= 1
            s[i], s[j] = s[j], s[i]
            i += 1
            j -= 1
        return ''.join(s)
```
