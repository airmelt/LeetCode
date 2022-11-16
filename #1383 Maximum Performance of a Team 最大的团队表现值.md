# 1383 Maximum Performance of a Team 最大的团队表现值

__Description:__

You are given two integers n and k and two integer arrays speed and efficiency both of length n. There are n engineers numbered from 1 to n. speed[i] and efficiency[i] represent the speed and efficiency of the ith engineer respectively.

Choose at most k different engineers out of the n engineers to form a team with the maximum performance.

The performance of a team is the sum of their engineers' speeds multiplied by the minimum efficiency among their engineers.

Return the maximum performance of this team. Since the answer can be a huge number, return it modulo 10^9 + 7.

__Example:__

Example 1:

Input: n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 2
Output: 60
Explanation:
We have the maximum performance of the team by selecting engineer 2 (with speed=10 and efficiency=4) and engineer 5 (with speed=5 and efficiency=7). That is, performance = (10 + 5) * min(4, 7) = 60.

Example 2:

Input: n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 3
Output: 68
Explanation:
This is the same example as the first but k = 3. We can select engineer 1, engineer 2 and engineer 5 to get the maximum performance of the team. That is, performance = (2 + 10 + 5) * min(5, 4, 7) = 68.

Example 3:

Input: n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 4
Output: 72

__Constraints:__

1 <= k <= n <= 10^5
speed.length == n
efficiency.length == n
1 <= speed[i] <= 10^5
1 <= efficiency[i] <= 10^8

__描述:__

公司有编号为 1 到 n 的 n 个工程师，给你两个数组 speed 和 efficiency ，其中 speed[i] 和 efficiency[i] 分别代表第 i 位工程师的速度和效率。请你返回由最多 k 个工程师组成的 最大团队表现值 ，由于答案可能很大，请你返回结果对 10^9 + 7 取余后的结果。

团队表现值 的定义为：一个团队中「所有工程师速度的和」乘以他们「效率值中的最小值」。

__示例:__

示例 1：

输入：n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 2
输出：60
解释：
我们选择工程师 2（speed=10 且 efficiency=4）和工程师 5（speed=5 且 efficiency=7）。他们的团队表现值为 performance = (10 + 5) * min(4, 7) = 60 。

示例 2：

输入：n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 3
输出：68
解释：
此示例与第一个示例相同，除了 k = 3 。我们可以选择工程师 1 ，工程师 2 和工程师 5 得到最大的团队表现值。表现值为 performance = (2 + 10 + 5) * min(5, 4, 7) = 68 。

示例 3：

输入：n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 4
输出：72

__提示：__

1 <= n <= 10^5
speed.length == n
efficiency.length == n
1 <= speed[i] <= 10^5
1 <= efficiency[i] <= 10^8
1 <= k <= n

__思路:__

```text
优先队列
需要计算速度和以及最小效率的乘积
如果将效率从大到小排序, 可以保证每次计算的时候当前效率就是所有工程师的最小值
将效率和速度对应插入大根堆, 每次取出堆顶, 则当前效率为当前所有工程师的最小值
然后将速度更新并插入另一个记录速度的小根堆
如果人数超过了 k 就减去速度最小的工程师
然后更新结果, 结果为当前结果和速度和与当前效率的乘积的较大值
时间复杂度为 O(nlgn), 空间复杂度为 O(n), 主要是排序需要的时间
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxPerformance(int n, vector<int>& speed, vector<int>& efficiency, int k) 
    {
        priority_queue<pair<int, int>> engineers;
        priority_queue<int> speed_queue;
        for (int i = 0; i < n; i++) engineers.push(make_pair(efficiency[i], speed[i]));
        long result = 0, speeds = 0, mod = 1e9 + 7;
        for (int i = 0; i < n; i++) {
            pair<int, int> engineer = engineers.top();
            engineers.pop();
            speeds += engineer.second;
            speed_queue.push(-engineer.second);
            if (i >= k)
            {
                speeds += speed_queue.top();
                speed_queue.pop();
            }
            result = max(result, speeds * engineer.first);
        }
        return result % mod;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxPerformance(int n, int[] speed, int[] efficiency, int k) {
        Queue<int[]> engineers = new PriorityQueue<>((o1, o2) -> o2[1]- o1[1]);
        Queue<Integer> speedQueue = new PriorityQueue<>();
        for (int i = 0; i < n; i++) engineers.offer(new int[]{speed[i], efficiency[i]});
        long speeds = 0, result = 0, mod = 1_000_000_007;
        for (int i = 0; i < n; i++) {
            int[] engineer = engineers.poll();
            speeds += engineer[0];
            speedQueue.offer(engineer[0]);
            speeds -= i >= k? speedQueue.poll() : 0;
            result = Math.max(result, speeds * engineer[1]);
        }
        return (int)(result % mod);
    }
}
```

__Python__:

```Python
class Solution:
    def maxPerformance(self, n: int, speed: List[int], efficiency: List[int], k: int) -> int:
        engineers, speed_queue, speeds, result, mod = [(-efficiency[i], speed[i]) for i in range(n)], [], 0, 0, 10 ** 9 + 7
        heapify(engineers)
        for i in range(n):
            speeds += (engineer := heappop(engineers))[1]
            heappush(speed_queue, engineer[1])
            speeds -= 0 if i < k else heappop(speed_queue)
            result = max(result, -speeds * engineer[0])
        return result % mod
```
