# 2135 Count Words Obtained After Adding a Letter 统计追加字母可以获得的单词数

__Description:__

You are given two __0-indexed__ arrays of strings `startWords` and `targetWords`. Each string consists of __lowercase English letters__ only.

For each string in `targetWords`, check if it is possible to choose a string from `startWords` and perform a __conversion operation__ on it to be equal to that from `targetWords`.

The conversion operation is described in the following two steps:

- For example, if the string is `"abc"`, the letters `'d'`, `'e'`, or `'y'` can be added to it, but not `'a'`. If `'d'` is added, the resulting string will be `"abcd"`.

- For example, `"abcd"` can be rearranged to `"acbd"`, `"bacd"`, `"cbda"`, and so on. Note that it can also be rearranged to `"abcd"` itself.

Return _the __number of strings__ in_ `targetWords` _that can be obtained by performing the operations on __any__ string of_ `startWords`.

__Note__ that you will only be verifying if the string in `targetWords` can be obtained from a string in `startWords` by performing the operations. The strings in `startWords` __do not__ actually change during this process.

__Example:__

Example 1:

```text
Input: startWords = ["ant","act","tack"], targetWords = ["tack","act","acti"]
Output: 2
Explanation:
```

- In order to form targetWords[0] = "tack", we use startWords[1] = "act", append 'k' to it, and rearrange "actk" to "tack".
- There is no string in startWords that can be used to obtain targetWords[1] = "act".

  Note that "act" does exist in startWords, but we must append one letter to the string before rearranging it.
- In order to form targetWords[2] = "acti", we use startWords[1] = "act", append 'i' to it, and rearrange "acti" to "acti" itself.

Example 2:

```text
Input: startWords = ["ab","a"], targetWords = ["abc","abcd"]
Output: 1
Explanation:
```

- In order to form targetWords[0] = "abc", we use startWords[0] = "ab", add 'c' to it, and rearrange it to "abc".
- There is no string in startWords that can be used to obtain targetWords[1] = "abcd".

__Constraints:__

- `1 <= startWords.length, targetWords.length <= 5 * 10 ^ 4`
- `1 <= startWords[i].length, targetWords[j].length <= 26`
- Each string of `startWords` and `targetWords` consists of lowercase English letters only.
- No letter occurs more than once in any string of `startWords` or `targetWords`.

__题目描述:__

给你两个下标从 __0__ 开始的字符串数组 `startWords` 和 `targetWords` 。每个字符串都仅由 __小写英文字母__ 组成。

对于 `targetWords` 中的每个字符串，检查是否能够从 `startWords` 中选出一个字符串，执行一次 __转换操作__ ，得到的结果与当前 `targetWords` 字符串相等。

转换操作 如下面两步所述：

- 例如，如果字符串为 `"abc"` ，那么字母 `'d'`、 `'e'` 或 `'y'` 都可以加到该字符串末尾，但 `'a'` 就不行。如果追加的是 `'d'` ，那么结果字符串为 `"abcd"` 。

- 例如， `"abcd"` 可以重排为 `"acbd"`、 `"bacd"`、 `"cbda"`，以此类推。注意，它也可以重排为 `"abcd"` 自身。

找出 `targetWords` 中有多少字符串能够由 `startWords` 中的 __任一__ 字符串执行上述转换操作获得。返回 `targetWords` 中这类 __字符串的数目__ 。

__注意:__你仅能验证 `targetWords` 中的字符串是否可以由 `startWords` 中的某个字符串经执行操作获得。 `startWords` 中的字符串在这一过程中 __不__ 发生实际变更。

__示例:__

示例 1：

```text
输入：startWords = ["ant","act","tack"], targetWords = ["tack","act","acti"]
输出：2
解释：
```

- 为了形成 targetWords[0] = "tack" ，可以选用 startWords[1] = "act" ，追加字母 'k' ，并重排 "actk" 为 "tack" 。
- startWords 中不存在可以用于获得 targetWords[1] = "act" 的字符串。

  注意 "act" 确实存在于 startWords ，但是 必须 在重排前给这个字符串追加一个字母。
- 为了形成 targetWords[2] = "acti" ，可以选用 startWords[1] = "act" ，追加字母 'i' ，并重排 "acti" 为 "acti" 自身。

示例 2：

```text
输入：startWords = ["ab","a"], targetWords = ["abc","abcd"]
输出：1
解释：
```

- 为了形成 targetWords[0] = "abc" ，可以选用 startWords[0] = "ab" ，追加字母 'c' ，并重排为 "abc" 。
- startWords 中不存在可以用于获得 targetWords[1] = "abcd" 的字符串。

__提示：__

- `1 <= startWords.length, targetWords.length <= 5 * 10 ^ 4`
- `1 <= startWords[i].length, targetWords[j].length <= 26`
- `startWords` 和 `targetWords` 中的每个字符串都仅由小写英文字母组成
- 在 `startWords` 或 `targetWords` 的任一字符串中，每个字母至多出现一次

__思路:__

```text
位运算 ➕ 哈希
注意到 targetWords 中的字符串中的每个字符最多出现一次
因此我们可以使用位运算用一个整数表示字符串中的字符
对于 startWords 中的每个字符串，我们可以将其字符转换为整数表示
由于必须要添加一个字符，我们可以将其字符的每个位置都尝试添加一个字符
如果能够添加一个不在当前字符串中的字符，那么我们可以将其添加到当前字符串中
最后我们可以将 targetWords 中的字符串转换为整数表示，判断是否在哈希表中
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int wordCount(vector<string>& startWords, vector<string>& targetWords) 
    {
        int result = 0;
        unordered_set<int> words;
        for (const auto& start : startWords) 
        {
            int cur = mask(start);
            for (int i = 0; i < 26; i++) if (!(cur & (1 << i))) words.insert(cur | (1 << i));
        } 
        for (const auto& target : targetWords) result += words.count(mask(target));
        return result;
    }
private:
    int mask(const string& word) 
    {
        int result = 0;
        for (const auto& c : word) result |= 1 << (c - 'a');
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int wordCount(String[] startWords, String[] targetWords) {
        int result = 0;
        Set<Integer> words = new HashSet<>();
        for (String start : startWords) {
            int cur = mask(start);
            for (int i = 0; i < 26; i++) if ((cur & (1 << i)) == 0) words.add(cur | (1 << i));
        } 
        for (String target : targetWords) if (words.contains(mask(target))) ++result;
        return result;
    }

    private int mask(String word) {
        int result = 0;
        for (char c : word.toCharArray()) result |= 1 << (c - 'a');
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def wordCount(self, startWords: List[str], targetWords: List[str]) -> int:
        mask, words = lambda word: sum(1 << i for i in range(26) if chr(ord('a') + i) in set(word)), set()
        for start in startWords:
            start = mask(start)
            for i in range(26):
                if not ((start >> i) & 1):
                    words.add(start | (1 << i))
        return sum(mask(target) in words for target in targetWords)
```
