# 2141 Maximum Running Time of N Computers 同时运行 N 台电脑的最长时间

__Description:__

You have `n` computers. You are given the integer `n` and a __0-indexed__ integer array `batteries` where the `i ^ th` battery can __run__ a computer for `batteries[i]` minutes. You are interested in running __all__ `n` computers __simultaneously__ using the given batteries.

Initially, you can insert at most one battery into each computer. After that and at any integer time moment, you can remove a battery from a computer and insert another battery any number of times. The inserted battery can be a totally new battery or a battery from another computer. You may assume that the removing and inserting processes take no time.

Note that the batteries cannot be recharged.

Return _the __maximum__ number of minutes you can run all the_ `n` _computers simultaneously._

__Example:__

Example 1:

![2141-1](https://assets.leetcode.com/uploads/2022/01/06/example1-fit.png)

```text
Input: n = 2, batteries = [3,3,3]
Output: 4
Explanation: 
Initially, insert battery 0 into the first computer and battery 1 into the second computer.
After two minutes, remove battery 1 from the second computer and insert battery 2 instead. Note that battery 1 can still run for one minute.
At the end of the third minute, battery 0 is drained, and you need to remove it from the first computer and insert battery 1 instead.
By the end of the fourth minute, battery 1 is also drained, and the first computer is no longer running.
We can run the two computers simultaneously for at most 4 minutes, so we return 4.
```

Example 2:

![2141-2](https://assets.leetcode.com/uploads/2022/01/06/example2.png)

```text
Input: n = 2, batteries = [1,1,1,1]
Output: 2
Explanation: 
Initially, insert battery 0 into the first computer and battery 2 into the second computer. 
After one minute, battery 0 and battery 2 are drained so you need to remove them and insert battery 1 into the first computer and battery 3 into the second computer. 
After another minute, battery 1 and battery 3 are also drained so the first and second computers are no longer running.
We can run the two computers simultaneously for at most 2 minutes, so we return 2.
```

__Constraints:__

- `1 <= n <= batteries.length <= 10 ^ 5`
- `1 <= batteries[i] <= 10 ^ 9`

__题目描述:__

你有 `n` 台电脑。给你整数 `n` 和一个下标从 __0__ 开始的整数数组 `batteries` ，其中第 `i` 个电池可以让一台电脑 __运行__ `batteries[i]` 分钟。你想使用这些电池让 __全部__ `n` 台电脑 _同时_ 运行。

一开始，你可以给每台电脑连接 至多一个电池 。然后在任意整数时刻，你都可以将一台电脑与它的电池断开连接，并连接另一个电池，你可以进行这个操作 任意次 。新连接的电池可以是一个全新的电池，也可以是别的电脑用过的电池。断开连接和连接新的电池不会花费任何时间。

注意，你不能给电池充电。

请你返回你可以让 `n` 台电脑同时运行的 __最长__ 分钟数。

__示例:__

示例 1：

![2141-3](https://assets.leetcode.com/uploads/2022/01/06/example1-fit.png)

```text
输入：n = 2, batteries = [3,3,3]
输出：4
解释：
一开始，将第一台电脑与电池 0 连接，第二台电脑与电池 1 连接。
2 分钟后，将第二台电脑与电池 1 断开连接，并连接电池 2 。注意，电池 0 还可以供电 1 分钟。
在第 3 分钟结尾，你需要将第一台电脑与电池 0 断开连接，然后连接电池 1 。
在第 4 分钟结尾，电池 1 也被耗尽，第一台电脑无法继续运行。
我们最多能同时让两台电脑同时运行 4 分钟，所以我们返回 4 。
```

示例 2：

![2141-4](https://assets.leetcode.com/uploads/2022/01/06/example2.png)

```text
输入：n = 2, batteries = [1,1,1,1]
输出：2
解释：
一开始，将第一台电脑与电池 0 连接，第二台电脑与电池 2 连接。
一分钟后，电池 0 和电池 2 同时耗尽，所以你需要将它们断开连接，并将电池 1 和第一台电脑连接，电池 3 和第二台电脑连接。
1 分钟后，电池 1 和电池 3 也耗尽了，所以两台电脑都无法继续运行。
我们最多能让两台电脑同时运行 2 分钟，所以我们返回 2 。
```

__提示：__

- `1 <= n <= batteries.length <= 10 ^ 5`
- `1 <= batteries[i] <= 10 ^ 9`

__思路:__

```text
1. 二分
如果我们可以让 n 台电脑同时运行的最长时间为 mid, 那么我们一定可以让每台电脑同时运行 mid - 1 分钟
即满足单调性
最多可以运行的时间为 sum(batteries) / n
最少可以运行的时间为 0
check 函数检查是否可以让 n 台电脑同时运行 T 分钟
总共需要运行 n * mid 分钟
对于每个电池, 我们可以运行 min(b, mid) 分钟
累计所有电池的运行时间, 如果大于等于 n * mid, 那么可以让 n 台电脑同时运行 mid 分钟
时间复杂度为 O(NlogM), 空间复杂度为 O(1), 其中 M 为 batteries 中电池的和
2. 贪心
将电池按照从大到小排序
理论上最大供应时间为 x = sum(batteries) / n
如果电池电量比 x 要大, 那么让这个电池供应 x 时间, 减少为 n - 1 的子问题
如果电池电量比 x 要小, 那么最多只能供应 sum(batteries) / n 时间, 因为已经排序, 其他电池只会比 x 更小
直接返回 sum(batteries) / n
时间复杂度为 O(NlogN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maxRunTime(int n, vector<int>& batteries) 
    {
        long long left = 0LL, right = accumulate(batteries.begin(), batteries.end(), 0LL) / n;
        auto check = [&](auto mid) -> bool
        {
            auto result = 0LL;
            for (const auto& b : batteries) result += min(static_cast<long long>(b), mid);
            return n * mid <= result;
        };
        while (left <= right) 
        {
            long long mid = left + ((right - left) >> 1);
            if (check(mid)) left = mid + 1;
            else right = mid - 1;
        }
        return right;
    }
};
```

__Java__:

```Java
class Solution:
    def maxRunTime(self, n: int, batteries: List[int]) -> int:
        batteries.sort(reverse=True)
        s = sum(batteries)
        for b in batteries:
            if b <= s // n:
                return s // n
            s -= b
            n -= 1
```

__Python__:

```Python
class Solution:
    def maxRunTime(self, n: int, batteries: List[int]) -> int:
        batteries.sort(reverse=True)
        s = sum(batteries)
        for b in batteries:
            if b <= s // n:
                return s // n
            s -= b
            n -= 1
```
