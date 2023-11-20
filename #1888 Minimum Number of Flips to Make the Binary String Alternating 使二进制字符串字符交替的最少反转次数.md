# 1888 Minimum Number of Flips to Make the Binary String Alternating 使二进制字符串字符交替的最少反转次数

__Description:__

You are given a binary string `s`. You are allowed to perform two types of operations on the string in any sequence:

- __Type-1: Remove__ the character at the start of the string `s` and __append__ it to the end of the string.
- __Type-2: Pick__ any character in `s` and __flip__ its value, i.e., if its value is `'0'` it becomes `'1'` and vice-versa.

Return _the __minimum__ number of __type-2__ operations you need to perform_ _such that_ `s` _becomes __alternating__._

The string is called alternating if no two adjacent characters are equal.

- For example, the strings `"010"` and `"1010"` are alternating, while the string `"0100"` is not.

__Example:__

Example 1:

```text
Input: s = "111000"
Output: 2
Explanation: Use the first operation two times to make s = "100011".
Then, use the second operation on the third and sixth elements to make s = "101010".
```

Example 2:

```text
Input: s = "010"
Output: 0
Explanation: The string is already alternating.
```

Example 3:

```text
Input: s = "1110"
Output: 1
Explanation: Use the second operation on the second element to make s = "1010".
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s[i]` is either `'0'` or `'1'`.

__题目描述:__

给你一个二进制字符串 `s` 。你可以按任意顺序执行以下两种操作任意次:

- __类型 1 :删除__ 字符串 `s` 的第一个字符并将它 __添加__ 到字符串结尾。
- __类型 2 :选择__ 字符串 `s` 中任意一个字符并将该字符 __反转__ ，也就是如果值为 `'0'` ，则反转得到 `'1'` ，反之亦然。

请你返回使 `s` 变成 __交替__ 字符串的前提下， __类型 2__ 的 __最少__ 操作次数 。

我们称一个字符串是 交替 的，需要满足任意相邻字符都不同。

- 比方说，字符串 `"010"` 和 `"1010"` 都是交替的，但是字符串 `"0100"` 不是。

__示例:__

示例 1：

```text
输入：s = "111000"
输出：2
解释：执行第一种操作两次，得到 s = "100011" 。
然后对第三个和第六个字符执行第二种操作，得到 s = "101010" 。
```

示例 2：

```text
输入：s = "010"
输出：0
解释：字符串已经是交替的。
```

示例 3：

```text
输入：s = "1110"
输出：1
解释：对第二个字符执行第二种操作，得到 s = "1010" 。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s[i]` 要么是 `'0'` ，要么是 `'1'` 。

__思路:__

```text
模拟
最后的字符串要么是 '101010...' 要么是 '0101010...'
不妨设最后的字符串是 '101010...', 若其需要 c 次第二种操作, 则将其转化为 '0101010...' 需要 n - c 次操作
对于第一种操作, 可以用 s + s, 取长度为 n 的滑动窗口
由于滑动窗口刚好为 [i, i + n], 也可以简化为取 i + n 处
计算转化为 '101010...' 需要的次数
只需要比较 s[i] 和 target[i & 1] 即可, 其中 target = '01'
每次移动滑动窗口时, c 减去 s[i] != target[i & 1], 加上 s[i] != target[(i + n) & 1]
取 c, n - c 的最小值即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minFlips(string s) 
    {
        int n = s.size(), c = 0;
        string target = "01";
        for (int i = 0; i < n; i++) c += s[i] != target[i & 1];
        int result = min({c, n - c});
        for (int i = 0; i < n; i++)
        {
            c += (s[i] != target[(i + n) & 1]) - (s[i] != target[i & 1]);
            result = min({result, c, n - c});
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minFlips(String s) {
        int n = s.length(), c = 0;
        String target = "01";
        for (int i = 0; i < n; i++) c += s.charAt(i) != target.charAt(i % 2) ? 1 : 0;
        int result = Math.min(c, n - c);
        for (int i = 0; i < n; i++) {
            c += (s.charAt(i) != target.charAt((i + n) % 2) ? 1 : 0) - (s.charAt(i) != target.charAt(i % 2) ? 1 : 0);
            result = Math.min(result, Math.min(c, n - c));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minFlips(self, s: str) -> int:
        result = min((count := sum(c != (target := '01')[i & 1] for i, c in enumerate(s))), (n := len(s)) - count)
        for i, c in enumerate(s):
            result = min(result, (count := count + (c != target[(i + n) & 1]) - (c != target[i & 1])), n - count)
        return result
```
