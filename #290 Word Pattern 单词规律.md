__Description__:
Given a pattern and a string str, find if str follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in str.

**Example :**
Example 1:
Input: pattern = "abba", str = "dog cat cat dog"
Output: true

Example 2:
Input:pattern = "abba", str = "dog cat cat fish"
Output: false

Example 3:
Input: pattern = "aaaa", str = "dog cat cat dog"
Output: false

Example 4:
Input: pattern = "abba", str = "dog dog dog dog"
Output: false

__Notes:__
You may assume pattern contains only lowercase letters, and str contains lowercase letters that may be separated by a single space.

__题目描述__:
给定一种规律 pattern 和一个字符串 str ，判断 str 是否遵循相同的规律。

这里的 遵循 指完全匹配，例如， pattern 里的每个字母和字符串 str 中的每个非空单词之间存在着双向连接的对应规律。

**示例 :**
示例1:
输入: pattern = "abba", str = "dog cat cat dog"
输出: true

示例 2:
输入:pattern = "abba", str = "dog cat cat fish"
输出: false

示例 3:
输入: pattern = "aaaa", str = "dog cat cat dog"
输出: false

示例 4:
输入: pattern = "abba", str = "dog dog dog dog"
输出: false

__说明:__
你可以假设 pattern 只包含小写字母， str 包含了由单个空格分隔的小写字母。

__思路__:
用 split()函数将字符串按空格分割成字符串数组
采用 map记录单词和 pattern中的字符的对应关系
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```
class Solution {
public:
    bool wordPattern(string pattern, string str) {
        const vector<string>& list = split(str,' '); 
        if (list.size() != pattern.size()) return false;
        map<string, char> dict1;
        map<char, string> dict2;
        for (int i = 0; i < list.size(); i++) {
            const string& s = list[i];            
            if (dict1.find(s) != dict1.end()) {
                char tmp = dict1[s];
                if (tmp != pattern[i]) return false;
            } else {
                if (dict2.size() && dict2.find(pattern[i]) != dict2.end()) return false;
                dict1.insert(make_pair(s, pattern[i]));
                dict2.insert(make_pair(pattern[i], s));
            }      
        }
        return true;
    }
private:
    vector<string> split(string& str,const char a) {
        vector<string> strvec;
        string::size_type pos1, pos2;
        pos2 = str.find(a);
        pos1 = 0;
        while (string::npos != pos2) {
            strvec.push_back(str.substr(pos1, pos2 - pos1));
            pos1 = pos2 + 1;
            pos2 = str.find(a, pos1);
        }
        strvec.push_back(str.substr(pos1));
        return strvec;
    }
};
```

__Java__:
```
class Solution {
    public boolean wordPattern(String pattern, String str) {
        char[] chars = pattern.toCharArray();
        String[] strs = str.split(" ");
        if (chars.length != strs.length) return false;
        Map<Character, String> map = new HashMap<>();
        for (int i = 0; i < chars.length; i++) {
            if (map.containsKey(chars[i])) {
                if (!map.get(chars[i]).equals(strs[i])) return false;
            } else if (map.containsValue(strs[i])) return false;
            map.put(chars[i], strs[i]);
        }
        return true;
    }
}
```

__Python__:
```
class Solution:
    def wordPattern(self, pattern: str, str: str) -> bool:
        str = str.split(' ')
        if not pattern or not str or len(set(str)) != len(set(pattern)) or len(str) != len(pattern):
        	return False
        dict = {}
        for i in range(len(str)):
        	if pattern[i] in dict:
        		if dict[pattern[i]] == str[i]:
        			continue
        		else:
        			return False
        	dict[pattern[i]] = str[i]
        return True
```