# 2085 Count Common Words With One Occurrence 统计出现过一次的公共字符串

__Description:__

Given two string arrays `words1` and `words2`, return _the number of strings that appear __exactly once__ in _each_ of the two arrays._

__Example:__

Example 1:

```text
Input: words1 = ["leetcode","is","amazing","as","is"], words2 = ["amazing","leetcode","is"]
Output: 2
Explanation:
```

- "leetcode" appears exactly once in each of the two arrays. We count this string.
- "amazing" appears exactly once in each of the two arrays. We count this string.
- "is" appears in each of the two arrays, but there are 2 occurrences of it in words1. We do not count this string.
- "as" appears once in words1, but does not appear in words2. We do not count this string.

Thus, there are 2 strings that appear exactly once in each of the two arrays.

Example 2:

```text
Input: words1 = ["b","bb","bbb"], words2 = ["a","aa","aaa"]
Output: 0
Explanation: There are no strings that appear in each of the two arrays.
```

Example 3:

```text
Input: words1 = ["a","ab"], words2 = ["a","a","a","ab"]
Output: 1
Explanation: The only string that appears exactly once in each of the two arrays is "ab".
```

__Constraints:__

- `1 <= words1.length, words2.length <= 1000`
- `1 <= words1[i].length, words2[j].length <= 30`
- `words1[i]` and `words2[j]` consists only of lowercase English letters.

__题目描述:__

给你两个字符串数组 `words1` 和 `words2` ，请你返回在两个字符串数组中 __都恰好出现一次__ 的字符串的数目。

__示例:__

示例 1：

```text
输入：words1 = ["leetcode","is","amazing","as","is"], words2 = ["amazing","leetcode","is"]
输出：2
解释：
```

- "leetcode" 在两个数组中都恰好出现一次，计入答案。
- "amazing" 在两个数组中都恰好出现一次，计入答案。
- "is" 在两个数组中都出现过，但在 words1 中出现了 2 次，不计入答案。
- "as" 在 words1 中出现了一次，但是在 words2 中没有出现过，不计入答案。

所以，有 2 个字符串在两个数组中都恰好出现了一次。

示例 2：

```text
输入：words1 = ["b","bb","bbb"], words2 = ["a","aa","aaa"]
输出：0
解释：没有字符串在两个数组中都恰好出现一次。
```

示例 3：

```text
输入：words1 = ["a","ab"], words2 = ["a","a","a","ab"]
输出：1
解释：唯一在两个数组中都出现一次的字符串是 "ab" 。
```

__提示：__

- `1 <= words1.length, words2.length <= 1000`
- `1 <= words1[i].length, words2[j].length <= 30`
- `words1[i]` 和 `words2[j]` 都只包含小写英文字母。

__思路:__

```text
哈希表计数
由于数组的大小不超过 1000
可以将第一个数组直接计数
第二个数组乘 1000 计数
统计出现次数为 1001 的字符串即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countWords(vector<string>& words1, vector<string>& words2) 
    {
        unordered_map<string, int> m;
        for (const auto& s : words1) ++m[s];
        for (const auto& s : words2) m[s] += 1000;
        int result = 0;
        for (const auto& [k, v] : m) result += v == 1001;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countWords(String[] words1, String[] words2) {
        Map<String, Integer> map = new HashMap<>();
        for (String s : words1) map.merge(s, 1, Integer::sum);
        for (String s : words2) map.merge(s, 1000, Integer::sum);
        return Collections.frequency(map.values(), 1001);
    }
}
```

__Python__:

```Python
class Solution:
    def countWords(self, words1: List[str], words2: List[str]) -> int:
        return 0 if not ((c1 := Counter(words1)) and (c2 := Counter(words2))) else sum(val == c2.get(key, 0) == 1 for key, val in c1.items())
```
