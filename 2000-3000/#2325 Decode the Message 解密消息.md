# 2325 Decode the Message 解密消息

__Description:__

You are given the strings `key` and `message`, which represent a cipher key and a secret message, respectively. The steps to decode `message` are as follows:

- For example, given `key = "happy boy"` (actual key would have __at least one__ instance of each letter in the alphabet), we have the partial substitution table of ( `'h' -> 'a'`, `'a' -> 'b'`, `'p' -> 'c'`, `'y' -> 'd'`, `'b' -> 'e'`, `'o' -> 'f'`).

Return the decoded message.

__Example:__

Example 1:

![2325-1](https://assets.leetcode.com/uploads/2022/05/08/ex1new4.jpg)

```text
Input: key = "the quick brown fox jumps over the lazy dog", message = "vkbs bs t suepuv"
Output: "this is a secret"
Explanation: The diagram above shows the substitution table.
It is obtained by taking the first appearance of each letter in "the quick brown fox jumps over the lazy dog".
```

Example 2:

![2325-2](https://assets.leetcode.com/uploads/2022/05/08/ex2new.jpg)

```text
Input: key = "eljuxhpwnyrdgtqkviszcfmabo", message = "zwx hnfx lqantp mnoeius ycgk vcnjrdb"
Output: "the five boxing wizards jump quickly"
Explanation: The diagram above shows the substitution table.
It is obtained by taking the first appearance of each letter in "eljuxhpwnyrdgtqkviszcfmabo".
```

__Constraints:__

- `26 <= key.length <= 2000`
- `key` consists of lowercase English letters and `' '`.
- `key` contains every letter in the English alphabet ( `'a'` to `'z'`) __at least once__.
- `1 <= message.length <= 2000`
- `message` consists of lowercase English letters and `' '`.

__题目描述:__

给你字符串 `key` 和 `message` ，分别表示一个加密密钥和一段加密消息。解密 `message` 的步骤如下:

- 例如， `key = "happy boy"`（实际的加密密钥会包含字母表中每个字母 __至少一次__），据此，可以得到部分对照表（ `'h' -> 'a'`、 `'a' -> 'b'`、 `'p' -> 'c'`、 `'y' -> 'd'`、 `'b' -> 'e'`、 `'o' -> 'f'`）。

返回解密后的消息。

__示例:__

示例 1：

![2325-3](https://assets.leetcode.com/uploads/2022/05/08/ex1new4.jpg)

```text
输入：key = "the quick brown fox jumps over the lazy dog", message = "vkbs bs t suepuv"
输出："this is a secret"
解释：对照表如上图所示。
提取 "the quick brown fox jumps over the lazy dog" 中每个字母的首次出现可以得到替换表。
```

示例 2：

![2325-4](https://assets.leetcode.com/uploads/2022/05/08/ex2new.jpg)

```text
输入：key = "eljuxhpwnyrdgtqkviszcfmabo", message = "zwx hnfx lqantp mnoeius ycgk vcnjrdb"
输出："the five boxing wizards jump quickly"
解释：对照表如上图所示。
提取 "eljuxhpwnyrdgtqkviszcfmabo" 中每个字母的首次出现可以得到替换表。
```

__提示：__

- `26 <= key.length <= 2000`
- `key` 由小写英文字母及 `' '` 组成
- `key` 包含英文字母表中每个字符（ `'a'` 到 `'z'`）__至少一次__
- `1 <= message.length <= 2000`
- `message` 由小写英文字母和 `' '` 组成

__思路:__

```text
哈希表
使用数组或者哈希表记录 key 对应的解密字符
空格直接解密为空格
遍历 message 将所有字符替换为解密字符
时间复杂度为 O(M + N), 空间复杂度为 O(1), 常数为 128/27 忽略不计, M 为 key 的长度, N 为 message 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string decodeMessage(string key, string message) 
    {
        char d[128]{};
        d[' '] = ' ';
        auto i = 'a';
        for (const auto& c : key) if (!d[c]) d[c] = i++;
        for (auto& c : message) c = d[c];
        return message;
    }
};
```

__Java__:

```Java
class Solution {
    public String decodeMessage(String key, String message) {
        char[] d = new char[128], result = message.toCharArray();
        d[' '] = ' ';
        for (int i = 0, j = 0, n = key.length(); i < n; i++) if (d[key.charAt(i)] == 0) d[key.charAt(i)] = (char)('a' + j++);
        for (int i = 0, m = result.length; i < m; i++) result[i] = d[result[i]];
        return String.valueOf(result);
    }
}
```

__Python__:

```Python
class Solution:
    def decodeMessage(self, key: str, message: str) -> str:
        return ''.join(chr((key := [k for i, k in enumerate(key) if key.index(k) == i and k != ' ']).index(i) + ord('a')) if i != ' ' else ' ' for i in message)
```
