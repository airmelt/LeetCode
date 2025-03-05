# 2462 Total Cost to Hire K Workers 雇佣 K 位工人的总代价

__Description:__

You are given a __0-indexed__ integer array `costs` where `costs[i]` is the cost of hiring the `i ^ th` worker.

You are also given two integers `k` and `candidates`. We want to hire exactly `k` workers according to the following rules:

- You will run `k` sessions and hire exactly one worker in each session.
- In each hiring session, choose the worker with the lowest cost from either the first `candidates` workers or the last `candidates` workers. Break the tie by the smallest index.
  - For example, if `costs = [3,2,7,7,1,2]` and `candidates = 2`, then in the first hiring session, we will choose the `4 ^ th` worker because they have the lowest cost [3,2,7,7,__1__,2].
  - In the second hiring session, we will choose `1 ^ st` worker because they have the same lowest cost as `4 ^ th` worker but they have the smallest index [3,__2__,7,7,2]. Please note that the indexing may be changed in the process.
- If there are fewer than candidates workers remaining, choose the worker with the lowest cost among them. Break the tie by the smallest index.
- A worker can only be chosen once.

Return _the total cost to hire exactly_ `k` _workers._

__Example:__

Example 1:

```text
Input: costs = [17,12,10,2,7,2,11,20,8], k = 3, candidates = 4
Output: 11
Explanation: We hire 3 workers in total. The total cost is initially 0.
```

- In the first hiring round we choose the worker from [17,12,10,2,7,2,11,20,8]. The lowest cost is 2, and we break the tie by the smallest index, which is 3. The total cost = 0 + 2 = 2.
- In the second hiring round we choose the worker from [17,12,10,7,2,11,20,8]. The lowest cost is 2 (index 4). The total cost = 2 + 2 = 4.
- In the third hiring round we choose the worker from [17,12,10,7,11,20,8]. The lowest cost is 7 (index 3). The total cost = 4 + 7 = 11. Notice that the worker with index 3 was common in the first and last four workers.

The total hiring cost is 11.

Example 2:

```text
Input: costs = [1,2,4,1], k = 3, candidates = 3
Output: 4
Explanation: We hire 3 workers in total. The total cost is initially 0.
```

- In the first hiring round we choose the worker from [1,2,4,1]. The lowest cost is 1, and we break the tie by the smallest index, which is 0. The total cost = 0 + 1 = 1. Notice that workers with index 1 and 2 are common in the first and last 3 workers.
- In the second hiring round we choose the worker from [2,4,1]. The lowest cost is 1 (index 2). The total cost = 1 + 1 = 2.
- In the third hiring round there are less than three candidates. We choose the worker from the remaining workers [2,4]. The lowest cost is 2 (index 0). The total cost = 2 + 2 = 4.

The total hiring cost is 4.

__Constraints:__

- `1 <= costs.length <= 10 ^ 5`
- `1 <= costs[i] <= 10 ^ 5`
- `1 <= k, candidates <= costs.length`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `costs` ，其中 `costs[i]` 是雇佣第 `i` 位工人的代价。

同时给你两个整数 `k` 和 `candidates` 。我们想根据以下规则恰好雇佣 `k` 位工人:

- 总共进行 `k` 轮雇佣，且每一轮恰好雇佣一位工人。
- 在每一轮雇佣中，从最前面 `candidates` 和最后面 `candidates` 人中选出代价最小的一位工人，如果有多位代价相同且最小的工人，选择下标更小的一位工人。
  - 比方说， `costs = [3,2,7,7,1,2]` 且 `candidates = 2` ，第一轮雇佣中，我们选择第 `4` 位工人，因为他的代价最小 [_3,2_,7,7,___1__,2_] 。
  - 第二轮雇佣，我们选择第 `1` 位工人，因为他们的代价与第 `4` 位工人一样都是最小代价，而且下标更小， [_3,__2___,7,_7,2_] 。注意每一轮雇佣后，剩余工人的下标可能会发生变化。
- 如果剩余员工数目不足 `candidates` 人，那么下一轮雇佣他们中代价最小的一人，如果有多位代价相同且最小的工人，选择下标更小的一位工人。
- 一位工人只能被选择一次。

返回雇佣恰好 `k` 位工人的总代价。

__示例:__

示例 1：

