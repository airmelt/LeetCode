# 3120 Count the Number of Special Characters I 统计特殊字母的数量 I

__Description:__

You are given a string `word`. A letter is called __special__ if it appears __both__ in lowercase and uppercase in `word`.

Return the number of __special__ letters in `word`.

__Example:__

Example 1:

```text
Input: word = "aaAbcBC"

Output: 3

Explanation:
```

The special characters in `word` are `'a'`, `'b'`, and `'c'`.

Example 2:

```text
Input: word = "abc"

Output: 0

Explanation:
```

No character in `word` appears in uppercase.

Example 3:

```text
Input: word = "abBCab"

Output: 1

Explanation:
```

The only special character in `word` is `'b'`.

__Constraints:__

- `1 <= word.length <= 50`
- `word` consists of only lowercase and uppercase English letters.

__题目描述:__

给你一个字符串 `word`。如果 `word` 中同时存在某个字母的小写形式和大写形式，则称这个字母为 __特殊字母__ 。

返回 `word` 中 __特殊字母__ 的数量。

__示例:__

示例 1:

```text
输入：word = "aaAbcBC"

输出：3

解释：
```

`word` 中的特殊字母是 `'a'`、 `'b'` 和 `'c'`。

示例 2:

```text
输入：word = "abc"

输出：0

解释：
```

`word` 中不存在大小写形式同时出现的字母。

示例 3:

```text
输入：word = "abBCab"

输出：1

解释：
```

`word` 中唯一的特殊字母是 `'b'`。

__提示：__

- `1 <= word.length <= 50`
- `word` 仅由小写和大写英文字母组成。

__思路:__

```text
位运算
将大小写字母分别放入两个整型
通过与运算即可得到交集
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfSpecialChars(string word) 
    {
        int mask[2]{}; 
        for (const auto& c : word) mask[c >> 5 & 1] |= 1 << (c & 31);
        return popcount(1u * (mask[0] & mask[1]));
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfSpecialChars(String word) {
        int[] mask = new int[2]; 
        for (var c : word.toCharArray()) mask[c >> 5 & 1] |= 1 << (c & 31);
        return Integer.bitCount(mask[0] & mask[1]);
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfSpecialChars(self, word: str) -> int:
        return sum(chr(ord('a') + i) in word and chr(ord('A') + i) in word for i in range(26))
```
