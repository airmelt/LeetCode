# 1641 Count Sorted Vowel Strings 统计字典序元音字符串的数目

__Description:__

Given an integer `n`, return _the number of strings of length_ `n` _that consist only of vowels (_`a`_,_ `e`_,_ `i`_,_ `o`_,_ `u`_) and are __lexicographically sorted__._

A string `s` is __lexicographically sorted__ if for all valid `i`, `s[i]` is the same as or comes before `s[i+1]` in the alphabet.

__Example:__

Example 1:

```text
Input:  n = 1
Output:  5
Explanation:  The 5 sorted strings that consist of vowels only are ["a","e","i","o","u"].
```

Example 2:

```text
Input: n = 2
Output: 15
Explanation: The 15 sorted strings that consist of vowels only are
["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"].
Note that "ea" is not a valid string since 'e' comes after 'a' in the alphabet.
```

Example 3:

```text
Input: n = 33
Output: 66045
```

__Constraints:__

- `1 <= n <= 50`

__题目描述:__

给你一个整数 `n`，请返回长度为 `n` 、仅由元音 ( `a`, `e`, `i`, `o`, `u`) 组成且按 __字典序排列__ 的字符串数量。

字符串 `s` 按 __字典序排列__ 需要满足:对于所有有效的 `i`， `s[i]` 在字母表中的位置总是与 `s[i+1]` 相同或在 `s[i+1]` 之前。

__示例:__

示例 1：

```text
输入: n = 1
输出: 5
解释: 仅由元音组成的 5 个字典序字符串为 ["a","e","i","o","u"]
```

示例 2：

```text
输入：n = 2
输出：15
解释：仅由元音组成的 15 个字典序字符串为
["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"]
注意，"ea" 不是符合题意的字符串，因为 'e' 在字母表中的位置比 'a' 靠后
```

示例 3：

```text
输入：n = 33
输出：66045
```

__提示：__

- `1 <= n <= 50`

__思路:__

```text
数学
由于一定是按字典顺序, 那么结果中一定按照 'aeiou' 的次序排列
也就是说只要确定各个字符的长度, 排序是一定的
转换为如果有长度为 n 的字符串选择 5 种字符的个数
由于可以空, 可以虚拟添加 5 个字符转化为不能空的情况
实际上就是求 C(n + 4, 4)
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countVowelStrings(int n) 
    {
        return (n + 4) * (n + 3) * (n + 2) * (n + 1) / 24;
    }
};
```

__Java__:

```Java
class Solution {
    public int countVowelStrings(int n) {
        return (n + 4) * (n + 3) * (n + 2) * (n + 1) / 24;
    }
}
```

__Python__:

```Python
class Solution:
    def countVowelStrings(self, n: int) -> int:
        return (n + 4) * (n + 3) * (n + 2) * (n + 1) // 24
```
