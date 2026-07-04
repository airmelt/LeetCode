# 3085 Minimum Deletions to Make String K-Special 成为 K 特殊字符串需要删除的最少字符数

__Description:__

You are given a string `word` and an integer `k`.

We consider `word` to be __k-special__ if `|freq(word[i]) - freq(word[j])| <= k` for all indices `i` and `j` in the string.

Here, `freq(x)` denotes the frequency of the character `x` in `word`, and `|y|` denotes the absolute value of `y`.

Return _the __minimum__ number of characters you need to delete to make_ `word` ___k-special___.

__Example:__

Example 1:

```text
Input: word = "aabcaba", k = 0

Output: 3
```

__Explanation:__ We can make `word` `0`-special by deleting `2` occurrences of `"a"` and `1` occurrence of `"c"`. Therefore, `word` becomes equal to `"baba"` where `freq('a') == freq('b') == 2`.

Example 2:

```text
Input: word = "dabdcbdcdcd", k = 2

Output: 2
```

__Explanation:__ We can make `word` `2`-special by deleting `1` occurrence of `"a"` and `1` occurrence of `"d"`. Therefore, `word` becomes equal to "bdcbdcdcd" where `freq('b') == 2`, `freq('c') == 3`, and `freq('d') == 4`.

Example 3:

```text
Input: word = "aaabaaa", k = 2

Output: 1
```

__Explanation:__ We can make `word` `2`-special by deleting `1` occurrence of `"b"`. Therefore, `word` becomes equal to `"aaaaaa"` where each letter's frequency is now uniformly `6`.

__Constraints:__

- `1 <= word.length <= 10 ^ 5`
- `0 <= k <= 10 ^ 5`
- `word` consists only of lowercase English letters.

__题目描述:__

给你一个字符串 `word` 和一个整数 `k`。

如果 `|freq(word[i]) - freq(word[j])| <= k` 对于字符串中所有下标 `i` 和 `j` 都成立，则认为 `word` 是 __k 特殊字符串__。

此处， `freq(x)` 表示字符 `x` 在 `word` 中的出现频率，而 `|y|` 表示 `y` 的绝对值。

返回使 `word` 成为 __k 特殊字符串__ 需要删除的字符的最小数量。

__示例:__

示例 1：

```text
输入：word = "aabcaba", k = 0

输出：3
```

__解释:__
可以删除 `2` 个 `"a"` 和 `1` 个 `"c"` 使 `word` 成为 `0` 特殊字符串。 `word` 变为 `"baba"`，此时 `freq('a') == freq('b') == 2`。

示例 2：

```text
输入：word = "dabdcbdcdcd", k = 2

输出：2
```

__解释:__
可以删除 `1` 个 `"a"` 和 `1` 个 `"d"` 使 `word` 成为 `2` 特殊字符串。 `word` 变为 `"bdcbdcdcd"`，此时 `freq('b') == 2`， `freq('c') == 3`， `freq('d') == 4`。

示例 3：

```text
输入：word = "aaabaaa", k = 2

输出：1
```

__解释:__
可以删除 1 个 `"b"` 使 `word` 成为 `2`特殊字符串。因此， `word` 变为 `"aaaaaa"`，此时每个字母的频率都是 `6`。

__提示：__

- `1 <= word.length <= 10 ^ 5`
- `0 <= k <= 10 ^ 5`
- `word` 仅由小写英文字母组成。

__思路:__

```text
贪心
先统计每个字母出现次数, 即频率
然后排序
排序之后枚举每一个字符
将比当前字符频率少的全部删除
要么保留当前字符频率 + k 要么保留更大的字符, 这两者取较小值
更新最多能保留的字符数
最后用原字符长度减去字符数
时间复杂度为 O(N + C ^ 2), 空间复杂度为 O(C), C 为字符种类, 本题为 26
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumDeletions(string word, int k) 
    {
        int c[26]{ }, cur = 0;
        for (const auto& w : word) ++c[w - 'a'];
        ranges::sort(c);
        for (int i = 0, s = 0; i < 26; i++, s = 0)
        {
            for (int j = i; j < 26; j++) s += min(c[j], c[i] + k);
            cur = max(cur, s);
        }
        return word.size() - cur;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumDeletions(String word, int k) {
        int c[] = new int[26], cur = 0;
        for (var w : word.toCharArray()) ++c[w - 'a'];
        Arrays.sort(c);
        for (int i = 0, s = 0; i < 26; i++, s = 0) {
            for (int j = i; j < 26; j++) s += Math.min(c[j], k + c[i]);
            cur = Math.max(cur, s);
        }
        return word.length() - cur;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumDeletions(self, word: str, k: int) -> int:
        return len(word) - max(sum(min(t, v + k) for t in c[i:]) for i, v in enumerate(c)) if (c := sorted(Counter(word).values())) else 0
```
