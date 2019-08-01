__Description__:
Given a List of words, return the words that can be typed using letters of alphabet on only one row's of American keyboard like the image below.
![keyboard](https://assets.leetcode.com/uploads/2018/10/12/keyboard.png)

__Example:__

Input: ["Hello", "Alaska", "Dad", "Peace"]
Output: ["Alaska", "Dad"]
 
__Note:__

You may use one character in the keyboard more than once.
You may assume the input string will only contain letters of alphabet.

__题目描述__:
给定一个单词列表，只返回可以使用在键盘同一行的字母打印出来的单词。键盘如下图所示。
![键盘](https://assets.leetcode.com/uploads/2018/10/12/keyboard.png)

__示例：__

输入: ["Hello", "Alaska", "Dad", "Peace"]
输出: ["Alaska", "Dad"]

__注意:__

你可以重复使用键盘上同一字符。
你可以假设输入的字符串将只包含字母。

__思路__:
存下三行键盘的值, 依次判断单词即可
时间复杂度O(mn), 空间复杂度O(1), m单词平均长度, n为数组中单词数

__代码__:
__C++__:
```
class Solution {
public:
    vector<string> findWords(vector<string>& words) {
        vector<string> result;
        string first = "qwertyuiop";
        string second = "asdfghjkl";
        string third = "zxcvbnm";
        for (auto word : words) if (check(word, first, second, third)) result.push_back(word);
        return result;
    }
private:
    bool check(string s, string first, string second, string third) {
        int one = 0, two = 0, three = 0;
        for(int i = 0; i < s.size(); i++) {
              if (first.find(tolower(s[i])) != string::npos) one++;
              else if (second.find(tolower(s[i])) != string::npos) two++;
              else if (third.find(tolower(s[i])) != string::npos) three++;
        }
        if (one == s.size() || two == s.size() || three == s.size()) return true;
        else return false;
    }

};
```

__Java__:
```
class Solution {
    public String[] findWords(String[] words) {
        String first = "qwertyuiop";
        String second = "asdfghjkl";
        String third = "zxcvbnm";
        List<String> result = new ArrayList<>();
        for (String word : words) if (check(word.toLowerCase(), first, second, third)) result.add(word);
        return result.toArray(new String[result.size()]);
    }

    private boolean check(String word, String first, String second, String third) {
        int one = 0, two = 0, three = 0;
        for (int i = 0; i < word.length(); i++) {
            if (first.indexOf(word.charAt(i)) != -1) one++;
            if (second.indexOf(word.charAt(i)) != -1) two++;
            if (third.indexOf(word.charAt(i)) != -1) three++;
        }
        if (one == word.length() || two == word.length() || three == word.length()) return true;
        return false;
    }
}
```

__Python__:
```
class Solution:
    def findWords(self, words: List[str]) -> List[str]:
        return [word for word in words if any(all(c in row for c in word.lower()) for row in ["qwertyuiop", "asdfghjkl", "zxcvbnm"])]
```
