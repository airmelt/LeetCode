# 1736 Latest Time by Replacing Hidden Digits 替换隐藏数字得到的最晚时间

__Description:__

You are given a string `time` in the form of `hh:mm`, where some of the digits in the string are hidden (represented by `?`).

The valid times are those inclusively between `00:00` and `23:59`.

Return _the latest valid time you can get from_ `time` _by replacing the hidden_ _digits_.

__Example:__

Example 1:

```text
Input: time = "2?:?0"
Output: "23:50"
Explanation: The latest hour beginning with the digit '2' is 23 and the latest minute ending with the digit '0' is 50.
```

Example 2:

```text
Input: time = "0?:3?"
Output: "09:39"
```

Example 3:

```text
Input: time = "1?:22"
Output: "19:22"
```

__Constraints:__

- `time` is in the format `hh:mm`.
- It is guaranteed that you can produce a valid time from the given string.

__题目描述:__

给你一个字符串 `time` ，格式为 `hh:mm`（小时:分钟），其中某几位数字被隐藏（用 `?` 表示）。

有效的时间为 `00:00` 到 `23:59` 之间的所有时间，包括 `00:00` 和 `23:59` 。

替换 `time` 中隐藏的数字，返回你可以得到的最晚有效时间。

__示例:__

示例 1：

```text
输入：time = "2?:?0"
输出："23:50"
解释：以数字 '2' 开头的最晚一小时是 23 ，以 '0' 结尾的最晚一分钟是 50 。
```

示例 2：

```text
输入：time = "0?:3?"
输出："09:39"
```

示例 3：

```text
输入：time = "1?:22"
输出："19:22"
```

__提示：__

- `time` 的格式为 `hh:mm`
- 题目数据保证你可以由输入的字符串生成有效的时间

__思路:__

```text
模拟
第一位为小时的十位数，如果为 '?'，则判断第二位是否大于 '3'，如果大于 '3' 且不大于 '9'(避免讨论第二位的问号)，则第一位为 '1'，否则为 '2'
第二位为小时的个位数，如果为 '?'，则判断第一位是否小于 '2'，如果小于 '2'，则第二位为 '9'，否则为 '3'
第四位为分钟的十位数，如果为 '?'，则为 '5'
第五位为分钟的个位数，如果为 '?'，则为 '9'
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string maximumTime(string time) 
    {
        time.at(0) = time.at(0) != '?' ? time.at(0) : ('3' < time.at(1) && time.at(1) <= '9' ? '1' : '2');
        time.at(1) = time.at(1) != '?' ? time.at(1) : (time.at(0) < '2' ? '9' : '3');
        time.at(3) = time.at(3) != '?'? time.at(3) : '5';
        time.at(4) = time.at(4) != '?'? time.at(4) : '9';
        return time;
    }
};
```

__Java__:

```Java
class Solution {
    public String maximumTime(String time) {
        StringBuilder sb = new StringBuilder();
        sb.append(time.charAt(0) != '?' ? time.charAt(0) : ('3' < time.charAt(1) && time.charAt(1) <= '9' ? '1' : '2')).append(time.charAt(1) != '?' ? time.charAt(1) : (time.charAt(0) < '2' ? '9' : '3')).append(':').append(time.charAt(3) != '?'? time.charAt(3) : '5').append(time.charAt(4) != '?'? time.charAt(4) : '9');
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def maximumTime(self, time: str) -> str:
        return (time[0] if time[0] != '?' else '1' if '3' < time[1] <= '9' else '2') + (time[1] if time[1] != '?' else '9' if time[0] < '2' else '3') + ':' + (time[3] if time[3] != '?' else '5') + (time[4] if time[4] != '?' else '9')
```
