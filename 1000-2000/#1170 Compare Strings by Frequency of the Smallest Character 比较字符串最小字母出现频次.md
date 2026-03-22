# 1170 Compare Strings by Frequency of the Smallest Character 比较字符串最小字母出现频次

__Description__:
Let's define a function f(s) over a non-empty string s, which calculates the frequency of the smallest character in s. For example, if s = "dcce" then f(s) = 2 because the smallest character is "c" and its frequency is 2.

Now, given string arrays queries and words, return an integer array answer, where each answer[i] is the number of words such that f(queries[i]) < f(W), where W is a word in words.

__Example:__

Example 1:

Input: queries = ["cbd"], words = ["zaaaz"]
Output: [1]
Explanation: On the first query we have f("cbd") = 1, f("zaaaz") = 3 so f("cbd") < f("zaaaz").

Example 2:

Input: queries = ["bbb","cc"], words = ["a","aa","aaa","aaaa"]
Output: [1,2]
Explanation: On the first query only f("bbb") < f("aaaa"). On the second query both f("aaa") and f("aaaa") are both > f("cc").

__Constraints:__

1 <= queries.length <= 2000
1 <= words.length <= 2000
1 <= queries[i].length, words[i].length <= 10
queries[i][j], words[i][j] are English lowercase letters.

__题目描述__:
我们来定义一个函数 f(s)，其中传入参数 s 是一个非空字符串；该函数的功能是统计 s  中（按字典序比较）最小字母的出现频次。

例如，若 s = "dcce"，那么 f(s) = 2，因为最小的字母是 "c"，它出现了 2 次。

现在，给你两个字符串数组待查表 queries 和词汇表 words，请你返回一个整数数组 answer 作为答案，其中每个 answer[i] 是满足 f(queries[i]) < f(W) 的词的数目，W 是词汇表 words 中的词。

__示例 :__

示例 1：

输入：queries = ["cbd"], words = ["zaaaz"]
输出：[1]
解释：查询 f("cbd") = 1，而 f("zaaaz") = 3 所以 f("cbd") < f("zaaaz")。

示例 2：

输入：queries = ["bbb","cc"], words = ["a","aa","aaa","aaaa"]
输出：[1,2]
解释：第一个查询 f("bbb") < f("aaaa")，第二个查询 f("aaa") 和 f("aaaa") 都 > f("cc")。

__提示：__

1 <= queries.length <= 2000
1 <= words.length <= 2000
1 <= queries[i].length, words[i].length <= 10
queries[i][j], words[i][j] 都是小写英文字母

__思路__:

用一张表记录 words中的函数值出现的个数, 函数的计算方法是对 words中的每个字符串进行排序, 找到最小的字符出现的次数
之后累计出现的值, 这个值代表大于这个值的个数有多少
比如 words = ["a","aa","aaa","aaaa"]
count对应的应该是 [0, 1, 1, 1, 1...]表示, 函数值为1, 2, 3, 4的分别有 1个, 累计值应该是 [0, 4, 3, 2, 1...] 表示大于函数值 0的有 4个, 大于函数值 1的有 3个, 以此类推
时间复杂度O(nmlgm), 空间复杂度O(1), n表示 max(words, queries的长度), m表示 max(words, queries中字符串的长度)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> numSmallerByFrequency(vector<string>& queries, vector<string>& words) 
    {
        vector<int> result(queries.size(), 0), count(12, 0);
        for (int i = 0; i < words.size(); ++i) ++count[helper(words[i])];
        for (int i = 9; i >= 0; --i) count[i] += count[i + 1];
        for (int i = 0; i < queries.size(); ++i) result[i] = count[helper(queries[i]) + 1];
        return result;
    }
private:
    int helper(string s)
    {
        sort(s.begin(), s.end());
        int count = 1;
        for (int i = 1; i < s.size(); ++i)
        {
            if (s[i] == s[i - 1]) count++;
            else break;
        }
        return count;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] numSmallerByFrequency(String[] queries, String[] words) {
        int count[] = new int[12], result[] = new int[queries.length];
        for (int i = 0; i < words.length; ++i) ++count[helper(words[i])];
        for (int i = 9; i >= 0; --i) count[i] += count[i + 1];
        for (int i = 0; i < queries.length; ++i) result[i] = count[helper(queries[i]) + 1];
        return result;
    }
    
    private int helper(String word) {
        char c[] = word.toCharArray();
        Arrays.sort(c);
        word = new String(c);
        int count = 1;
        for (int i = 1; i < word.length(); ++i) {
            if (word.charAt(i) == word.charAt(i - 1)) count++;
            else break;
        }
        return count;
    }
}
```

__Python__:

```Python
class Solution:
    def numSmallerByFrequency(self, queries: List[str], words: List[str]) -> List[int]:
        f = lambda x: x.count(min(x))
        w = sorted(map(f, words))
        return [len(words) - bisect.bisect(w, i) for i in map(f, queries)]
```
