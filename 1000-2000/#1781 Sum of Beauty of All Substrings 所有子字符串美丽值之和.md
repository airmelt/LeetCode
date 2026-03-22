# 1781 Sum of Beauty of All Substrings 所有子字符串美丽值之和

__Description:__

The beauty of a string is the difference in frequencies between the most frequent and least frequent characters.

- For example, the beauty of `"abaacc"` is `3 - 1 = 2`.

Given a string `s`, return _the sum of __beauty__ of all of its substrings._

__Example:__

Example 1:

```text
Input: s = "aabcb"
Output: 5
Explanation: The substrings with non-zero beauty are ["aab","aabc","aabcb","abcb","bcb"], each with beauty equal to 1.
```

Example 2:

```text
Input: s = "aabcbaa"
Output: 17
```

__Constraints:__

- `1 <= s.length <= ^ 500`
- `s` consists of only lowercase English letters.

__题目描述:__

一个字符串的 美丽值 定义为：出现频率最高字符与出现频率最低字符的出现次数之差。

- 比方说， `"abaacc"` 的美丽值为 `3 - 1 = 2` 。

给你一个字符串 `s` ，请你返回它所有子字符串的 __美丽值__ 之和。

__示例:__

示例 1：

```text
输入：s = "aabcb"
输出：5
解释：美丽值不为零的字符串包括 ["aab","aabc","aabcb","abcb","bcb"] ，每一个字符串的美丽值都为 1 。
```

示例 2：

```text
输入：s = "aabcbaa"
输出：17
```

__提示：__

- `1 <= s.length <= ^ 500`
- `s` 只包含小写英文字母。

__思路:__

```text
暴力法
用一个数组记录字符出现的次数
使用双层循环查询子串的字符出现次数最大值和最小值
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1), 只需要 26 个字母的空间
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int beautySum(string s) 
    {
        int result = 0, n = s.size();
        for (int i = 0; i < n; i++) 
        {
            int c[26]{0}, max_value = 0;
            for (int j = i; j < n; j++) {
                max_value = max(max_value, ++c[s[j] - 'a']);
                int min_value = n;
                for (int k = 0; k < 26; k++) if (c[k] > 0) min_value = min(min_value, c[k]);
                result += max_value - min_value;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int beautySum(String s) {
        int result = 0, n = s.length();
        for (int i = 0; i < n; i++) {
            int c[] = new int[26], maxValue = 0;
            for (int j = i; j < n; j++) {
                maxValue = Math.max(maxValue, ++c[s.charAt(j) - 'a']);
                int minValue = n;
                for (int k = 0; k < 26; k++) if (c[k] > 0) minValue = Math.min(minValue, c[k]);
                result += maxValue - minValue;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def beautySum(self, s: str) -> int:
        result, n = 0, len(s)
        for i in range(n):
            c = Counter()
            for j in range(i, n):
                c[s[j]] += 1
                result += max(c.values()) - min(c.values())
        return result
```
