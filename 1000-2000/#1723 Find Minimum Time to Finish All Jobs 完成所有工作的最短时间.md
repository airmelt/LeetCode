# 1723 Find Minimum Time to Finish All Jobs 完成所有工作的最短时间

__Description:__

You are given an integer array `jobs`, where `jobs[i]` is the amount of time it takes to complete the `i ^ th` job.

There are `k` workers that you can assign jobs to. Each job should be assigned to __exactly__ one worker. The __working time__ of a worker is the sum of the time it takes to complete all jobs assigned to them. Your goal is to devise an optimal assignment such that the __maximum working time__ of any worker is __minimized__.

Return the minimum possible maximum working time of any assignment.

__Example:__

Example 1:

```text
Input: jobs = [3,2,3], k = 3
Output: 3
Explanation: By assigning each person one job, the maximum time is 3.
```

Example 2:

```text
Input: jobs = [1,2,4,7,8], k = 2
Output: 11
Explanation: Assign the jobs the following way:
Worker 1: 1, 2, 8 (working time = 1 + 2 + 8 = 11)
Worker 2: 4, 7 (working time = 4 + 7 = 11)
The maximum working time is 11.
```

__Constraints:__

- `1 <= k <= jobs.length <= 12`
- `1 <= jobs[i] <= 10 ^ 7`

__题目描述:__

给你一个整数数组 `jobs` ，其中 `jobs[i]` 是完成第 `i` 项工作要花费的时间。

请你将这些工作分配给 `k` 位工人。所有工作都应该分配给工人，且每项工作只能分配给一位工人。工人的 __工作时间__ 是完成分配给他们的所有工作花费时间的总和。请你设计一套最佳的工作分配方案，使工人的 __最大工作时间__ 得以 __最小化__ 。

返回分配方案中尽可能 最小 的 最大工作时间 。

__示例:__

示例 1：

```text
输入：jobs = [3,2,3], k = 3
输出：3
解释：给每位工人分配一项工作，最大工作时间是 3 。
```

示例 2：

```text
输入：jobs = [1,2,4,7,8], k = 2
输出：11
解释：按下述方式分配工作：
1 号工人：1、2、8（工作时间 = 1 + 2 + 8 = 11）
2 号工人：4、7（工作时间 = 4 + 7 = 11）
最大工作时间是 11 。
```

__提示：__

- `1 <= k <= jobs.length <= 12`
- `1 <= jobs[i] <= 10 ^ 7`

__思路:__

```text
1. 回溯 ➕ 二分
二分的上界为 sum(jobs), 下界为 max(jobs), 至少有一个工人需要做工作量最大的工作, 最多有一个工人需要做所有的工作
优先分配工作量较大的工作
优先分配给 i 号工人, 再分配给 i + 1 号工人
如果分配的总时间不小于当前的最小值, 则剪枝
时间复杂度为 O(NlogN + log(S - M) * k ^ N), 空间复杂度为 O(N), 其中 N 为 jobs 的长度, S 为 jobs 的和, M 为 jobs 中的最大值
2. 状态压缩动态规划
dp[i][j] 表示前 i 个工人完成状态为 j 的工作的最小工作时间
预计算 sum[i] 表示状态 i 的工作量
dp[i][j] = min(dp[i][j], max(dp[i - 1][j - k], sum[k])), 其中 k 为 j 的子集
初始化 dp[0][j] = sum[j]
时间复杂度为 O(N * 3 ^ N), 空间复杂度为 O(N * 2 ^ N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumTimeRequired(vector<int>& jobs, int k) 
    {
        int n = jobs.size();
        vector<int> sum(1 << n);
        for (int i = 1; i < (1 << n); i++) 
        {
            int x = __builtin_ctz(i), y = i - (1 << x);
            sum[i] = sum[y] + jobs[x];
        }
        vector<vector<int>> dp(k, vector<int>(1 << n));
        for (int i = 0; i < (1 << n); i++) dp[0][i] = sum[i];
        for (int i = 1; i < k; i++) 
        {
            for (int j = 0; j < (1 << n); j++) 
            {
                int minn = INT_MAX;
                for (int x = j; x; x = (x - 1) & j) minn = min(minn, max(dp[i - 1][j - x], sum[x]));
                dp[i][j] = minn;
            }
        }
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumTimeRequired(int[] jobs, int k) {
        Arrays.sort(jobs);
        Collections.reverse(Arrays.asList(jobs));
        int left = jobs[0], right = Arrays.stream(jobs).sum();
        while (left < right) {
            int mid = left + ((right - left) >>> 1);
            if (backtrack(jobs, new int[k], 0, mid)) right = mid;
            else left = mid + 1;
        }
        return left;
    }

    public boolean backtrack(int[] jobs, int[] workloads, int i, int limit) {
        if (i >= jobs.length) return true;
        int cur = jobs[i];
        for (int j = 0; j < workloads.length; ++j) {
            if (workloads[j] + cur <= limit) {
                workloads[j] += cur;
                if (backtrack(jobs, workloads, i + 1, limit)) return true;
                workloads[j] -= cur;
            }
            if (workloads[j] == 0 || workloads[j] + cur == limit) break;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumTimeRequired(self, jobs: List[int], k: int) -> int:
        right, left, n = sum(jobs), max(jobs), len(jobs)
        jobs.sort(reverse=True)
        
        def backtrack(pos: int, cur: List[int]) -> bool:
            if pos == n:
                return True
            job = jobs[pos]
            for i in range(k):
                if cur[i] + job <= mid:
                    cur[i] += job
                    if backtrack(pos + 1, cur):
                        return True
                    cur[i] -= job
                if not cur[i] or cur[i] + job == mid:
                    break
            return False
        while left < right:
            mid = ((right + left) >> 1)
            if backtrack(0, [0] * k):
                right = mid
            else:
                left = mid + 1
        return left
```
