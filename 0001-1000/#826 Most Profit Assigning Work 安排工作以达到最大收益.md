# 826 Most Profit Assigning Work 安排工作以达到最大收益

__Description__:
You have n jobs and m workers. You are given three arrays: difficulty, profit, and worker where:

difficulty[i] and profit[i] are the difficulty and the profit of the ith job, and
worker[j] is the ability of jth worker (i.e., the jth worker can only complete a job with difficulty at most worker[j]).
Every worker can be assigned at most one job, but one job can be completed multiple times.

For example, if three workers attempt the same job that pays $1, then the total profit will be $3. If a worker cannot complete any job, their profit is $0.
Return the maximum profit we can achieve after assigning the workers to the jobs.

__Example:__

Example 1:

Input: difficulty = [2,4,6,8,10], profit = [10,20,30,40,50], worker = [4,5,6,7]
Output: 100
Explanation: Workers are assigned jobs of difficulty [4,4,6,6] and they get a profit of [20,20,30,30] separately.

Example 2:

Input: difficulty = [85,47,57], profit = [24,66,99], worker = [40,25,25]
Output: 0

__Constraints:__

n == difficulty.length
n == profit.length
m == worker.length
1 <= n, m <= 10^4
1 <= difficulty[i], profit[i], worker[i] <= 10^5

__题目描述__:
有一些工作：difficulty[i] 表示第 i 个工作的难度，profit[i] 表示第 i 个工作的收益。

现在我们有一些工人。worker[i] 是第 i 个工人的能力，即该工人只能完成难度小于等于 worker[i] 的工作。

每一个工人都最多只能安排一个工作，但是一个工作可以完成多次。

举个例子，如果 3 个工人都尝试完成一份报酬为 1 的同样工作，那么总收益为 $3。如果一个工人不能完成任何工作，他的收益为 $0 。

我们能得到的最大收益是多少？

__示例 :__

输入: difficulty = [2,4,6,8,10], profit = [10,20,30,40,50], worker = [4,5,6,7]
输出: 100
解释: 工人被分配的工作难度是 [4,4,6,6] ，分别获得 [20,20,30,30] 的收益。

__提示:__

1 <= difficulty.length = profit.length <= 10000
1 <= worker.length <= 10000
difficulty[i], profit[i], worker[i]  的范围是 [1, 10^5]

__思路__:

排序
按照难度大小排序, 按照能力大小给工人排序
贪心的给工人当前能力值下最大的收益的工作
如果工人人数远大于任务数, 考虑到工人其实可以是任意顺序, 可以用二分法查找最适合的工作, 减少工人排序的时间复杂度到 O(nlgn)
时间复杂度为 O(nlgn + mlgm), 空间复杂度为 O(m), 其中 n 为人数, m 为任务数

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxProfitAssignment(vector<int>& difficulty, vector<int>& profit, vector<int>& worker) 
    {
        int n = difficulty.size(), result = 0, j = 0, best = 0;
        vector<pair<int, int>> jobs;
        for (int i = 0; i < n; i++) jobs.push_back({ difficulty[i], profit[i] });
        sort(jobs.begin(), jobs.end());
        sort(worker.begin(), worker.end());
        for (const auto& skill: worker) 
        {
            while (j < n and skill >= jobs[j].first) best = max(best, jobs[j++].second);
            result += best;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxProfitAssignment(int[] difficulty, int[] profit, int[] worker) {
        int n = difficulty.length, jobs[][] = new int[n][2], result = 0, j = 0, best = 0;
        for (int i = 0; i < n; i++) {
            jobs[i][0] = difficulty[i];
            jobs[i][1] = profit[i];
        }
        Arrays.sort(jobs, (a, b) -> (a[0] - b[0]));
        Arrays.sort(worker);
        for (int skill: worker) {
            while (j < n && skill >= jobs[j][0]) best = Math.max(best, jobs[j++][1]);
            result += best;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxProfitAssignment(self, difficulty: List[int], profit: List[int], worker: List[int]) -> int:
        jobs, record, diff, result = sorted(zip(difficulty, map(lambda x: -x, profit))), [0], [0], 0
        for i, (j, k) in enumerate(jobs):
            if i and j == jobs[i - 1]:
                continue
            diff.append(j)
            record.append(max(record[-1], -k))
        for i in worker:
            l, r = 0, len(diff) - 1
            while l <= r:
                mid = ((l + r) >> 1)
                if diff[mid] <= i:
                    l = mid + 1
                else: 
                    r = mid - 1
            result += record[r]
        return result
```
