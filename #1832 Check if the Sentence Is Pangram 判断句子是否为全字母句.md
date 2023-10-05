# 1832 Check if the Sentence Is Pangram 判断句子是否为全字母句

__Description:__

A pangram is a sentence where every letter of the English alphabet appears at least once.

Given a string `sentence` containing only lowercase English letters, return `true` _if_ `sentence` _is a __pangram__, or_ `false` _otherwise._

__Example:__

Example 1:

```text
Input: sentence = "thequickbrownfoxjumpsoverthelazydog"
Output: true
Explanation: sentence contains at least one of every letter of the English alphabet.
```

Example 2:

```text
Input: sentence = "leetcode"
Output: false
```

__Constraints:__

- `1 <= sentence.length <= 1000`
- `sentence` consists of lowercase English letters.

__题目描述:__

全字母句 指包含英语字母表中每个字母至少一次的句子。

给你一个仅由小写英文字母组成的字符串 `sentence` ，请你判断 `sentence` 是否为 __全字母句__ 。

如果是，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入: sentence = "thequickbrownfoxjumpsoverthelazydog"
输出: true
解释: `sentence` 包含英语字母表中每个字母至少一次。
```

示例 2：

```text
输入：sentence = "leetcode"
输出：false
```

__提示：__

- `1 <= sentence.length <= 1000`
- `sentence` 由小写英语字母组成

__思路:__

```text
模拟
用布尔数组记录所有字符是否出现
需要全部出现才返回 true
可以用位运算代替布尔数组
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool checkIfPangram(string sentence) 
    {
        int visited = 0;
        for (const auto& c : sentence) visited |= (1 << (c - 'a'));
        return visited == (1 << 26) - 1;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkIfPangram(String sentence) {
        boolean[] visited = new boolean[26];
        for (char c : sentence.toCharArray()) visited[c - 'a'] = true;
        for (boolean b : visited) if (!b) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def checkIfPangram(self, sentence: str) -> bool:
        return len(set(sentence)) == 26
```
