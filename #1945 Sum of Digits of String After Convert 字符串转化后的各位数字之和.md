# 1945 Sum of Digits of String After Convert 字符串转化后的各位数字之和

__Description:__

You are given a string `s` consisting of lowercase English letters, and an integer `k`.

First, __convert__ `s` into an integer by replacing each letter with its position in the alphabet (i.e., replace `'a'` with `1`, `'b'` with `2`, ..., `'z'` with `26`). Then, __transform__ the integer by replacing it with the __sum of its digits__. Repeat the __transform__ operation `k` __times__ in total.

For example, if `s = "zbax"` and `k = 2`, then the resulting integer would be `8` by the following operations:

- __Convert__: `"zbax" ➝ "(26)(2)(1)(24)" ➝ "262124" ➝ 262124`
- __Transform #1__: `262124 ➝ 2 + 6 + 2 + 1 + 2 + 4 ➝ 17`
- __Transform #2__: `17 ➝ 1 + 7 ➝ 8`

Return the resulting integer after performing the operations described above.

__Example:__

Example 1:

```text
Input: s = "iiii", k = 1
Output: 36
Explanation: The operations are as follows:
```

- Convert: "iiii" ➝ "(9)(9)(9)(9)" ➝ "9999" ➝ 9999
- Transform #1: 9999 ➝ 9 + 9 + 9 + 9 ➝ 36
Thus the resulting integer is 36.

Example 2:

```text
Input: s = "leetcode", k = 2
Output: 6
Explanation: The operations are as follows:
```

- Convert: "leetcode" ➝ "(12)(5)(5)(20)(3)(15)(4)(5)" ➝ "12552031545" ➝ 12552031545
- Transform #1: 12552031545 ➝ 1 + 2 + 5 + 5 + 2 + 0 + 3 + 1 + 5 + 4 + 5 ➝ 33
- Transform #2: 33 ➝ 3 + 3 ➝ 6
Thus the resulting integer is 6.

Example 3:

```text
Input: s = "zbax", k = 2
Output: 8
```

__Constraints:__

- `1 <= s.length <= 100`
- `1 <= k <= 10`
- `s` consists of lowercase English letters.

__题目描述:__

给你一个由小写字母组成的字符串 `s` ，以及一个整数 `k` 。

首先，用字母在字母表中的位置替换该字母，将 `s` __转化__ 为一个整数（也就是， `'a'` 用 `1` 替换， `'b'` 用 `2` 替换，... `'z'` 用 `26` 替换）。接着，将整数 __转换__ 为其 __各位数字之和__ 。共重复 __转换__ 操作 __`k` 次__ 。

例如，如果 `s = "zbax"` 且 `k = 2` ，那么执行下述步骤后得到的结果是整数 `8` :

- __转化:__`"zbax" ➝ "(26)(2)(1)(24)" ➝ "262124" ➝ 262124`
- __转换 #1__: `262124 ➝ 2 + 6 + 2 + 1 + 2 + 4 ➝ 17`
- __转换 #2__: `17 ➝ 1 + 7 ➝ 8`

返回执行上述操作后得到的结果整数。

__示例:__

示例 1：

```text
输入：s = "iiii", k = 1
输出：36
解释：操作如下：
```

- 转化："iiii" ➝ "(9)(9)(9)(9)" ➝ "9999" ➝ 9999
- 转换 #1：9999 ➝ 9 + 9 + 9 + 9 ➝ 36
因此，结果整数为 36 。

示例 2：

```text
输入：s = "leetcode", k = 2
输出：6
解释：操作如下：
```

- 转化："leetcode" ➝ "(12)(5)(5)(20)(3)(15)(4)(5)" ➝ "12552031545" ➝ 12552031545
- 转换 #1：12552031545 ➝ 1 + 2 + 5 + 5 + 2 + 0 + 3 + 1 + 5 + 4 + 5 ➝ 33
- 转换 #2：33 ➝ 3 + 3 ➝ 6
因此，结果整数为 6 。

__提示：__

- `1 <= s.length <= 100`
- `1 <= k <= 10`
- `s` 由小写英文字母组成

__思路:__

```text
模拟
先将字符串转化为数字
如果数字长度不为 1, 进行 k 次数字和转换
时间复杂度为 O(N), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int getLucky(string s, int k) 
    {
        string digits;
        for (const auto& c : s) digits += to_string(c - 'a' + 1);
        for (int i = 1; i <= k && digits.size() > 1; i++) 
        {
            int sum = 0;
            for (const auto& c : digits) sum += c - '0';
            digits = to_string(sum);
        }
        return stoi(digits);
    }
};
```

__Java__:

```Java
class Solution {
    public int getLucky(String s, int k) {
        StringBuilder sb = new StringBuilder();
        for (char c : s.toCharArray()) sb.append(c - 'a' + 1);
        String digits = sb.toString();
        for (int i = 1; i <= k && digits.length() > 1; ++i) {
            int sum = 0;
            for (char c : digits.toCharArray()) sum += c - '0';
            digits = Integer.toString(sum);
        }
        return Integer.parseInt(digits);
    }
}
```

__Python__:

```Python
class Solution:
    def getLucky(self, s: str, k: int) -> int:
        return self.getLucky(''.join(str(ord(c) - ord('a') + 1) for c in s), k) if s[0].islower() else (int(s) if not k else self.getLucky(str(sum(map(int, s))), k - 1))
```
