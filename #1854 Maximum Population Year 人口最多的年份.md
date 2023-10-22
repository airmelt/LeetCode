# 1854 Maximum Population Year 人口最多的年份

__Description:__

You are given a 2D integer array `logs` where each `logs[i] = [birthi, deathi]` indicates the birth and death years of the `i ^ th` person.

The __population__ of some year `x` is the number of people alive during that year. The `i ^ th` person is counted in year `x`'s population if `x` is in the __inclusive__ range `[birthi, deathi - 1]`. Note that the person is __not__ counted in the year that they die.

Return the earliest year with the maximum population.

__Example:__

Example 1:

```text
Input: logs = [[1993,1999],[2000,2010]]
Output: 1993
Explanation: The maximum population is 1, and 1993 is the earliest year with this population.
```

Example 2:

```text
Input: logs = [[1950,1961],[1960,1971],[1970,1981]]
Output: 1960
Explanation: 
The maximum population is 2, and it had happened in years 1960 and 1970.
The earlier year between them is 1960.
```

__Constraints:__

- `1 <= logs.length <= 100`
- `1950 <= birthi < deathi <= 2050`

__题目描述:__

给你一个二维整数数组 `logs` ，其中每个 `logs[i] = [birthi, deathi]` 表示第 `i` 个人的出生和死亡年份。

年份 `x` 的 __人口__ 定义为这一年期间活着的人的数目。第 `i` 个人被计入年份 `x` 的人口需要满足: `x` 在闭区间 `[birthi, deathi - 1]` 内。注意，人不应当计入他们死亡当年的人口中。

返回 人口最多 且 最早 的年份。

__示例:__

示例 1：

```text
输入：logs = [[1993,1999],[2000,2010]]
输出：1993
解释：人口最多为 1 ，而 1993 是人口为 1 的最早年份。
```

示例 2：

```text
输入：logs = [[1950,1961],[1960,1971],[1970,1981]]
输出：1960
解释： 
人口最多为 2 ，分别出现在 1960 和 1970 。
其中最早年份是 1960 。
```

__提示：__

- `1 <= logs.length <= 100`
- `1950 <= birthi < deathi <= 2050`

__思路:__

```text
1. 暴力法
用一个数组记录每年的存活人数
对每一个记录的出生时间和死亡时间中间的每一个时间进行累加
最后找到数组中最大值对应的下标
时间复杂度为 O(MN), 空间复杂度为 O(M), 其中 N 为 logs 数组的大小, M 为出生时间和死亡时间的差值
2. 差分数组
在出生时间点加 1
在死亡时间点减 1
求出这个数组的前缀和就是当前时间的存活人数
累计的最大存活人数对应的时间就是人口最多的年份
时间复杂度为 O(max(M, N)), 空间复杂度为 O(M), 其中 N 为 logs 数组的大小, M 为出生时间和死亡时间的差值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumPopulation(vector<vector<int>>& logs) 
    {
        vector<int> record(102), result(101);
        for (const auto& log: logs)
        {
            record[log.front() - 1950] += 1;
            record[log.back() - 1950] -= 1;
        }
        for (int i = 0; i < 101; i++) result[i] = i ? result[i - 1] + record[i] : record.front();
        return max_element(result.begin(), result.end()) - result.begin() + 1950;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumPopulation(int[][] logs) {
        int record[] = new int[102], s = 0, result = 0, count = 0;
        for (int[] log : logs) {
            record[log[0] - 1950] += 1;
            record[log[1] - 1950] -= 1;
        }
        for (int i = 0; i < 101; i++) {
            if ((s += record[i]) > count) {
                count = s; 
                result = i;
            }
        }
        return result + 1950;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumPopulation(self, logs: List[List[int]]) -> int:
        count = [0] * 2050
        for log in logs:
            for i in range(log[0], log[1]):
                count[i] += 1
        return count.index(max(count))
```
