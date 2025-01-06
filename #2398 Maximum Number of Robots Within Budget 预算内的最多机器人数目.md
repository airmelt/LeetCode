# 2398 Maximum Number of Robots Within Budget 预算内的最多机器人数目

__Description:__

You have `n` robots. You are given two __0-indexed__ integer arrays, `chargeTimes` and `runningCosts`, both of length `n`. The `i ^ th` robot costs `chargeTimes[i]` units to charge and costs `runningCosts[i]` units to run. You are also given an integer `budget`.

The total cost of running `k` chosen robots is equal to `max(chargeTimes) + k * sum(runningCosts)`, where `max(chargeTimes)` is the largest charge cost among the `k` robots and `sum(runningCosts)` is the sum of running costs among the `k` robots.

Return the __maximum__ number of __consecutive__ robots you can run such that the total cost __does not__ exceed `budget`.

__Example:__

Example 1:

```text
Input: chargeTimes = [3,6,1,3,4], runningCosts = [2,1,3,4,5], budget = 25
Output: 3
Explanation: 
It is possible to run all individual and consecutive pairs of robots within budget.
To obtain answer 3, consider the first 3 robots. The total cost will be max(3,6,1) + 3 * sum(2,1,3) = 6 + 3 * 6 = 24 which is less than 25.
It can be shown that it is not possible to run more than 3 consecutive robots within budget, so we return 3.
```

Example 2:

```text
Input: chargeTimes = [11,12,19], runningCosts = [10,8,7], budget = 19
Output: 0
Explanation: No robot can be run that does not exceed the budget, so we return 0.
```

__Constraints:__

- `chargeTimes.length == runningCosts.length == n`
- `1 <= n <= 5 * 10 ^ 4`
- `1 <= chargeTimes[i], runningCosts[i] <= 10 ^ 5`
- `1 <= budget <= 10 ^ 15`

__题目描述:__

你有 `n` 个机器人，给你两个下标从 __0__ 开始的整数数组 `chargeTimes` 和 `runningCosts` ，两者长度都为 `n` 。第 `i` 个机器人充电时间为 `chargeTimes[i]` 单位时间，花费 `runningCosts[i]` 单位时间运行。再给你一个整数 `budget` 。

运行 `k` 个机器人 总开销 是 `max(chargeTimes) + k * sum(runningCosts)` ，其中 `max(chargeTimes)` 是这 `k` 个机器人中最大充电时间，`sum(runningCosts)` 是这 `k` 个机器人的运行时间之和。

请你返回在 __不超过__ `budget` 的前提下，你 __最多__ 可以 __连续__ 运行的机器人数目为多少。

__示例:__

示例 1：

```text
输入：chargeTimes = [3,6,1,3,4], runningCosts = [2,1,3,4,5], budget = 25
输出：3
解释：
可以在 budget 以内运行所有单个机器人或者连续运行 2 个机器人。
选择前 3 个机器人，可以得到答案最大值 3 。总开销是 max(3,6,1) + 3 * sum(2,1,3) = 6 + 3 * 6 = 24 ，小于 25 。
可以看出无法在 budget 以内连续运行超过 3 个机器人，所以我们返回 3 。
```

示例 2：

```text
输入：matrix = [[1],[0]], numSelect = 1
输入：chargeTimes = [11,12,19], runningCosts = [10,8,7], budget = 19
输出：0
解释：即使运行任何一个单个机器人，还是会超出 budget，所以我们返回 0 。
```

__提示：__

- `chargeTimes.length == runningCosts.length == n`
- `1 <= n <= 5 * 10 ^ 4`
- `1 <= chargeTimes[i], runningCosts[i] <= 10 ^ 5`
- `1 <= budget <= 10 ^ 15`

__思路:__

```text
单调队列 ➕ 滑动窗口
单调队列维护最大值, 队首到队尾单调递减
枚举右边界, 并将单调队列所有不大于当前值的元素弹出
用 s 维护滑动窗口内的元素之和
如果超出预算, 左边界右移
如果队首刚好是左边界, 队首出队
并维护滑动窗口内元素之和
更新滑动窗口长度的最大值为结果
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumRobots(vector<int>& chargeTimes, vector<int>& runningCosts, long long budget) 
    {
        int result = 0, left = 0, n = chargeTimes.size();
        long long s = 0LL;
        deque<int> q;
        for (int right = 0; right < n; right++)
        {
            while (!q.empty() and chargeTimes[right] >= chargeTimes[q.back()]) q.pop_back();
            q.push_back(right);
            s += runningCosts[right];
            while (!q.empty() and chargeTimes[q.front()] + (right - left + 1) * s > budget)
            {
                if (q.front() == left) q.pop_front();
                s -= runningCosts[left++];
            }
            result = max(result, right - left + 1);
        }    
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumRobots(int[] chargeTimes, int[] runningCosts, long budget) {
        int result = 0, left = 0, n = chargeTimes.length;
        long s = 0L;
        Deque<Integer> q = new ArrayDeque<>();
        for (int right = 0; right < n; right++) {
            while (!q.isEmpty() && chargeTimes[right] >= chargeTimes[q.peekLast()]) q.pollLast();
            q.addLast(right);
            s += runningCosts[right];
            while (!q.isEmpty() && chargeTimes[q.peekFirst()] + (right - left + 1) * s > budget) {
                if (q.peekFirst() == left) q.pollFirst();
                s -= runningCosts[left++];
            }
            result = Math.max(result, right - left + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumRobots(self, chargeTimes: List[int], runningCosts: List[int], budget: int) -> int:
        left, right, pre = 1, (n := len(chargeTimes)), [0] + list(accumulate(runningCosts))
        def check(mid: int) -> int:
            result, queue = float('inf'), deque()
            for i in range(n):
                if queue and i - queue[0] >= mid:
                    queue.popleft()
                while queue and chargeTimes[queue[-1]] < chargeTimes[i]:
                    queue.pop()
                queue.append(i)
                if i > mid - 2:
                    result = min(result, chargeTimes[queue[0]] + mid * (pre[i + 1] - pre[i - mid + 1]))
            return result
        while left <= right:
            mid = left + right >> 1
            if check(mid) <= budget:
                left = mid + 1
            else:
                right = mid - 1
        return right
```
