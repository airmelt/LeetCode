# 2273 Find Resultant Array After Removing Anagrams 移除字母异位词后的结果数组

__Description:__

You are given a __0-indexed__ string array `words`, where `words[i]` consists of lowercase English letters.

In one operation, select any index `i` such that `0 < i < words.length` and `words[i - 1]` and `words[i]` are __anagrams__, and __delete__ `words[i]` from `words`. Keep performing this operation as long as you can select an index that satisfies the conditions.

Return `words` _after performing all operations_. It can be shown that selecting the indices for each operation in __any__ arbitrary order will lead to the same result.

An __Anagram__ is a word or phrase formed by rearranging the letters of a different word or phrase using all the original letters exactly once. For example, `"dacb"` is an anagram of `"abdc"`.

__Example:__

Example 1:

```text
Input: words = ["abba","baba","bbaa","cd","cd"]
Output: ["abba","cd"]
Explanation:
One of the ways we can obtain the resultant array is by using the following operations:
```

- Since words[2] = "bbaa" and words[1] = "baba" are anagrams, we choose index 2 and delete words[2].
  Now words = ["abba","baba","cd","cd"].
- Since words[1] = "baba" and words[0] = "abba" are anagrams, we choose index 1 and delete words[1].
  Now words = ["abba","cd","cd"].
- Since words[2] = "cd" and words[1] = "cd" are anagrams, we choose index 2 and delete words[2].
  Now words = ["abba","cd"].

We can no longer perform any operations, so ["abba","cd"] is the final answer.

Example 2:

```text
Input: words = ["a","b","c","d","e"]
Output: ["a","b","c","d","e"]
Explanation:
No two adjacent strings in words are anagrams of each other, so no operations are performed.
```

__Constraints:__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 10`
- `words[i]` consists of lowercase English letters.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `words` ，其中 `words[i]` 由小写英文字符组成。

在一步操作中，需要选出任一下标 `i` ，从 `words` 中 __删除__ `words[i]` 。其中下标 `i` 需要同时满足下述两个条件:

只要可以选出满足条件的下标，就一直执行这个操作。

在执行所有操作后，返回 `words` 。可以证明，按任意顺序为每步操作选择下标都会得到相同的结果。

__字母异位词__ 是由重新排列源单词的字母得到的一个新单词，所有源单词中的字母通常恰好只用一次。例如， `"dacb"` 是 `"abdc"` 的一个字母异位词。

__示例:__

示例 1：

```text
输入：words = ["abba","baba","bbaa","cd","cd"]
输出：["abba","cd"]
解释：
获取结果数组的方法之一是执行下述步骤：
```

- 由于 words[2] = "bbaa" 和 words[1] = "baba" 是字母异位词，选择下标 2 并删除 words[2] 。
  现在 words = ["abba","baba","cd","cd"] 。
- 由于 words[1] = "baba" 和 words[0] = "abba" 是字母异位词，选择下标 1 并删除 words[1] 。
  现在 words = ["abba","cd","cd"] 。
- 由于 words[2] = "cd" 和 words[1] = "cd" 是字母异位词，选择下标 2 并删除 words[2] 。
  现在 words = ["abba","cd"] 。

无法再执行任何操作，所以 ["abba","cd"] 是最终答案。

示例 2：

```text
输入：words = ["a","b","c","d","e"]
输出：["a","b","c","d","e"]
解释：
words 中不存在互为字母异位词的两个相邻字符串，所以无需执行任何操作。
```

__提示：__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 10`
- `words[i]` 由小写英文字母组成

__思路:__

```text
模拟
检查相邻字符串是否为字母异位词
字母异位词可以用排序或者差分数组的方式进行检查
时间复杂度为 O(MN), 空间复杂度为 O(1), 其中 M 为字符串长度, N 为字符串个数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> removeAnagrams(vector<string>& words) 
    {
        vector<string> result;
        int left = 0, right = 0, n = words.size();
        while (right < n) 
        {
            while (right < n and check(words[left], words[right])) ++right;
            result.emplace_back(words[left]);
            left = right;
        }
        return result;
    }
private:
    bool check(string& a, string& b) 
    {
        vector<int> cnt(26);
        for (const auto& c : a) ++cnt[c - 'a'];
        for (const auto& c : b) --cnt[c - 'a'];
        for (int i = 0; i < 26; i++) if (cnt[i]) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> removeAnagrams(String[] words) {
        List<String> result = new ArrayList<>();
        int left = 0, right = 0, n = words.length;
        while (right < n) {
            while (right < n && check(words[left], words[right])) ++right;
            result.add(words[left]);
            left = right;
        }
        return result;
    }

    private boolean check(String a, String b) {
        int[] count = new int[26];
        for (char c : a.toCharArray()) ++count[c - 'a'];
        for (char c : b.toCharArray()) --count[c - 'a'];
        for (int i = 0; i < 26; i++) if (count[i] != 0) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def removeAnagrams(self, words: List[str]) -> List[str]:
        return [w for i, w in enumerate(words) if i == 0 or sorted(words[i - 1]) != sorted(words[i])]
```
