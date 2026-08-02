# 3121 Count the Number of Special Characters II 统计特殊字母的数量 II

__Description:__

You are given a string `word`. A letter `c` is called __special__ if it appears __both__ in lowercase and uppercase in `word`, and __every__ lowercase occurrence of `c` appears before the __first__ uppercase occurrence of `c`.

Return the number of __special__ letters in `word`.

__Example:__

Example 1:

```text
Input: word = "aaAbcBC"

Output: 3

Explanation:
```

The special characters are `'a'`, `'b'`, and `'c'`.

Example 2:

```text
Input: word = "abc"

Output: 0

Explanation:
```

There are no special characters in `word`.

Example 3:

```text
Input: word = "AbBCab"

Output: 0

Explanation:
```

There are no special characters in `word`.

__Constraints:__

- `1 <= word.length <= 2 * 10 ^ 5`
- `word` consists of only lowercase and uppercase English letters.

__题目描述:__

给你一个字符串 `word`。如果 `word` 中同时出现某个字母 `c` 的小写形式和大写形式，并且 __每个__ 小写形式的 `c` 都出现在第一个大写形式的 `c` 之前，则称字母 `c` 是一个 __特殊字母__ 。

返回 `word` 中 __特殊字母__ 的数量。

__示例:__

示例 1:

```text
输入：word = "aaAbcBC"

输出：3

解释：
```

特殊字母是 `'a'`、 `'b'` 和 `'c'`。

示例 2:

```text
输入：word = "abc"

输出：0

解释：
```

`word` 中不存在特殊字母。

示例 3:

```text
输入：word = "AbBCab"

输出：0

解释：
```

`word` 中不存在特殊字母。

__提示：__

- `1 <= word.length <= 2 * 10 ^ 5`
- `word` 仅由小写和大写英文字母组成。

__思路:__

```text
位运算
用三个位分别表示出现过小写字符/大写字符/小写字符都在大写字符之前
初始设置小写字符都在大写字符之前的位为 true
当出现小写字符时设置小写字符位为真
如果这里出现过大写字符那么将小写字符都在大写字符之前的判断位设置为 false
如果是大写字符, 先检查是否有对应的小写字符如果有, 将小写字符都在大写字符之前的判断位设置为 false
当将小写字符都在大写字符之前的判断位设置为 true 时, 设置大写字符位为 true
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
        vector<int> record(26, 2);
        int result = 0, cur = 0;
        for (const auto& c : word) 
        {
            if (c >= 'a' and c <= 'z') 
            {
                record[cur = c - 'a'] |= 1;
                if (record[cur] & 4) record[cur] &= 1;
            } 
            else 
            {
                if (!((record[cur = c - 'A']) & 1)) record[cur] &= 5;
                if (record[cur] & 2) record[cur] |= 4;
            }
        }
        for (const auto& r : record) result += (r >> 2);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfSpecialChars(String word) {
        int record[] = new int[26], cur = 0, result = 0;
        Arrays.fill(record, 2);
        for (var c : word.toCharArray()) {
            if (c >= 'a' && c <= 'z') {
                record[cur = c - 'a'] |= 1;
                if ((record[cur] & 4) != 0) record[cur] &= 1;
            } else {
                if (((record[cur = c - 'A']) & 1) == 0) record[cur] &= 5;
                if ((record[cur] & 2) != 0) record[cur] |= 4;
            }
        }
        for (int r : record) result += r >> 2;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfSpecialChars(self, word: str) -> int:
        record = [2] * 26
        for c in word:
            if 'a' <= c <= 'z':
                record[c := ord(c) - ord('a')] |= 1
                if record[c] & 4:
                    record[c] &= 1
            else:
                if not (record[c := ord(c) - ord('A')] & 1):
                    record[c] &= 5
                if record[c] & 2:
                    record[c] |= 4
        return sum(r >> 2 for r in record)
```
