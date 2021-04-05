# 539 Minimum Time Difference 最小时间差

__Description__:
Given a list of 24-hour clock time points in "HH:MM" format, return the minimum minutes difference between any two time-points in the list.

__Example:__

Example 1:

Input: timePoints = ["23:59","00:00"]
Output: 1

Example 2:

Input: timePoints = ["00:00","23:59","00:00"]
Output: 0

__Constraints:__

2 <= timePoints <= 2 * 104
timePoints[i] is in the format "HH:MM".

__题目描述__:
给定一个 24 小时制（小时:分钟 "HH:MM"）的时间列表，找出列表中任意两个时间的最小时间差并以分钟数表示。

__示例 :__

示例 1：

输入：timePoints = ["23:59","00:00"]
输出：1

示例 2：

输入：timePoints = ["00:00","23:59","00:00"]
输出：0

__提示:__

2 <= timePoints <= 2 * 104
timePoints[i] 格式为 "HH:MM"

__思路__:

转化为分钟数
注意到两头的值可能最小, 可以初始化赋值
排序之后比较差值即可
时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findMinDifference(vector<string>& timePoints) 
    {
        int n = timePoints.size();
        if (n >= 1440) return 0;
        vector<int> count(n);
        for (int i = 0; i < n; i++) count[i] = [](const auto &s) { return s[0] * 600 + s[1] * 60 + s[3] * 10 + s[4]; }(timePoints[i]);
        sort(count.begin(), count.end());
        int m = 1440 + count.front() - count.back();
        for (int i = 0; i < n - 1; i++)
        {
            m = min(m, count[i + 1] - count[i]);
            if (!m) return m;
        }
        return m;
    }
};
```

__Java__:

```Java
class Solution {
    public int findMinDifference(List<String> timePoints) {
        int n = timePoints.size();
        if (n >= 1440) return 0;
        int[] count = new int[n];
        for (int i = 0; i < n; i++) count[i] = timePoints.get(i).charAt(0) * 600 + timePoints.get(i).charAt(1) * 60 + timePoints.get(i).charAt(3) * 10 + timePoints.get(i).charAt(4);
        Arrays.sort(count);
        int m = 1440 + count[0] - count[n - 1];
        for (int i = 1; i < n; i++) {
            m = Math.min(m, count[i] - count[i - 1]);
            if (m == 0) return m;
        }
        return m;
    }
}
```

__Python__:

```Python
class Solution:
    def findMinDifference(self, timePoints: List[str]) -> int:
        start, end, count = 1441, -1, [0] * 1440
        for t in timePoints:
            t = t.split(":")
            count[(t := int(t[0]) * 60 + int(t[1]))] += 1
            if count[t] == 2:
                return 0
            start, end = min(start, t), max(end, t)
        result, pre = 1440 + start - end, start
        for i in range(start + 1, end + 1):
            if count[i]:
                result, pre = min(result, i - pre), i
        return result
```
