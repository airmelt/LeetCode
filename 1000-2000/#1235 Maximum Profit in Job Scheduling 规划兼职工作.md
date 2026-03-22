# 1235 Maximum Profit in Job Scheduling 规划兼职工作

__Description:__

We have n jobs, where every job is scheduled to be done from startTime[i] to endTime[i], obtaining a profit of profit[i].

You're given the startTime, endTime and profit arrays, return the maximum profit you can take such that there are no two jobs in the subset with overlapping time range.

If you choose a job that ends at time X you will be able to start another job that starts at time X.

__Example:__

Example 1:

![Job 1](https://assets.leetcode.com/uploads/2019/10/10/sample1_1584.png)

Input: startTime = [1,2,3,3], endTime = [3,4,5,6], profit = [50,10,40,70]
Output: 120
Explanation: The subset chosen is the first and fourth job.
Time range [1-3]+[3-6] , we get profit of 120 = 50 + 70.

Example 2:

![Job 2](https://assets.leetcode.com/uploads/2019/10/10/sample22_1584.png)

Input: startTime = [1,2,3,4,6], endTime = [3,5,10,6,9], profit = [20,20,100,70,60]
Output: 150
Explanation: The subset chosen is the first, fourth and fifth job.
Profit obtained 150 = 20 + 70 + 60.

Example 3:

![Job 3](https://assets.leetcode.com/uploads/2019/10/10/sample3_1584.png)

Input: startTime = [1,1,1], endTime = [2,3,4], profit = [5,6,4]
Output: 6

__Constraints:__

1 <= startTime.length == endTime.length == profit.length <= 5 * 10^4
1 <= startTime[i] < endTime[i] <= 10^9
1 <= profit[i] <= 10^4

__题目描述：__

你打算利用空闲时间来做兼职工作赚些零花钱。

这里有 n 份兼职工作，每份工作预计从 startTime[i] 开始到 endTime[i] 结束，报酬为 profit[i]。

给你一份兼职工作表，包含开始时间 startTime，结束时间 endTime 和预计报酬 profit 三个数组，请你计算并返回可以获得的最大报酬。

注意，时间上出现重叠的 2 份工作不能同时进行。

如果你选择的工作在时间 X 结束，那么你可以立刻进行在时间 X 开始的下一份工作。

__示例：__

示例 1：

![工作 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/19/sample1_1584.png)

输入：startTime = [1,2,3,3], endTime = [3,4,5,6], profit = [50,10,40,70]
输出：120
解释：
我们选出第 1 份和第 4 份工作，
时间范围是 [1-3]+[3-6]，共获得报酬 120 = 50 + 70。

示例 2：

![工作 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/19/sample22_1584.png)

输入：startTime = [1,2,3,4,6], endTime = [3,5,10,6,9], profit = [20,20,100,70,60]
输出：150
解释：
我们选择第 1，4，5 份工作。
共获得报酬 150 = 20 + 70 + 60。

示例 3：

![工作 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/19/sample3_1584.png)

输入：startTime = [1,1,1], endTime = [2,3,4], profit = [5,6,4]
输出：6

__提示：__

1 <= startTime.length == endTime.length == profit.length <= 5 * 10^4
1 <= startTime[i] < endTime[i] <= 10^9
1 <= profit[i] <= 10^4

__思路：__

动态规划 ➕ 二分查找 ➕ 贪心
先将 endTime 和 works = (startTime, endTime, profit) 按照 endTime 进行排序(贪心)
设 dp[i] 表示选前 i 个工作能够获得的最大利润
初始化 dp[0] = works[0][2]
dp[i] = max(dp[i - 1], dp[k] + profit[i]), 其中 k 需要满足 startTime[i] >= endTime[k]
使用二分查找找到 startTime[i][1] 在 endTime 中的插入位置 k 更新 dp 即可
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) 
    {
        int n = profit.size();
        vector<vector<int>> works(n, vector<int>(3));
        vector<int> dp(n);
        for (int i = 0; i < n; i++) 
        {
            works[i][0] = startTime[i];
            works[i][1] = endTime[i];
            works[i][2] = profit[i];
        }
        sort(works.begin(), works.end(), [](const auto& w1, const auto & w2){return w1[1] < w2[1];});
        dp.front() = works.front().back();
        for (int i = 1; i < n; i++) 
        {
            int left = 0, right = i, start = works[i].front();
            while (left < right) 
            {
                int mid = left + ((right - left) >> 1);
                if (works[mid][1] > start) right = mid;
                else left = mid + 1;
            }
            dp[i] = left ? max(dp[i - 1], dp[left - 1] + works[i].back()) : max(dp[i - 1], works[i].back());
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int jobScheduling(int[] startTime, int[] endTime, int[] profit) {
        int n = profit.length, works[][] = new int[n][3], dp[] = new int[n];
        for (int i = 0; i < n; i++) {
            works[i][0] = startTime[i];
            works[i][1] = endTime[i];
            works[i][2] = profit[i];
        }
        Arrays.sort(works, (w1, w2) -> w1[1] - w2[1]);
        dp[0] = works[0][2];
        for (int i = 1; i < n; i++) {
            int left = 0, right = i, start = works[i][0];
            while (left < right) {
                int mid = left + ((right - left) >> 1);
                if (works[mid][1] > start) right = mid;
                else left = mid + 1;
            }
            dp[i] = Math.max(dp[i - 1], (left == 0 ? 0 : dp[left - 1]) + works[i][2]);
        }
        return dp[n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def jobScheduling(self, startTime: List[int], endTime: List[int], profit: List[int]) -> int:
        works, endTime, n = sorted(list(zip(endTime, startTime, profit))), sorted(endTime), len(profit)
        dp = [works[0][2]] + [0] * (n - 1)
        for i in range(1, n):
            dp[i] = max(dp[i - 1], dp[bisect_right(endTime, works[i][1]) - 1] + works[i][2])
        return dp[-1]
```
