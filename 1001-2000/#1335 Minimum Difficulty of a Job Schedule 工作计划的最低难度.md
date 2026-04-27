# 1335 Minimum Difficulty of a Job Schedule 工作计划的最低难度

__Description:__

You want to schedule a list of jobs in d days. Jobs are dependent (i.e To work on the ith job, you have to finish all the jobs j where 0 <= j < i).

You have to finish at least one task every day. The difficulty of a job schedule is the sum of difficulties of each day of the d days. The difficulty of a day is the maximum difficulty of a job done on that day.

You are given an integer array jobDifficulty and an integer d. The difficulty of the ith job is jobDifficulty[i].

Return the minimum difficulty of a job schedule. If you cannot find a schedule for the jobs return -1.

__Example:__

Example 1:

![Schedule](https://assets.leetcode.com/uploads/2020/01/16/untitled.png)

Input: jobDifficulty = [6,5,4,3,2,1], d = 2
Output: 7
Explanation: First day you can finish the first 5 jobs, total difficulty = 6.
Second day you can finish the last job, total difficulty = 1.
The difficulty of the schedule = 6 + 1 = 7

Example 2:

Input: jobDifficulty = [9,9,9], d = 4
Output: -1
Explanation: If you finish a job per day you will still have a free day. you cannot find a schedule for the given jobs.

Example 3:

Input: jobDifficulty = [1,1,1], d = 3
Output: 3
Explanation: The schedule is one job per day. total difficulty will be 3.

__Constraints:__

1 <= jobDifficulty.length <= 300
0 <= jobDifficulty[i] <= 1000
1 <= d <= 10

__题目描述：__

你需要制定一份 d 天的工作计划表。工作之间存在依赖，要想执行第 i 项工作，你必须完成全部 j 项工作（ 0 <= j < i）。

你每天 至少 需要完成一项任务。工作计划的总难度是这 d 天每一天的难度之和，而一天的工作难度是当天应该完成工作的最大难度。

给你一个整数数组 jobDifficulty 和一个整数 d，分别代表工作难度和需要计划的天数。第 i 项工作的难度是 jobDifficulty[i]。

返回整个工作计划的 最小难度 。如果无法制定工作计划，则返回 -1 。

__示例：__

示例 1：

![工作计划](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/26/untitled.png)

输入：jobDifficulty = [6,5,4,3,2,1], d = 2
输出：7
解释：第一天，您可以完成前 5 项工作，总难度 = 6.
第二天，您可以完成最后一项工作，总难度 = 1.
计划表的难度 = 6 + 1 = 7

示例 2：

输入：jobDifficulty = [9,9,9], d = 4
输出：-1
解释：就算你每天完成一项工作，仍然有一天是空闲的，你无法制定一份能够满足既定工作时间的计划表。

示例 3：

输入：jobDifficulty = [1,1,1], d = 3
输出：3
解释：工作计划为每天一项工作，总难度为 3 。

示例 4：

输入：jobDifficulty = [7,1,7,1,7,1], d = 3
输出：15

示例 5：

输入：jobDifficulty = [11,111,22,222,33,333,44,444], d = 6
输出：843

__提示：__

1 <= jobDifficulty.length <= 300
0 <= jobDifficulty[i] <= 1000
1 <= d <= 10

__思路：__

动态规划
预处理区间[i, j]最大值
设 dp\[i][j] 表示 jobDifficulty[:j] 中经过 i 天的难度之和的最小值
最后需要返回 dp\[n][d]
首先如果 d 天不够分配, 即 n < d, 直接返回 -1
初始化 dp\[0][0] = 0
转移方程为 dp\[i][j] = min(dp\[i - 1][k] + range_max\[k][j - 1]), 其中 i - 1 <= k <= j - 1
时间复杂度为 O(n ^ 2d), 空间复杂度为 O(nd)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int minDifficulty(vector<int>& jobDifficulty, int d) 
    {
        int n = jobDifficulty.size();
        vector<vector<int>> dp(d + 1, vector<int>(n + 1, 0x3f3f3f3f)), range_max(n, vector<int>(n, -1));
        dp.front().front() = 0;
        for (int i = 0; i < n; i++) for (int j = i; j < n; j++) for (int k = i; k <= j; k++) range_max[i][j] = max(range_max[i][j], jobDifficulty[k]);
        for (int i = 0; i < n; i++) dp.front()[i + 1] = max(dp.front()[i], jobDifficulty[i]);
        for (int i = 1; i <= d; i++) for (int j = i; j <= n; j++) for (int k = i - 1; k <= j - 1; k++) dp[i][j] = min(dp[i - 1][k] + range_max[k][j - 1], dp[i][j]);
        return n < d ? -1 : dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minDifficulty(int[] jobDifficulty, int d) {
        int n = jobDifficulty.length, rangeMax[][] = new int[n][n], dp[][] = new int[d + 1][n + 1];
        if (n < d) return -1;
        for (int i = 0; i < n; i++) for (int j = i; j < n; j++) for (int k = i; k <= j; k++) rangeMax[i][j] = Math.max(rangeMax[i][j], jobDifficulty[k]);
        for (int i = 0; i < n; i++) dp[0][i + 1] = Math.max(dp[0][i], jobDifficulty[i]);
        for (int i = 1; i <= d; i++) {
            for (int j = i; j <= n; j++) {
                dp[i][j]=Integer.MAX_VALUE;
                for (int k = i - 1; k <= j - 1; k++) dp[i][j] = Math.min(dp[i - 1][k] + rangeMax[k][j - 1], dp[i][j]);
            }
        }
        return dp[d][n];
    }
}
```

__Python__:

```Python
class Solution:
    def minDifficulty(self, jobDifficulty: List[int], d: int) -> int:
        n = len(jobDifficulty)
        dp = [[float('inf')] * (n + 1) for _ in range(d + 1)]
        dp[0][0] = 0
        for i in range(1, d + 1):
            for j in range(1, n + 1):
                for k in range(j):
                    dp[i][j] = min(dp[i][j], dp[i - 1][j - k - 1] + max(jobDifficulty[j - k - 1:j]))
        return dp[-1][-1] if dp[-1][-1] < float('inf') else -1
```
