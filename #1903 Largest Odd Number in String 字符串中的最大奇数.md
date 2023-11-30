# 1903 Largest Odd Number in String 字符串中的最大奇数

__Description:__

You are given a string `num`, representing a large integer. Return _the __largest-valued odd__ integer (as a string) that is a __non-empty substring__ of_ `num`_, or an empty string_ `""` _if no odd integer exists_.

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: num = "52"
Output: "5"
Explanation: The only non-empty substrings are "5", "2", and "52". "5" is the only odd number.
```

Example 2:

```text
Input: num = "4206"
Output: ""
Explanation: There are no odd numbers in "4206".
```

Example 3:

```text
Input: num = "35427"
Output: "35427"
Explanation: "35427" is already an odd number.
```

__Constraints:__

- `1 <= num.length <= 10 ^ 5`
- `num` only consists of digits and does not contain any leading zeros.

__题目描述:__

给你一个字符串 `num` ，表示一个大整数。请你在字符串 `num` 的所有 __非空子字符串__ 中找出 __值最大的奇数__ ，并以字符串形式返回。如果不存在奇数，则返回一个空字符串 `""` 。

子字符串 是字符串中的一个连续的字符序列。

__示例:__

示例 1：

```text
输入：num = "52"
输出："5"
解释：非空子字符串仅有 "5"、"2" 和 "52" 。"5" 是其中唯一的奇数。
```

示例 2：

```text
输入：num = "4206"
输出：""
解释：在 "4206" 中不存在奇数。
```

示例 3：

```text
输入：num = "35427"
输出："35427"
解释："35427" 本身就是一个奇数。
```

__提示：__

- `1 <= num.length <= 10 ^ 5`
- `num` 仅由数字组成且不含前导零

__思路:__

```text
贪心
从后往前遍历
遇到第一个奇数就截断
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string largestOddNumber(string num)
    {
        for (int n = num.size(), i = n - 1; i > -1; i--) if (num[i] & 1) return num.substr(0, i + 1);
        return "";
    }
};
```

__Java__:

```Java
class Solution {
    public String largestOddNumber(String num) {
        for (int n = num.length(), i = n - 1; i > -1; i--) if ((num.charAt(i) & 1) != 0) return num.substring(0, i + 1);
        return "";
    }
}
```

__Python__:

```Python
class Solution 
{
public:
    string largestOddNumber(string num)
    {
        for (int n = num.size(), i = n - 1; i > -1; i--) if (num[i] & 1) return num.substr(0, i + 1);
        return "";
    }
};//*[@id="qd-content"]/div[1]/div/div/div/div[1]/div/div/a[1]/div
```
