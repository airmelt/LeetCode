# 2262 Total Appeal of A String 字符串的总引力

__Description:__

The appeal of a string is the number of distinct characters found in the string.

- For example, the appeal of `"abbca"` is `3` because it has `3` distinct characters: `'a'`, `'b'`, and `'c'`.

Given a string `s`, return _the __total appeal of all of its __substrings__.___

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "abbca"
Output: 28
Explanation: The following are the substrings of "abbca":
```

- Substrings of length 1: "a", "b", "b", "c", "a" have an appeal of 1, 1, 1, 1, and 1 respectively. The sum is 5.
- Substrings of length 2: "ab", "bb", "bc", "ca" have an appeal of 2, 1, 2, and 2 respectively. The sum is 7.
- Substrings of length 3: "abb", "bbc", "bca" have an appeal of 2, 2, and 3 respectively. The sum is 7.
- Substrings of length 4: "abbc", "bbca" have an appeal of 3 and 3 respectively. The sum is 6.
- Substrings of length 5: "abbca" has an appeal of 3. The sum is 3.

The total sum is 5 + 7 + 7 + 6 + 3 = 28.

Example 2:

```text
Input: s = "code"
Output: 20
Explanation: The following are the substrings of "code":
- Substrings of length 1: "c", "o", "d", "e" have an appeal of 1, 1, 1, and 1 respectively. The sum is 4.
- Substrings of length 2: "co", "od", "de" have an appeal of 2, 2, and 2 respectively. The sum is 6.
- Substrings of length 3: "cod", "ode" have an appeal of 3 and 3 respectively. The sum is 6.
- Substrings of length 4: "code" has an appeal of 4. The sum is 4.
The total sum is 4 + 6 + 6 + 4 = 20.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists of lowercase English letters.

__题目描述:__

字符串的 引力 定义为：字符串中 不同 字符的数量。

- 例如， `"abbca"` 的引力为 `3` ，因为其中有 `3` 个不同字符 `'a'`、 `'b'` 和 `'c'` 。

给你一个字符串 `s` ，返回 __其所有子字符串的总引力__ __。__

子字符串 定义为：字符串中的一个连续字符序列。

__示例:__

示例 1：

```text
输入：s = "abbca"
输出：28
解释："abbca" 的子字符串有：
```

- 长度为 1 的子字符串："a"、"b"、"b"、"c"、"a" 的引力分别为 1、1、1、1、1，总和为 5 。
- 长度为 2 的子字符串："ab"、"bb"、"bc"、"ca" 的引力分别为 2、1、2、2 ，总和为 7 。
- 长度为 3 的子字符串："abb"、"bbc"、"bca" 的引力分别为 2、2、3 ，总和为 7 。
- 长度为 4 的子字符串："abbc"、"bbca" 的引力分别为 3、3 ，总和为 6 。
- 长度为 5 的子字符串："abbca" 的引力为 3 ，总和为 3 。

引力总和为 5 + 7 + 7 + 6 + 3 = 28 。

示例 2：

```text
输入：s = "code"
输出：20
解释："code" 的子字符串有：
- 长度为 1 的子字符串："c"、"o"、"d"、"e" 的引力分别为 1、1、1、1 ，总和为 4 。
- 长度为 2 的子字符串："co"、"od"、"de" 的引力分别为 2、2、2 ，总和为 6 。
- 长度为 3 的子字符串："cod"、"ode" 的引力分别为 3、3 ，总和为 6 。
- 长度为 4 的子字符串："code" 的引力为 4 ，总和为 4 。
引力总和为 4 + 6 + 6 + 4 = 20 。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 由小写英文字母组成

__思路:__

```text
前缀和
每个字符的引力和下标有关
设 s[i - 1] 对应的引力为 x
如果 s[i] 没出现过, 那么它可以给 s[i - 1] 的每个引力加 1, 即加上 i 和自身的 1
如果 s[i] 出现过, 那么上一次出现的位置为 j, 那么它可以给 s[j] 到 s[i] 的每个引力加 1, 即加上 i - j
因此, 可以用一个哈希表记录每个字符上一次出现的位置
遍历字符串, 计算引力并累加
gravity += i - last[s[i]], 如果 s[i] 没出现过, last[s[i]] = -1
时间复杂度为 O(N), 空间复杂度为 O(1), 这里的空间复杂度为 O(1) 是因为字符集是固定的, 为 26 个小写字母
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long appealSum(string s) 
    {
        long long result = 0LL, gravity = 0LL;
        unordered_map<char, int> last;
        for (int i = 0, n = s.size(); i < n; i++) 
        {
            result += (gravity += i - (last.count(s[i]) ? last[s[i]] : -1LL));
            last[s[i]] = i;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long appealSum(String s) {
        long result = 0L, gravity = 0L;
        Map<Character, Integer> last = new HashMap<>();
        for (int i = 0, n = s.length(); i < n; i++) {
            result += (gravity += i - last.getOrDefault(s.charAt(i), -1));
            last.put(s.charAt(i), i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def appealSum(self, s: str) -> int:
        result, gravity, last = 0, 0, defaultdict(lambda: -1)
        for i, c in enumerate(s):
            result += (gravity := gravity + i - last[c])
            last[c] = i
        return result
```
