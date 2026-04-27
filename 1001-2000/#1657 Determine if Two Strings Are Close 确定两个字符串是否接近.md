# 1657 Determine if Two Strings Are Close 确定两个字符串是否接近

__Description:__

Two strings are considered close if you can attain one from the other using the following operations:

- Operation 1: Swap any two __existing__ characters.
  - For example, a __b__ cd __e__ -> a __e__ cd __b__
- Operation 2: Transform __every__ occurrence of one __existing__ character into another __existing__ character, and do the same with the other character.
  - For example, __aa__ c __abb__ -> __bb__ c __baa__ (all `a`'s turn into `b`'s, and all `b`'s turn into `a`'s)

You can use the operations on either string as many times as necessary.

Given two strings, `word1` and `word2`, return `true` _if_ `word1` _and_ `word2` _are __close__, and_ `false` _otherwise._

__Example:__

Example 1:

```text
Input: word1 = "abc", word2 = "bca"
Output: true
Explanation: You can attain word2 from word1 in 2 operations.
Apply Operation 1: "abc" -> "acb"
Apply Operation 1: "acb" -> "bca"
```

Example 2:

```text
Input: word1 = "a", word2 = "aa"
Output: false
Explanation: It is impossible to attain word2 from word1, or vice versa, in any number of operations.
```

Example 3:

```text
Input:  word1 = "cabbba", word2 = "abbccc"
Output:  true
Explanation:  You can attain word2 from word1 in 3 operations.
Apply Operation 1: "cabbba" -> "caabbb"
 `Apply Operation 2: "`caabbb" -> "baaccc"
Apply Operation 2: "baaccc" -> "abbccc"
```

__Constraints:__

- `1 <= word1.length, word2.length <= 10 ^ 5`
- `word1` and `word2` contain only lowercase English letters.

__题目描述:__

如果可以使用以下操作从一个字符串得到另一个字符串，则认为两个字符串 接近 ：

- 操作 1:交换任意两个 __现有__ 字符。
  - 例如， a __b__ cd __e__ -> a __e__ cd __b__
- 操作 2:将一个 __现有__ 字符的每次出现转换为另一个 __现有__ 字符，并对另一个字符执行相同的操作。
  - 例如， __aa__ c __abb__ -> __bb__ c __baa__（所有 `a` 转化为 `b` ，而所有的 `b` 转换为 `a` ）

你可以根据需要对任意一个字符串多次使用这两种操作。

给你两个字符串， `word1` 和 `word2` 。如果 `word1` 和 `word2` __接近__ ，就返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：word1 = "abc", word2 = "bca"
输出：true
解释：2 次操作从 word1 获得 word2 。
执行操作 1："abc" -> "acb"
执行操作 1："acb" -> "bca"
```

示例 2：

```text
输入：word1 = "a", word2 = "aa"
输出：false
解释：不管执行多少次操作，都无法从 word1 得到 word2 ，反之亦然。
```

示例 3：

```text
输入: word1 = "cabbba", word2 = "abbccc"
输出: true
解释: 3 次操作从 word1 获得 word2 。
执行操作 1:"cabbba" -> "caabbb"
执行操作 2: `"`caabbb" -> "baaccc"
执行操作 2:"baaccc" -> "abbccc"
```

示例 4：

```text
输入：word1 = "cabbba", word2 = "aabbss"
输出：false
解释：不管执行多少次操作，都无法从 word1 得到 word2 ，反之亦然。
```

__提示：__

- `1 <= word1.length, word2.length <= 10 ^ 5`
- `word1` 和 `word2` 仅包含小写英文字母

__思路:__

```text
模拟
首先判断两个字符串的长度是否相等，不相等则返回 False
统计字符串中的字符的种类
字符串的字符种类相同，且排序后的出现次数相同，则返回 True
时间复杂度为 O(N), 空间复杂度为 O(1), 只需要 26 个字母的空间
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool closeStrings(string word1, string word2) 
    {
        if (word1.size() != word2.size()) return false;
        vector<int> c1(26, 0), c2(26, 0);
        for (int i = 0, n = word1.size(); i < n; i++) 
        {
            ++c1[word1[i] - 'a'];
            ++c2[word2[i] - 'a'];
        }
        for (int i = 0; i < 26; i++) if (!c1[i] and c2[i] or c1[i] and !c2[i]) return false;
        sort(c1.begin(), c1.end());
        sort(c2.begin(), c2.end());
        return c1 == c2;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean closeStrings(String word1, String word2) {
        if (word1.length() != word2.length()) return false;
        int c1[] = new int[26], c2[] = new int[26];
        for (int i = 0, n = word1.length(); i < n; i++) {
            ++c1[word1.charAt(i) - 'a'];
            ++c2[word2.charAt(i) - 'a'];
        }
        for (int i = 0; i < 26; i++) if (c1[i] == 0 && c2[i] != 0 || c1[i] != 0 && c2[i] == 0) return false;
        Arrays.sort(c1);
        Arrays.sort(c2);
        for (int i = 0; i < 26; i++) if (c1[i] != c2[i]) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def closeStrings(self, word1: str, word2: str) -> bool:
        return len(word1) == len(word2) and set(word1) == set(word2) and sorted(Counter(word1).values()) == sorted(Counter(word2).values())
```