```text
输入：costs = [17,12,10,2,7,2,11,20,8], k = 3, candidates = 4
输出：11
解释：我们总共雇佣 3 位工人。总代价一开始为 0 。
```

- 第一轮雇佣，我们从 [17,12,10,2,7,2,11,20,8] 中选择。最小代价是 2 ，有两位工人，我们选择下标更小的一位工人，即第 3 位工人。总代价是 0 + 2 = 2 。
- 第二轮雇佣，我们从 [17,12,10,7,2,11,20,8] 中选择。最小代价是 2 ，下标为 4 ，总代价是 2 + 2 = 4 。
- 第三轮雇佣，我们从 [17,12,10,7,11,20,8] 中选择，最小代价是 7 ，下标为 3 ，总代价是 4 + 7 = 11 。注意下标为 3 的工人同时在最前面和最后面 4 位工人中。

总雇佣代价是 11 。

示例 2：

```text
输入：costs = [1,2,4,1], k = 3, candidates = 3
输出：4
解释：我们总共雇佣 3 位工人。总代价一开始为 0 。
```

- 第一轮雇佣，我们从 [1,2,4,1] 中选择。最小代价为 1 ，有两位工人，我们选择下标更小的一位工人，即第 0 位工人，总代价是 0 + 1 = 1 。注意，下标为 1 和 2 的工人同时在最前面和最后面 3 位工人中。
- 第二轮雇佣，我们从 [2,4,1] 中选择。最小代价为 1 ，下标为 2 ，总代价是 1 + 1 = 2 。
- 第三轮雇佣，少于 3 位工人，我们从剩余工人 [2,4] 中选择。最小代价是 2 ，下标为 0 。总代价为 2 + 2 = 4 。

总雇佣代价是 4 。

__提示：__

- `1 <= costs.length <= 10 ^ 5`
- `1 <= costs[i] <= 10 ^ 5`
- `1 <= k, candidates <= costs.length`

__思路:__

```text
优先队列
用两个优先队列 pre, suf 分别记录前 candidates 个和后 candidates 个工人
先判断是否可以选到所有工人
如果 candidates * 2 + k > n, 直接排序取前 k 个工人
每次选择代价最小的工人
选择之后移动对应指针将下一个工人加入对应优先队列
注意 Cpp 中优先队列默认是大根堆，所以取负值
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long totalCost(vector<int>& costs, int k, int candidates) 
    {
        int n = costs.size(), i = candidates, j = n - 1 - candidates;
        long result = 0LL;
        if ((candidates << 1) + k > n) 
        {
            sort(costs.begin(), costs.end());
            while (k--) result += costs[k];
            return result;
        }
        priority_queue<int> pre, suf;
        while (candidates--) 
        {
            pre.push(-costs[candidates]);
            suf.push(-costs[n - 1 - candidates]);
        }
        while (k--) 
        {
            if (pre.top() >= suf.top()) 
            {
                result += -pre.top();
                pre.pop();
                pre.push(-costs[i++]);
            } 
            else 
            {
                result += -suf.top();
                suf.pop();
                suf.push(-costs[j--]);
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long totalCost(int[] costs, int k, int candidates) {
        int n = costs.length, i = candidates, j = n - 1 - candidates;
        long result = 0L;
        if ((candidates << 1) + k > n) {
            Arrays.sort(costs);
            while (k-- > 0) result += costs[k];
            return result;
        }
        PriorityQueue<Integer> pre = new PriorityQueue<>(), suf = new PriorityQueue<>();
        while (candidates-- > 0) {
            pre.offer(costs[candidates]);
            suf.offer(costs[n - 1 - candidates]);
        }
        while (k-- > 0) {
            if (pre.peek() <= suf.peek()) {
                result += pre.poll();
                pre.offer(costs[i++]);
            } else {
                result += suf.poll();
                suf.offer(costs[j--]);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def totalCost(self, costs: List[int], k: int, candidates: int) -> int:
        if (candidates << 1) + k > (n := len(costs)):
            return sum(sorted(costs)[:k])
        pre, suf, result, i, j = costs[:candidates], costs[-candidates:], 0, candidates, n - 1 - candidates
        heapify(pre), heapify(suf)
        for _ in range(k):
            if pre[0] <= suf[0]:
                result += heapreplace(pre, costs[i])
                i += 1
            else:
                result += heapreplace(suf, costs[j])
                j -= 1
        return result
```
