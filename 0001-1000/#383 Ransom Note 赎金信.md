# 383 Ransom Note 赎金信

__Description__:
Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

__Note:__
You may assume that both strings contain only lowercase letters.

```text
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
```

__题目描述__:
给定一个赎金信 (ransom) 字符串和一个杂志(magazine)字符串，判断第一个字符串ransom能不能由第二个字符串magazines里面的字符构成。如果可以构成，返回 true ；否则返回 false。

(题目说明：为了不暴露赎金信字迹，要从杂志上搜索各个需要的字母，组成单词来表达意思。)

__注意：__

你可以假设两个字符串均只含有小写字母。

```text
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
```

__思路__:

参考[LeetCode #242 Valid Anagram 有效的字母异位词](https://www.jianshu.com/p/34f01e89c893)
可以建立一个 26位的数组用来记录 ransomNote出现的字符及次数
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool canConstruct(string ransomNote, string magazine) 
    {
        vector<int> count(26, 0);
        for (int i = 0; i < magazine.size(); i++) ++count[magazine[i] - 'a'];
        for (int i = 0; i < ransomNote.size(); i++) 
        {
            count[ransomNote[i] - 'a']--;
            if (count[ransomNote[i] - 'a'] < 0) return false;
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int count[] = new int[26];
        for (int i = 0; i < magazine.length(); i++) count[magazine.charAt(i) - 'a']++;
        for (int i = 0; i < ransomNote.length(); i++) {
            count[ransomNote.charAt(i) - 'a']--;
            if (count[ransomNote.charAt(i) - 'a'] < 0) return false;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        return not collections.Counter(ransomNote) - collections.Counter(magazine)
```
