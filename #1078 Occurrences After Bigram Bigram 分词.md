__Description__:
Given words first and second, consider occurrences in some text of the form "first second third", where second comes immediately after first, and third comes immediately after second.

For each such occurrence, add "third" to the answer, and return the answer.

__Example:__
Example 1:

Input: text = "alice is a good girl she is a good student", first = "a", second = "good"
Output: ["girl","student"]

Example 2:

Input: text = "we will we will rock you", first = "we", second = "will"
Output: ["we","rock"]
 
__Note:__

1 <= text.length <= 1000
text consists of space separated words, where each word consists of lowercase English letters.
1 <= first.length, second.length <= 10
first and second consist of lowercase English letters.

__题目描述__:
给出第一个词 first 和第二个词 second，考虑在某些文本 text 中可能以 "first second third" 形式出现的情况，其中 second 紧随 first 出现，third 紧随 second 出现。

对于每种这样的情况，将第三个词 "third" 添加到答案中，并返回答案。

__示例 :__
示例 1：

输入：text = "alice is a good girl she is a good student", first = "a", second = "good"
输出：["girl","student"]

示例 2：

输入：text = "we will we will rock you", first = "we", second = "will"
输出：["we","rock"]
 
__提示：__

1 <= text.length <= 1000
text 由一些用空格分隔的单词组成，每个单词都由小写英文字母组成
1 <= first.length, second.length <= 10
first 和 second 由小写英文字母组成

__思路__:
将字符串按照空格字符分开, 查找到符合题目要求的字符串就加入结果
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<string> findOcurrences(string text, string first, string second) 
    {
        stringstream ss(text);
        int flag = 0;
        vector<string> result;
        string target = first + second, s, t;
        ss >> s;
        t = s;
        while (ss >> s)
        {
            if (flag-- > 0) result.push_back(s);
            string temp = t + s;
            if (temp == target) flag = 1;
            t = s;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public String[] findOcurrences(String text, String first, String second) {
        String[] words = text.split(" ");
        List<String> result = new ArrayList<>();
        for (int i = 0; i < words.length - 2; ++i) if (first.equals(words[i]) && second.equals(words[i + 1])) result.add(words[i + 2]);
        return result.toArray(new String[result.size()]);
    }
}
```

__Python__:
```Python
class Solution:
    def findOcurrences(self, text: str, first: str, second: str) -> List[str]:
        text, result = text.split(' '), []
        for i in range(len(text) - 2):
            if text[i] == first and text[i + 1] == second:
                result.append(text[i + 2])
        return result
```