# 1185 Day of the Week 一周中的第几天

__Description__:
Given a date, return the corresponding day of the week for that date.

The input is given as three integers representing the day, month and year respectively.

Return the answer as one of the following values {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"}.

__Example:__

Example 1:

Input: day = 31, month = 8, year = 2019
Output: "Saturday"

Example 2:

Input: day = 18, month = 7, year = 1999
Output: "Sunday"

Example 3:

Input: day = 15, month = 8, year = 1993
Output: "Sunday"

__Constraints:__

The given dates are valid dates between the years 1971 and 2100.

__题目描述__:
给你一个日期，请你设计一个算法来判断它是对应一周中的哪一天。

输入为三个整数：day、month 和 year，分别表示日、月、年。

您返回的结果必须是这几个值中的一个 {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"}。

__示例 :__

示例 1：

输入：day = 31, month = 8, year = 2019
输出："Saturday"

示例 2：

输入：day = 18, month = 7, year = 1999
输出："Sunday"

示例 3：

输入：day = 15, month = 8, year = 1993
输出："Sunday"

__提示：__

给出的日期一定是在 1971 到 2100 年之间的有效日期。

__思路__:

1. 以某时间作为参考, 计算出所给时间与标准的天数的差值, 然后对 7取模即可
2. 蔡勒公式
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string dayOfTheWeek(int day, int month, int year) 
    {
        int y = 1971, d = 0, m = 0, result = 0, count[12] ={31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        for (; y < year; y++)
        {
            if ((!(y % 4) && y % 100) || !(y % 400)) result +=366;
            else result +=365;
        }
        if ((!(y % 4) && y % 100) || !(y % 400)) count[1] = 29;
        for (; d < month - 1; d++) result += count[d];
        result += day - 1;
        vector<string> week = {"Friday", "Saturday", "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday"};
        return week[result % 7];
    }
};
```

__Java__:

```Java
class Solution {
    public String dayOfTheWeek(int day, int month, int year) {
        int benchmark[] = new int[]{0, 3, 2, 5, 0, 3, 5, 1, 4, 6, 2, 4};
        String week[] = new String[]{"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"};
        if (month < 3) --year;
        return week[(year + year / 4 - year / 100 + year / 400 + benchmark[month - 1] + day) % 7];
    }
}
```

__Python__:

```Python
class Solution:
    def dayOfTheWeek(self, day: int, month: int, year: int) -> str:
        return ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"][(year + year // 4 - year // 100 + year // 400 + [0, 3, 2, 5, 0, 3, 5, 1, 4, 6, 2, 4][month - 1] + day) % 7] if month > 2 else ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"][((year - 1) + (year - 1) // 4 - (year - 1) // 100 + (year - 1) // 400 + [0, 3, 2, 5, 0, 3, 5, 1, 4, 6, 2, 4][month - 1] + day) % 7];
```
