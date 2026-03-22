# 2186 Minimum Number of Steps to Make Two Strings Anagram II 制造字母异位词的最小步骤数 II

__Description:__

You are given two strings `s` and `t`. In one step, you can append __any character__ to either `s` or `t`.

Return _the minimum number of steps to make_ `s` _and_ `t` ___anagrams__ of each other._

An anagram of a string is a string that contains the same characters with a different (or the same) ordering.

__Example:__

Example 1:

```text
Input: s = "leetcode", t = "coats"
Output: 7
Explanation: 
```

- In 2 steps, we can append the letters in "as" onto s = "leetcode", forming s = "leetcodeas".
- In 5 steps, we can append the letters in "leede" onto t = "coats", forming t = "coatsleede".

"leetcodeas" and "coatsleede" are now anagrams of each other.
We used a total of 2 + 5 = 7 steps.
It can be shown that there is no way to make them anagrams of each other with less than 7 steps.

Example 2:

```text
Input: s = "night", t = "thing"
Output: 0
Explanation: The given strings are already anagrams of each other. Thus, we do not need any further steps.
```

__Constraints:__

- `1 <= s.length, t.length <= 2 * 10 ^ 5`
- `s` and `t` consist of lowercase English letters.

__题目描述:__

给你两个字符串 `s` 和 `t` 。在一步操作中，你可以给 `s` 或者 `t` 追加 __任一字符__ 。

返回使 `s` 和 `t` 互为 __字母异位词__ 所需的最少步骤数_。_

字母异位词 指字母相同但是顺序不同（或者相同）的字符串。

__示例:__

示例 1：

```text
输入：s = "leetcode", t = "coats"
输出：7
解释：
```

- 执行 2 步操作，将 "as" 追加到 s = "leetcode" 中，得到 s = "leetcodeas" 。
- 执行 5 步操作，将 "leede" 追加到 t = "coats" 中，得到 t =
"coatsleede" 。

"leetcodeas" 和 "coatsleede" 互为字母异位词。
总共用去 2 + 5 = 7 步。
可以证明，无法用少于 7 步操作使这两个字符串互为字母异位词。

示例 2：

```text
输入：s = "night", t = "thing"
输出：0
解释：给出的字符串已经互为字母异位词。因此，不需要任何进一步操作。
```

__提示：__

- `1 <= s.length, t.length <= 2 * 10 ^ 5`
- `s` 和 `t` 由小写英文字符组成

__思路:__

```text
计数
我们不关心字符的顺序，只关心字符的数量
用一个数组或者哈希表记录每个字符出现的次数
遍历 s 和 t ，分别统计每个字符出现的次数
可以一起统计其中 s 记为正数，t 记为负数
最后将数组的元素绝对值相加即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minSteps(string s, string t) 
    {
        vector<int> result(26);
        for (const auto& c : s) ++result[c - 'a'];
        for (const auto& c : t) --result[c - 'a'];
        return accumulate(result.begin(), result.end(), 0, [](int a, int b) { return abs(a) + abs(b); });        
    }
};
```

__Java__:

```Java
class Solution {
    public int minSteps(String s, String t) {
        int result[] = new int[26];
        for (char c : s.toCharArray()) ++result[c - 'a'];
        for (char c : t.toCharArray()) --result[c - 'a'];
        return Arrays.stream(result).map(a -> Math.abs(a)).sum();
    }
}
```

__Python__:

```Python
class Solution:
    def minSteps(self, s: str, t: str) -> int:
        return sum(abs(s.count(chr(ord('a') + i)) - t.count(chr(ord('a') + i))) for i in range(26))
```
