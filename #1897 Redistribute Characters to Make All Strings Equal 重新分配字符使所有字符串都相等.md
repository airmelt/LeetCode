# 1897 Redistribute Characters to Make All Strings Equal 重新分配字符使所有字符串都相等

__Description:__

You are given an array of strings `words` (__0-indexed__).

In one operation, pick two __distinct__ indices `i` and `j`, where `words[i]` is a non-empty string, and move __any__ character from `words[i]` to __any__ position in `words[j]`.

Return `true` _if you can make __every__ string in_ `words` ___equal__ using __any__ number of operations_, _and_ `false` _otherwise_.

__Example:__

Example 1:

```text
Input:  words = ["abc","aabc","bc"]
Output:  true
Explanation:  Move the first 'a' in `words[1] to the front of words[2],
to make` `words[1]` = "abc" and words[2] = "abc".
All the strings are now equal to "abc", so return `true`.
```

Example 2:

```text
Input: words = ["ab","a"]
Output: false
Explanation: It is impossible to make all the strings equal using the operation.
```

__Constraints:__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` consists of lowercase English letters.

__题目描述:__

给你一个字符串数组 `words`（下标 __从 0 开始__ 计数）。

在一步操作中，需先选出两个 __不同__ 下标 `i` 和 `j`，其中 `words[i]` 是一个非空字符串，接着将 `words[i]` 中的 __任一__ 字符移动到 `words[j]` 中的 __任一__ 位置上。

如果执行任意步操作可以使 `words` 中的每个字符串都相等，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入: words = ["abc","aabc","bc"]
输出: true
解释: 将 `words[1] 中的第一个` 'a' 移动到 `words[2] 的最前面。
使` `words[1]` = "abc" 且 words[2] = "abc" 。
所有字符串都等于 "abc" ，所以返回 `true` 。
```

示例 2：

```text
输入：words = ["ab","a"]
输出：false
解释：执行操作无法使所有字符串都相等。
```

__提示：__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` 由小写英文字母组成

__思路:__

```text
计数
统计所有字符出现的次数
若所有字符出现的次数是字符串个数的整数倍，则返回 true
否则返回 false
时间复杂度为 O(N), 空间复杂度为 O(1), 其中 N 为所有字符串的长度和
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool makeEqual(vector<string>& words) 
    {
        vector<int> cur(26);
        for (const auto& word : words) for (const auto& c : word) ++cur[c - 'a'];
        for (const auto c : cur) if (c % words.size()) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean makeEqual(String[] words) {
        int n = words.length, count[] = new int[26];
        for (String word : words) for (char c : word.toCharArray()) ++count[c - 'a'];
        for (int c : count) if (c % n != 0) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def makeEqual(self, words: List[str]) -> bool:
        return all(not v % len(words) for v in Counter(i for s in words for i in s).values())
```
