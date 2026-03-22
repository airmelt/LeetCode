# 2002 Maximum Product of the Length of Two Palindromic Subsequences 两个回文子序列长度的最大乘积

__Description:__

Given a string `s`, find two __disjoint palindromic subsequences__ of `s` such that the __product__ of their lengths is __maximized__. The two subsequences are __disjoint__ if they do not both pick a character at the same index.

Return the maximum possible product of the lengths of the two palindromic subsequences.

A subsequence is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters. A string is palindromic if it reads the same forward and backward.

__Example:__

Example 1:

![2002-1](https://assets.leetcode.com/uploads/2021/08/24/two-palindromic-subsequences.png)

```text
Input: s = "leetcodecom"
Output: 9
Explanation: An optimal solution is to choose "ete" for the 1st subsequence and "cdc" for the 2nd subsequence.
The product of their lengths is: 3 * 3 = 9.
```

Example 2:

```text
Input: s = "bb"
Output: 1
Explanation: An optimal solution is to choose "b" (the first character) for the 1st subsequence and "b" (the second character) for the 2nd subsequence.
The product of their lengths is: 1 * 1 = 1.
```

Example 3:

```text
Input: s = "accbcaxxcxx"
Output: 25
Explanation: An optimal solution is to choose "accca" for the 1st subsequence and "xxcxx" for the 2nd subsequence.
The product of their lengths is: 5 * 5 = 25.
```

__Constraints:__

- `2 <= s.length <= 12`
- `s` consists of lowercase English letters only.

__题目描述:__

给你一个字符串 `s` ，请你找到 `s` 中两个 __不相交回文子序列__ ，使得它们长度的 __乘积最大__ 。两个子序列在原字符串中如果没有任何相同下标的字符，则它们是 __不相交__ 的。

请你返回两个回文子序列长度可以达到的 最大乘积 。

子序列 指的是从原字符串中删除若干个字符（可以一个也不删除）后，剩余字符不改变顺序而得到的结果。如果一个字符串从前往后读和从后往前读一模一样，那么这个字符串是一个 回文字符串 。

__示例:__

示例 1：

![2002-2](https://assets.leetcode.com/uploads/2021/08/24/two-palindromic-subsequences.png)

```text
输入：s = "leetcodecom"
输出：9
解释：最优方案是选择 "ete" 作为第一个子序列，"cdc" 作为第二个子序列。
它们的乘积为 3 * 3 = 9 。
```

示例 2：

```text
输入：s = "bb"
输出：1
解释：最优方案为选择 "b" （第一个字符）作为第一个子序列，"b" （第二个字符）作为第二个子序列。
它们的乘积为 1 * 1 = 1 。
```

示例 3：

```text
输入：s = "accbcaxxcxx"
输出：25
解释：最优方案为选择 "accca" 作为第一个子序列，"xxcxx" 作为第二个子序列。
它们的乘积为 5 * 5 = 25 。
```

__提示：__

- `2 <= s.length <= 12`
- `s` 只含有小写英文字母。

__思路:__

```text
状态压缩
将所有的回文子序列及长度记录下来
遍历记录找到不相交的回文子序列
返回最大乘积
时间复杂度为 O(2 ^ N), 空间复杂度为 O(2 ^ N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxProduct(string s) 
    {
        int n = s.size(), total = 1 << n, result = 0;
        vector<pair<int, int>> record;
        for (int mask = 1; mask < total; mask++) 
        {
            string cur;
            for (int i = 0; i < n; i++) if (mask & (1 << i)) cur += s[i];
            if (check(cur)) record.emplace_back(make_pair(cur.length(), mask));
        }
        for (const auto& x : record) for (const auto& y : record) if (x != y and !(x.second & y.second)) result = max(result, x.first * y.first);
        return result;
    }
private:
    bool check(string sb) 
    {
        int left = 0, right = sb.size() - 1;
        while (left < right) 
        {
            if (sb[left] == sb[right]) 
            {
                ++left;
                --right;
            } 
            else break;
        }
        return left >= right;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxProduct(String s) {
        int n = s.length(), total = 1 << n, result = 0;
        List<int[]> record = new ArrayList<>();
        for (int mask = 1; mask < total; mask++) {
            StringBuilder cur = new StringBuilder();
            for (int i = 0; i < n; i++) if ((mask & (1 << i)) != 0) cur.append(s.charAt(i));
            if (check(cur)) record.add(new int[]{ cur.length(), mask });
        }
        for (int[] x : record) for (int[] y : record) if (x != y && (x[1] & y[1]) == 0) result = Math.max(result, x[0] * y[0]);
        return result;
    }

    private boolean check(StringBuilder sb) {
        int left = 0, right = sb.length() - 1;
        while (left < right) {
            if (sb.charAt(left) == sb.charAt(right)) {
                ++left;
                --right;
            } else break;
        }
        return left >= right;
    }
}
```

__Python__:

```Python
class Solution:
    def maxProduct(self, s: str) -> int:
        total, record = (1 << (n := len(s))), []
        for mask in range(1, total):
            cur = []
            for i in range(n):
                if mask & (1 << i):
                    cur.append(s[i])
            if cur == cur[::-1]:
                record.append((len(cur), mask))
        return max([x[0] * y[0] for x in record for y in record if x != y and not (x[1] & y[1])], default=1)
```
