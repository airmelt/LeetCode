# 387 First Unique Character in a String 字符串中的第一个唯一字符

__Description__:
Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

**Example :**

s = "leetcode"
return 0.

s = "loveleetcode",
return 2.

__Note:__
You may assume the string contain only lowercase letters.

__题目描述__:
给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

**示例 :**

s = "leetcode"
返回 0.

s = "loveleetcode",
返回 2.

__注意：__
您可以假定该字符串只包含小写字母。

__思路__:

可以建立一个 26位的数组用来记录 s中出现的字符及次数, 再遍历一次记录第一个出现的 1即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int firstUniqChar(string s) 
    {
        int count[26] = {0};
        for (int i = 0; i < s.size(); i++) ++count[s[i] - 'a'];
        for (int i = 0; i < s.size(); i++) if(count[s[i] - 'a'] == 1) return i;
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int firstUniqChar(String s) {
        int count[] = new int[26];
        for (int i = 0; i < s.length(); i++) count[s.charAt(i) - 'a']++;
        for (int i = 0; i < s.length(); i++) if(count[s.charAt(i) - 'a'] == 1) return i;
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def firstUniqChar(self, s: str) -> int:
        return [s.index(i) for i in s if s.find(i) == s.rfind(i)][0] if [s.index(i) for i in s if s.find(i) == s.rfind(i)] else -1
```
