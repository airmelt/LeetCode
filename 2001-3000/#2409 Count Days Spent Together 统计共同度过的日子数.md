# 2409 Count Days Spent Together 统计共同度过的日子数

__Description:__

Alice and Bob are traveling to Rome for separate business meetings.

You are given 4 strings `arriveAlice`, `leaveAlice`, `arriveBob`, and `leaveBob`. Alice will be in the city from the dates `arriveAlice` to `leaveAlice` (__inclusive__), while Bob will be in the city from the dates `arriveBob` to `leaveBob` (__inclusive__). Each will be a 5-character string in the format `"MM-DD"`, corresponding to the month and day of the date.

Return the total number of days that Alice and Bob are in Rome together.

You can assume that all dates occur in the __same__ calendar year, which is __not__ a leap year. Note that the number of days per month can be represented as: `[31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]`.

__Example:__

Example 1:

```text
Input: arriveAlice = "08-15", leaveAlice = "08-18", arriveBob = "08-16", leaveBob = "08-19"
Output: 3
Explanation: Alice will be in Rome from August 15 to August 18. Bob will be in Rome from August 16 to August 19. They are both in Rome together on August 16th, 17th, and 18th, so the answer is 3.
```

Example 2:

```text
Input: arriveAlice = "10-01", leaveAlice = "10-31", arriveBob = "11-01", leaveBob = "12-31"
Output: 0
Explanation: There is no day when Alice and Bob are in Rome together, so we return 0.
```

__Constraints:__

- All dates are provided in the format `"MM-DD"`.
- Alice and Bob's arrival dates are __earlier than or equal to__ their leaving dates.
- The given dates are valid dates of a __non-leap__ year.

__题目描述:__

Alice 和 Bob 计划分别去罗马开会。

给你四个字符串 `arriveAlice` ， `leaveAlice` ， `arriveBob` 和 `leaveBob` 。Alice 会在日期 `arriveAlice` 到 `leaveAlice` 之间在城市里（__日期为闭区间__），而 Bob 在日期 `arriveBob` 到 `leaveBob` 之间在城市里（__日期为闭区间__）。每个字符串都包含 5 个字符，格式为 `"MM-DD"` ，对应着一个日期的月和日。

请你返回 Alice和 Bob 同时在罗马的天数。

你可以假设所有日期都在 __同一个__ 自然年，而且 __不是__ 闰年。每个月份的天数分别为: `[31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]` 。

__示例:__

示例 1：

```text
输入：arriveAlice = "08-15", leaveAlice = "08-18", arriveBob = "08-16", leaveBob = "08-19"
输出：3
解释：Alice 从 8 月 15 号到 8 月 18 号在罗马。Bob 从 8 月 16 号到 8 月 19 号在罗马，他们同时在罗马的日期为 8 月 16、17 和 18 号。所以答案为 3 。
```

示例 2：

```text
输入：arriveAlice = "10-01", leaveAlice = "10-31", arriveBob = "11-01", leaveBob = "12-31"
输出：0
解释：Alice 和 Bob 没有同时在罗马的日子，所以我们返回 0 。
```

__提示：__

- 所有日期的格式均为 `"MM-DD"` 。
- Alice 和 Bob 的到达日期都 __早于或等于__ 他们的离开日期。
- 题目测试用例所给出的日期均为 __非闰年__ 的有效日期。

__思路:__

```text
模拟
到达的日子选择最晚的那个, 离开的日子选择最早的那个
先将所有月份日子都转换为天数
然后计算两个人共同度过的日子
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
int DAYS[] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
class Solution 
{
public:
    int countDaysTogether(string arriveAlice, string leaveAlice, string arriveBob, string leaveBob) 
    {
        return max(helper(min(leaveAlice, leaveBob)) - helper(max(arriveAlice, arriveBob)) + 1, 0);
    }
private:
    int helper(const string& s) 
    {
        int day = (s[3] - '0') * 10 + s[4] - '0';
        for (int i = 0, month = (s[0] - '0') * 10 + s[1] - '0'; i < month - 1; i++) day += DAYS[i];
        return day;
    }
};
```

__Java__:

```Java
class Solution {
    private static final int[] DAYS = new int[]{31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

    private int helper(String s) {
        int day = (s.charAt(3) - '0') * 10 + s.charAt(4) - '0';
        for (int i = 0, month = (s.charAt(0) - '0') * 10 + s.charAt(1) - '0'; i < month - 1; i++) day += DAYS[i];
        return day;
    }

    public int countDaysTogether(String arriveAlice, String leaveAlice, String arriveBob, String leaveBob) {
        return Math.max(helper(leaveAlice.compareTo(leaveBob) < 0 ? leaveAlice : leaveBob) - helper(arriveAlice.compareTo(arriveBob) > 0 ? arriveAlice : arriveBob) + 1, 0);
    }
}
```

__Python__:

```Python
class Solution:
    def countDaysTogether(self, arriveAlice: str, leaveAlice: str, arriveBob: str, leaveBob: str) -> int:
        return max(0, ((lambda s: date(2022, *map(int, s.split('-'))))(min(leaveAlice, leaveBob)) - (lambda s: date(2022, *map(int, s.split('-'))))(max(arriveAlice, arriveBob))).days + 1)
```
