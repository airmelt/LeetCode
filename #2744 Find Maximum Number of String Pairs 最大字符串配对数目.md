# 2744 Find Maximum Number of String Pairs 最大字符串配对数目

__Description:__

You are given a __0-indexed__ array `words` consisting of __distinct__ strings.

The string `words[i]` can be paired with the string `words[j]` if:

- The string `words[i]` is equal to the reversed string of `words[j]`.
- `0 <= i < j < words.length`.

Return _the __maximum__ number of pairs that can be formed from the array_ `words`_._

Note that each string can belong in at most one pair.

__Example:__

Example 1:

```text
Input: words = ["cd","ac","dc","ca","zz"]
Output: 2
Explanation: In this example, we can form 2 pair of strings in the following way:
```

- We pair the 0th string with the 2nd string, as the reversed string of word[0] is "dc" and is equal to words[2].
- We pair the 1st string with the 3rd string, as the reversed string of word[1] is "ca" and is equal to words[3].

It can be proven that 2 is the maximum number of pairs that can be formed.

Example 2:

```text
Input: words = ["ab","ba","cc"]
Output: 1
Explanation: In this example, we can form 1 pair of strings in the following way:
```

- We pair the 0th string with the 1st string, as the reversed string of words[1] is "ab" and is equal to words[0].

It can be proven that 1 is the maximum number of pairs that can be formed.

Example 3:

```text
Input: words = ["aa","ab"]
Output: 0
Explanation: In this example, we are unable to form any pair of strings.
```

__Constraints:__

- `1 <= words.length <= 50`
- `words[i].length == 2`
- `words` consists of distinct strings.
- `words[i]` contains only lowercase English letters.

__题目描述:__

给你一个下标从 __0__ 开始的数组 `words` ，数组中包含 __互不相同__ 的字符串。

如果字符串 `words[i]` 与字符串 `words[j]` 满足以下条件，我们称它们可以匹配:

- 字符串 `words[i]` 等于 `words[j]` 的反转字符串。
- `0 <= i < j < words.length`

请你返回数组 `words` 中的 __最大__ 匹配数目。

注意，每个字符串最多匹配一次。

__示例:__

示例 1：

```text
输入：words = ["cd","ac","dc","ca","zz"]
输出：2
解释：在此示例中，我们可以通过以下方式匹配 2 对字符串：
```

- 我们将第 0 个字符串与第 2 个字符串匹配，因为 word[0] 的反转字符串是 "dc" 并且等于 words[2]。
- 我们将第 1 个字符串与第 3 个字符串匹配，因为 word[1] 的反转字符串是 "ca" 并且等于 words[3]。

可以证明最多匹配数目是 2 。

示例 2：

```text
输入：words = ["ab","ba","cc"]
输出：1
解释：在此示例中，我们可以通过以下方式匹配 1 对字符串：
```

- 我们将第 0 个字符串与第 1 个字符串匹配，因为 words[1] 的反转字符串 "ab" 与 words[0] 相等。

可以证明最多匹配数目是 1 。

示例 3：

```text
输入：words = ["aa","ab"]
输出：0
解释：这个例子中，无法匹配任何字符串。
```

__提示：__

- `1 <= words.length <= 50`
- `words[i].length == 2`
- `words` 包含的字符串互不相同。
- `words[i]` 只包含小写英文字母。

__思路:__

```text
1. 暴力法
由于每个字符串的长度都为 2
又由于每个字符串互不相同
检查每个字符串的反向是否在哈希表中
且字符串不能和自身相等
时间复杂度为 O(N), 空间复杂度为 O(N)
2. 哈希表
由于字符串长度为 2
使用 26 X 26 的数组记录出现过的字母
如果逆向之前已经遍历过就可以累加到结果中
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int maximumNumberOfStringPairs(vector<string>& words) {
        int result = 0, visited[26][26]{ 0 };
        for (const auto& word : words) 
        {
            if (visited[word.back() - 'a'][word.front() - 'a']) ++result;
            else visited[word.front() - 'a'][word.back() - 'a'] = true;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution:
    def maximumNumberOfStringPairs(self, words: List[str]) -> int:
        return sum(word[::-1] in set(words) and word != word[::-1] for word in words) >> 1
```

__Python__:

```Python
class Solution:
    def maximumNumberOfStringPairs(self, words: List[str]) -> int:
        return sum(word[::-1] in set(words) and word != word[::-1] for word in words) >> 1
```
