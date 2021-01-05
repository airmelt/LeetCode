# 58 Length of Last Word 最后一个单词的长度

__Description__:
Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0.

Note: A word is defined as a character sequence consists of non-space characters only.

__Example__:

Input: "Hello World"
Output: 5

__题目描述__:
给定一个仅包含大小写字母和空格 ' ' 的字符串，返回其最后一个单词的长度。

如果不存在最后一个单词，请返回 0 。

说明：一个单词是指由字母组成，但不包含任何空格的字符串。

__示例__:

输入: "Hello World"
输出: 5

__思路__:

先去除两端的括号, 再按空格拆分成字符串数组, 选择最后一个元素输出长度即可
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    void split(std::string& s, std::string& separator, std::vector<std::string>& result) 
    {
        size_t last = 0, index = s.find_first_of(separator, last);
        while (index != std::string::npos) 
        {
            result.push_back(s.substr(last,index - last));
            last = index + 1;
            index = s.find_first_of(separator,last);
        }
        if (index - last > 0) result.push_back(s.substr(last, index - last));
    }

    int lengthOfLastWord(string s) 
    {
        trim(s);
        vector<string> result;
        string separator = " ";
        split(s, separator, result);
        return result[result.size() - 1].size();
    }

    // 去除string两端的空格
    std::string& trim(std::string &s) 
    {
        if (s.empty()) return s;
        // iterator erase (iterator position);　　删除指定元素
        // iterator erase (iterator first, iterator last);　　删除指定范围内的元素
        // size_type find_first_not_of( const basic_string &str, size_type index = 0 );
        // 在字符串中查找第一个与str中的字符都不匹配的字符
        s.erase(0, s.find_first_not_of(" "));
        s.erase(s.find_last_not_of(" ") + 1);
        return s;
    }

};
```

__Java__:

```Java
class Solution {
    public int lengthOfLastWord(String s) {
        String[] strs = s.trim().split(" ");
        return strs[strs.length - 1].length();
    }
}
```

__Python__:

```Python
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        return len(s.strip().split(" ")[-1])
```
