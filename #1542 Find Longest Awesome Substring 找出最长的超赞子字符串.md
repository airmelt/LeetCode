# 1542 Find Longest Awesome Substring 找出最长的超赞子字符串

__Description:__

You are given a string `s`. An __awesome__ substring is a non-empty substring of `s` such that we can make any number of swaps in order to make it a palindrome.

Return _the length of the maximum length __awesome substring__ of_ `s`.

__Example:__

Example 1:

```text
Input: s = "3242415"
Output: 5
Explanation: "24241" is the longest awesome substring, we can form the palindrome "24142" with some swaps.
```

Example 2:

```text
Input: s = "12345678"
Output: 1
```

Example 3:

```text
Input: s = "213123"
Output: 6
Explanation: "213123" is the longest awesome substring, we can form the palindrome "231132" with some swaps.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists only of digits.

__题目描述:__

给你一个字符串 `s` 。请返回 `s` 中最长的 __超赞子字符串__ 的长度。

「超赞子字符串」需满足满足下述两个条件：

- 该字符串是 `s` 的一个非空子字符串
- 进行任意次数的字符交换后，该字符串可以变成一个回文字符串

__示例:__

示例 1：

```text
输入：s = "3242415"
输出：5
解释："24241" 是最长的超赞子字符串，交换其中的字符后，可以得到回文 "24142"
```

示例 2：

```text
输入：s = "12345678"
输出：1
```

示例 3：

```text
输入：s = "213123"
输出：6
解释："213123" 是最长的超赞子字符串，交换其中的字符后，可以得到回文 "231132"
```

示例 4：

```text
输入：s = "00"
输出：2
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 仅由数字组成

__思路:__

```text
状态压缩 ➕ 前缀和
用一个整数记录每个数字出现的次数, 只需要记录出现的次数的奇偶性
对于一个回文子串, 出现次数为奇数的次数不大于 1
在 count 数组中记录出现这种次数的首次位置
初始化 count[0] = 0, 表示空字符串出现的位置为 0
遍历字符串, 并同时记录下状态出现的位置
对于每个数字可以最多有一次为奇数次出现次数
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestAwesome(string s) 
    {
        int n = s.size(), result = 0, mask = 0;
        vector<int> count(1 << 10, n + 5);
        count.front() = 0;
        for (int i = 1; i <= n; i++) 
        {
            mask ^= (1 << (s[i - 1] - '0'));
            result = max(result, i - count[mask]);
            count[mask] = min(i, count[mask]);
            for (int j = 0; j < 10; j++) result = max(result, i - count[mask ^ (1 << j)]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestAwesome(String s) {
        int n = s.length(), count[] = new int[1 << 10], result = 0, mask = 0;
        Arrays.fill(count, n + 5);
        count[0] = 0;
        for (int i = 1; i <= n; i++) {
            mask ^= (1 << (s.charAt(i - 1) - '0'));
            result = Math.max(result, i - count[mask]);
            count[mask] = Math.min(i, count[mask]);
            for (int j = 0; j < 10; j++) result = Math.max(result, i - count[mask ^ (1 << j)]);
        }
        return result;
    }   
}
```

__Python__:

```Python
class Solution:
    def longestAwesome(self, s: str) -> int:
        dp, n = [float('inf')] * (1 << 10), len(s)
        dp[0] = mask = result = 0
        for i in range(1, n + 1):
            mask ^= 1 << int(s[i - 1])
            result, dp[mask] = max(result, i - dp[mask]), min(i, dp[mask])
            for j in range(10):
                result = max(result, i - dp[(1 << j) ^ mask])
        return result
```
