# 2437 Number of Valid Clock Times 有效时间的数目

__Description:__

You are given a string of length `5` called `time`, representing the current time on a digital clock in the format `"hh:mm"`. The __earliest__ possible time is `"00:00"` and the __latest__ possible time is `"23:59"`.

In the string `time`, the digits represented by the `?` symbol are __unknown__, and must be __replaced__ with a digit from `0` to `9`.

Return _an integer_ `answer`_, the number of valid clock times that can be created by replacing every_ `?` _with a digit from_ `0` _to_ `9`.

__Example:__

Example 1:

```text
Input: time = "?5:00"
Output: 2
Explanation: We can replace the ? with either a 0 or 1, producing "05:00" or "15:00". Note that we cannot replace it with a 2, since the time "25:00" is invalid. In total, we have two choices.
```

Example 2:

```text
Input: time = "0?:0?"
Output: 100
Explanation: Each ? can be replaced by any digit from 0 to 9, so we have 100 total choices.
```

Example 3:

```text
Input: time = "??:??"
Output: 1440
Explanation: There are 24 possible choices for the hours, and 60 possible choices for the minutes. In total, we have 24 * 60 = 1440 choices.
```

__Constraints:__

- `time` is a valid string of length `5` in the format `"hh:mm"`.
- `"00" <= hh <= "23"`
- `"00" <= mm <= "59"`
- Some of the digits might be replaced with `'?'` and need to be replaced with digits from `0` to `9`.

__题目描述:__

给你一个长度为 `5` 的字符串 `time` ，表示一个电子时钟当前的时间，格式为 `"hh:mm"` 。__最早__ 可能的时间是 `"00:00"` ，__最晚__ 可能的时间是 `"23:59"` 。

在字符串 `time` 中，被字符 `?` 替换掉的数位是 __未知的__ ，被替换的数字可能是 `0` 到 `9` 中的任何一个。

请你返回一个整数 `answer` ，将每一个 `?` 都用 `0` 到 `9` 中一个数字替换后，可以得到的有效时间的数目。

__示例:__

示例 1：

```text
输入：time = "?5:00"
输出：2
解释：我们可以将 ? 替换成 0 或 1 ，得到 "05:00" 或者 "15:00" 。注意我们不能替换成 2 ，因为时间 "25:00" 是无效时间。所以我们有两个选择。
```

示例 2：

```text
输入：time = "0?:0?"
输出：100
解释：两个 ? 都可以被 0 到 9 之间的任意数字替换，所以我们总共有 100 种选择。
```

示例 3：

```text
输入：time = "??:??"
输出：1440
解释：小时总共有 24 种选择，分钟总共有 60 种选择。所以总共有 24 * 60 = 1440 种选择。
```

__提示：__

- `time` 是一个长度为 `5` 的有效字符串，格式为 `"hh:mm"` 。
- `"00" <= hh <= "23"`
- `"00" <= mm <= "59"`
- 字符串中有的数位是 `'?'` ，需要用 `0` 到 `9` 之间的数字替换。

__思路:__

```text
模拟
小时从 0 遍历到 24
分钟从 0 遍历到 60
检查是否满足条件
根据乘法原理, 小时的有效结果和分钟的有效结果相乘即为最终结果
时间复杂度为 O(1), 空间复杂度为 O(1), 实际上为 O(24) + O(60)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countTime(string time) 
    {
        return helper(time.substr(0, 2), 24) * helper(time.substr(3), 60); 
    }
private:
    int helper(string t, int a) 
    {
        int result = 0;
        for (int i = 0; i < a; i++) if ((t[0] == '?' or i / 10 == t[0] - '0') and (t[1] == '?' or i % 10 == t[1] - '0')) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countTime(String time) {
        return helper(time.substring(0, 2).toCharArray(), 24) * helper(time.substring(3).toCharArray(), 60); 
    }

    private int helper(char[] t, int a) {
        int result = 0;
        for (int i = 0; i < a; i++) if ((t[0] == '?' || i / 10 == t[0] - '0') && (t[1] == '?' || i % 10 == t[1] - '0')) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countTime(self, time: str) -> int:
        return sum(all(b in a + '?' for a, b in zip('%02d:%02d'%(h, m), time)) for h, m in product(range(24), range(60)))
```
