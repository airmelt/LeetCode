# 857 Minimum Cost to Hire K Workers 雇佣 K 名工人的最低成本

__Description__:
There are n workers. You are given two integer arrays quality and wage where quality[i] is the quality of the ith worker and wage[i] is the minimum wage expectation for the ith worker.

We want to hire exactly k workers to form a paid group. To hire a group of k workers, we must pay them according to the following rules:

Every worker in the paid group should be paid in the ratio of their quality compared to other workers in the paid group.
Every worker in the paid group must be paid at least their minimum wage expectation.
Given the integer k, return the least amount of money needed to form a paid group satisfying the above conditions. Answers within 10-5 of the actual answer will be accepted.

__Example:__

Example 1:

Input: quality = [10,20,5], wage = [70,50,30], k = 2
Output: 105.00000
Explanation: We pay 70 to 0th worker and 35 to 2nd worker.

Example 2:

Input: quality = [3,1,10,10,1], wage = [4,8,2,2,7], k = 3
Output: 30.66667
Explanation: We pay 4 to 0th worker, 13.33333 to 2nd and 3rd workers separately.

__Constraints:__

n == quality.length == wage.length
1 <= k <= n <= 10^4
1 <= quality[i], wage[i] <= 10^4

__题目描述__:
有 N 名工人。 第 i 名工人的工作质量为 quality[i] ，其最低期望工资为 wage[i] 。

现在我们想雇佣 K 名工人组成一个工资组。在雇佣 一组 K 名工人时，我们必须按照下述规则向他们支付工资：

对工资组中的每名工人，应当按其工作质量与同组其他工人的工作质量的比例来支付工资。
工资组中的每名工人至少应当得到他们的最低期望工资。
返回组成一个满足上述条件的工资组至少需要多少钱。

__示例 :__

示例 1：

输入： quality = [10,20,5], wage = [70,50,30], K = 2
输出： 105.00000
解释： 我们向 0 号工人支付 70，向 2 号工人支付 35。

示例 2：

输入： quality = [3,1,10,10,1], wage = [4,8,2,2,7], K = 3
输出： 30.66667
解释： 我们向 0 号工人支付 4，向 2 号和 3 号分别支付 13.33333。

__提示:__

1 <= K <= N <= 10000，其中 N = quality.length = wage.length
1 <= quality[i] <= 10000
1 <= wage[i] <= 10000
与正确答案误差在 10^-5 之内的答案将被视为正确的。

__思路__:

排序 ➕ 优先队列
计算每一个工人的性价比 value[i] = wage[i] / quality[i]
对工人的性价比进行排序
每次选择一个工人, 将他的期望工资加到大根堆中
每次堆的大小超过 k, 即 pq.size() == k + 1, sum_quality 减去 pq.top()
每次堆的大小为 k 时, 更新结果为 sum_quality * wage[i] 和结果的较小值
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    double mincostToHireWorkers(vector<int>& quality, vector<int>& wage, int k) 
    {
        double result = DBL_MAX, sum_quality  = 0.0;
        int n = quality.size();
        vector<vector<double>> workers(n, vector<double>(2));
        for (int i = 0, n = quality.size(); i < n; i++) workers[i] = vector<double>{ (double)(wage[i]) / quality[i], (double)quality[i] };
        sort(workers.begin(), workers.end());
        priority_queue<double> pq;
        for (const auto& worker: workers) 
        {
            sum_quality  += worker.back();
            pq.emplace(worker[1]);
            if (pq.size() > k) 
            {
                sum_quality  -= pq.top();
                pq.pop();
            }
            if (pq.size() == k) result = min(result, sum_quality  * worker.front());
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public double mincostToHireWorkers(int[] quality, int[] wage, int k) {
        double workers[][] = new double[quality.length][2], result = Double.MAX_VALUE, sumQuality = 0.0;
        for (int i = 0, n = quality.length; i < n; i++) workers[i] = new double[]{ (double)(wage[i]) / quality[i], (double)quality[i] };
        Arrays.sort(workers, (a, b) -> Double.compare(a[0], b[0]));
        PriorityQueue<Double> pq = new PriorityQueue<>();
        for (double[] worker: workers) {
            sumQuality += worker[1];
            pq.add(-worker[1]);
            if (pq.size() > k) sumQuality += pq.poll();
            if (pq.size() == k) result = Math.min(result, sumQuality * worker[0]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def mincostToHireWorkers(self, quality: List[int], wage: List[int], k: int) -> float:
        workers, n, result, heap, s = sorted([(wage[i] / quality[i], quality[i]) for i in range(len(wage))]), len(wage), float('inf'), [], 0.0
        for worker in workers:
            s += worker[1]
            heappush(heap, -worker[1])
            if len(heap) > k:
                s += heappop(heap)
            if len(heap) == k:
                result = min(result, s * worker[0])
        return result
```
