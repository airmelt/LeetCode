# 2942 Find Words Containing Character 查找包含给定字符的单词

__Description:__

You are given a __0-indexed__ array of strings `words` and a character `x`.

Return _an __array of indices__ representing the words that contain the character_ `x`.

Note that the returned array may be in any order.

__Example:__

Example 1:

```text
Input: words = ["leet","code"], x = "e"
Output: [0,1]
Explanation: "e" occurs in both words: "leet", and "code". Hence, we return indices 0 and 1.
```

Example 2:

```text
Input: words = ["abc","bcd","aaaa","cbc"], x = "a"
Output: [0,2]
Explanation: "a" occurs in "abc", and "aaaa". Hence, we return indices 0 and 2.
```

Example 3:

```text
Input: words = ["abc","bcd","aaaa","cbc"], x = "z"
Output: []
Explanation: "z" does not occur in any of the words. Hence, we return an empty array.
```

__Constraints:__

- `1 <= words.length <= 50`
- `1 <= words[i].length <= 50`
- `x` is a lowercase English letter.
- `words[i]` consists only of lowercase English letters.

__题目描述:__

给你一个下标从 __0__ 开始的字符串数组 `words` 和一个字符 `x` 。

请你返回一个 __下标数组__ ，表示下标在数组中对应的单词包含字符 `x` 。

注意 ，返回的数组可以是 任意 顺序。

__示例:__

示例 1：

```text
输入：words = ["leet","code"], x = "e"
输出：[0,1]
解释："e" 在两个单词中都出现了："leet" 和 "code" 。所以我们返回下标 0 和 1 。
```

示例 2：

```text
输入：words = ["abc","bcd","aaaa","cbc"], x = "a"
输出：[0,2]
解释："a" 在 "abc" 和 "aaaa" 中出现了，所以我们返回下标 0 和 2 。
```

示例 3：

```text
输入：words = ["abc","bcd","aaaa","cbc"], x = "z"
输出：[]
解释："z" 没有在任何单词中出现。所以我们返回空数组。
```

__提示：__

- `1 <= words.length <= 50`
- `1 <= words[i].length <= 50`
- `x` 是一个小写英文字母。
- `words[i]` 只包含小写英文字母。

__思路:__

```text
模拟
分别检查每一个字符串
如果字符串包含字符就加入结果
时间复杂度为 O(MN), 空间复杂度为 O(1), M 为字符串平均长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findWordsContaining(vector<string>& words, char x) 
    {
        vector<int> result;
        for (int i = 0, n = words.size(); i < n; i++) if (words[i].contains(x)) result.emplace_back(i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> findWordsContaining(String[] words, char x) {
        return IntStream.range(0, words.length).filter(i -> words[i].indexOf(x) >= 0).boxed().toList();
    }
}
```

__Python__:

```Python
class Solution:
    def findWordsContaining(self, words: List[str], x: str) -> List[int]:
        return [i for i, w in enumerate(words) if x in w]
```
