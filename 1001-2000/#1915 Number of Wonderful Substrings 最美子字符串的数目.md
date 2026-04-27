# 1915 Number of Wonderful Substrings 最美子字符串的数目

__Description:__

A wonderful string is a string where at most one letter appears an odd number of times.

- For example, `"ccjjc"` and `"abab"` are wonderful, but `"ab"` is not.

Given a string `word` that consists of the first ten lowercase English letters ( `'a'` through `'j'`), return _the __number of wonderful non-empty substrings__ in_ `word`_. If the same substring appears multiple times in_ `word`_, then count __each occurrence__ separately._

A substring is a contiguous sequence of characters in a string.

__Example:__

Example 1:

```text
Input: word = "aba"
Output: 4
Explanation: The four wonderful substrings are underlined below:
```

- "aba" -> "a"
- "aba" -> "b"
- "aba" -> "a"
- "aba" -> "aba"

Example 2:

```text
Input: word = "aabb"
Output: 9
Explanation: The nine wonderful substrings are underlined below:
```

- "aabb" -> "a"
- "aabb" -> "aa"
- "aabb" -> "aab"
- "aabb" -> "aabb"
- "aabb" -> "a"
- "aabb" -> "abb"
- "aabb" -> "b"
- "aabb" -> "bb"
- "aabb" -> "b"

Example 3:

```text
Input: word = "he"
Output: 2
Explanation: The two wonderful substrings are underlined below:
```

- "he" -> "h"
- "he" -> "e"

__Constraints:__

- `1 <= word.length <= 10 ^ 5`
- `word` consists of lowercase English letters from `'a'` to `'j'`.

__题目描述:__

如果某个字符串中 至多一个 字母出现 奇数 次，则称其为 最美 字符串。

- 例如， `"ccjjc"` 和 `"abab"` 都是最美字符串，但 `"ab"` 不是。

给你一个字符串 `word` ，该字符串由前十个小写英文字母组成（ `'a'` 到 `'j'`）。请你返回 `word` 中 __最美非空子字符串__ 的数目_。_如果同样的子字符串在 `word` 中出现多次，那么应当对 __每次出现__ 分别计数_。_

子字符串 是字符串中的一个连续字符序列。

__示例:__

示例 1：

```text
输入：word = "aba"
输出：4
解释：4 个最美子字符串如下所示：
```

- "aba" -> "a"
- "aba" -> "b"
- "aba" -> "a"
- "aba" -> "aba"

示例 2：

```text
输入：word = "aabb"
输出：9
解释：9 个最美子字符串如下所示：
```

- "aabb" -> "a"
- "aabb" -> "aa"
- "aabb" -> "aab"
- "aabb" -> "aabb"
- "aabb" -> "a"
- "aabb" -> "abb"
- "aabb" -> "b"
- "aabb" -> "bb"
- "aabb" -> "b"

示例 3：

```text
输入：word = "he"
输出：2
解释：2 个最美子字符串如下所示：
```

- "he" -> "h"
- "he" -> "e"

__提示：__

- `1 <= word.length <= 10 ^ 5`
- `word` 由从 `'a'` 到 `'j'` 的小写英文字母组成

__思路:__

```text
状态压缩
由于只需要记录出现次数的奇偶性, 我们用一个二进制数组记录前 i 个字符出现的奇偶性
二进制为 1 表示对应字符出现奇数次, 否则表示出现偶数次
求前缀和即求异或
如果两个异或结果相同, 则表示这两个子串中的字符出现次数的奇偶性相同, 出现的各个字符的数量为偶数, 即为最美子字符串
同时由于可以出现 1 次奇数, 遍历所有字符, 将每个字符出现奇数次的情况也加入结果中, 即加上反转之后的前缀和
初始化将 0 加入 map 中, 表示前 0 个字符出现的奇偶性为偶数, 即为最美子字符串
时间复杂度为 O(CN), 空间复杂度为 O(N), 其中 C = 10 为所有字符的种类数
```

__代码:__

__C++__:

```C++
class Solution {
public:
    long long wonderfulSubstrings(string word) {
        unordered_map<int, int> m;
        m[0] = 1;
        long long result = 0;
        for (int i = 0, states = 0, n = word.size(); i < n; i++)
        {
            states ^= 1 << (word[i] - 'a');
            for (int j = 0; j < 10; j++) result += m[states ^ (1 << j)];
            result += m[states];
            ++m[states];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long wonderfulSubstrings(String word) {
       Map<Integer, Integer> map = new HashMap<>();
       long result = 0;
       map.put(0, 1);
       for (int i = 0, status = 0, n = word.length(); i < n; i++) {
           status ^= 1 << (word.charAt(i) - 'a');
           for (int j = 0; j < 10; j++) result += map.getOrDefault(status ^ (1 << j), 0);
           result += map.getOrDefault(status, 0);
           map.merge(status, 1, Integer::sum);
       }
       return result;
    }
}
```

__Python__:

```Python
class Solution:
    def wonderfulSubstrings(self, word: str) -> int:
        m, result, states, n = defaultdict(int), 0, 0, len(word)
        m[0] = 1
        for i in range(n):
            states ^= 1 << (ord(word[i]) - ord('a'))
            result += sum(m[states ^ (1 << j)] for j in range(10)) + m[states]
            m[states] += 1
        return result
```
