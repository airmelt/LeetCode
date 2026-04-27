# 1154 Day of the Year 一年中的第几天

__Description__:
Given a string date representing a Gregorian calendar date formatted as YYYY-MM-DD, return the day number of the year.

__Example:__

Example 1:

Input: date = "2019-01-09"
Output: 9
Explanation: Given date is the 9th day of the year in 2019.

Example 2:

Input: date = "2019-02-10"
Output: 41

Example 3:

Input: date = "2003-03-01"
Output: 60

Example 4:

Input: date = "2004-03-01"
Output: 61

__Constraints:__

date.length == 10
date[4] == date[7] == '-', and all other date[i]'s are digits
date represents a calendar date between Jan 1st, 1900 and Dec 31, 2019.

__题目描述__:
给你一个按 YYYY-MM-DD 格式表示日期的字符串 date，请你计算并返回该日期是当年的第几天。

通常情况下，我们认为 1 月 1 日是每年的第 1 天，1 月 2 日是每年的第 2 天，依此类推。每个月的天数与现行公元纪年法（格里高利历）一致。

__示例 :__

示例 1：

输入：date = "2019-01-09"
输出：9

示例 2：

输入：date = "2019-02-10"
输出：41

示例 3：

输入：date = "2003-03-01"
输出：60

示例 4：

输入：date = "2004-03-01"
输出：61

__提示：__

date.length == 10
date[4] == date[7] == '-'，其他的 date[i] 都是数字。
date 表示的范围从 1900 年 1 月 1 日至 2019 年 12 月 31 日。

__思路__:

将每个月的天数作为一个数组, 按照格式将日期转化为年月日, 按照月份加上天数, 最后判断是否是闰年, 注意题中给了年份的范围为 1900-2019, 所以只要去掉 1900特殊值即可
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int dayOfYear(string date) 
    {
        int days[] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30}, year = atoi(date.substr(0, 4).c_str()), month = atoi(date.substr(5, 2).c_str()), day = atoi(date.substr(8, 2).c_str());
        for (int i = 0; i < month - 1; i++) day += days[i];
        return year != 1900 and year % 4 == 0 and month > 2 ? ++day : day;
    }
};
```

__Java__:

```Java
class Solution {
    public int dayOfYear(String date) {
        int days[] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30}, year = Integer.parseInt(date.substring(0, 4)), month = Integer.parseInt(date.substring(5, 7)), day = Integer.parseInt(date.substring(8, 10));
        for (int i = 0; i < month - 1; i++) day += days[i];
        return year != 1900 && year % 4 == 0 && month > 2 ? ++day : day;
    }
}
```

__Python__:

```Python
import time
class Solution:
    def dayOfYear(self, date: str) -> int:
        return time.strptime(date, "%Y-%m-%d")[-2]
```
