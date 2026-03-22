# 884 Uncommon Words from Two Sentences 两句话中的不常见单词

__Description__:
We are given two sentences A and B.  (A sentence is a string of space separated words.  Each word consists only of lowercase letters.)

A word is uncommon if it appears exactly once in one of the sentences, and does not appear in the other sentence.

Return a list of all uncommon words.

You may return the list in any order.

__Example:__

Example 1:

Input: A = "this apple is sweet", B = "this apple is sour"
Output: ["sweet","sour"]

Example 2:

Input: A = "apple apple", B = "banana"
Output: ["banana"]

__Note:__

0 <= A.length <= 200
0 <= B.length <= 200
A and B both contain only spaces and lowercase letters.

__题目描述__:
给定两个句子 A 和 B 。 （句子是一串由空格分隔的单词。每个单词仅由小写字母组成。）

如果一个单词在其中一个句子中只出现一次，在另一个句子中却没有出现，那么这个单词就是不常见的。

返回所有不常用单词的列表。

您可以按任何顺序返回列表。

__示例 :__

示例 1：

输入：A = "this apple is sweet", B = "this apple is sour"
输出：["sweet","sour"]

示例 2：

输入：A = "apple apple", B = "banana"
输出：["banana"]

__提示：__

0 <= A.length <= 200
0 <= B.length <= 200
A 和 B 都只包含空格和小写字母。

__思路__:

将 A和 B加起来看作一个字符串, 按空格分开(split(" "))之后用 map记录单词和出现次数, 返回只出现一次的单词即可
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> uncommonFromSentences(string A, string B) 
    {
        vector<string> result;
        A += " " + B;
        map<string, int> word_times;
        split(A, result, " ");
        for (auto word : result) word_times[word]++;
        result.clear();
        for (auto word : word_times) if (word.second < 2) result.push_back(word.first);
        return result;
    }
private:
    int split(const string& str, vector<string>& result, string separator = ",") 
    {
        if (str.empty())
        {
            return 0;
        }

        string temp;
        string::size_type pos_begin = str.find_first_not_of(separator);
        string::size_type comma_pos = 0;

        while (pos_begin != string::npos)
        {
            comma_pos = str.find(separator, pos_begin);
            if (comma_pos != string::npos)
            {
                temp = str.substr(pos_begin, comma_pos - pos_begin);
                pos_begin = comma_pos + separator.length();
            }
            else
            {
                temp = str.substr(pos_begin);
                pos_begin = comma_pos;
            }

            if (!temp.empty())
            {
                result.push_back(temp);
                /* string.clear()
                   作用: 将字符串的内容清空，让源字符串成为一个空字符串（长度为0个字符）*/
                temp.clear();
            }
        }
        return 0;
    }
};
```

__Java__:

```Java
class Solution {
    public String[] uncommonFromSentences(String A, String B) {
        A += " " + B;
        String words[] = A.split(" ");
        Map<String, Integer> word_times = new HashMap<>();
        for (String word : words) word_times.put(word, word_times.getOrDefault(word, 0) + 1);
        List<String> result = new ArrayList<>();
        for (Map.Entry<String, Integer> entry : word_times.entrySet()) if (entry.getValue() < 2) result.add(entry.getKey());
        return result.toArray(new String[result.size()]);
    }
}
```

__Python__:

```Python
class Solution:
    def uncommonFromSentences(self, A: str, B: str) -> List[str]:
        return [word for word in (A + " " + B).split(" ") if (A + " " + B).split(" ").count(word) == 1]
```
