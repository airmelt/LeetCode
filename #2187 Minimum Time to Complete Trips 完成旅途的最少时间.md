# 2187 Minimum Time to Complete Trips 完成旅途的最少时间

__Description:__

You are given an array `time` where `time[i]` denotes the time taken by the `i ^ th` bus to complete __one trip__.

Each bus can make multiple trips successively; that is, the next trip can start immediately after completing the current trip. Also, each bus operates independently; that is, the trips of one bus do not influence the trips of any other bus.

You are also given an integer `totalTrips`, which denotes the number of trips all buses should make __in total__. Return _the __minimum time__ required for all buses to complete __at least___ `totalTrips` _trips_.

__Example:__

Example 1:

```text
Input: time = [1,2,3], totalTrips = 5
Output: 3
Explanation:
```

- At time t = 1, the number of trips completed by each bus are [1,0,0].
  The total number of trips completed is 1 + 0 + 0 = 1.
- At time t = 2, the number of trips completed by each bus are [2,1,0].
  The total number of trips completed is 2 + 1 + 0 = 3.
- At time t = 3, the number of trips completed by each bus are [3,1,1].

  The total number of trips completed is 3 + 1 + 1 = 5.
So the minimum time needed for all buses to complete at least 5 trips is 3.

Example 2:

```text
Input: time = [2], totalTrips = 1
Output: 2
Explanation:
There is only one bus, and it will complete its first trip at t = 2.
So the minimum time needed to complete 1 trip is 2.
```

__Constraints:__

- `1 <= time.length <= 10 ^ 5`
- `1 <= time[i], totalTrips <= 10 ^ 7`

__题目描述:__

给你一个数组 `time` ，其中 `time[i]` 表示第 `i` 辆公交车完成 __一趟__ __旅途__ 所需要花费的时间。

每辆公交车可以 连续 完成多趟旅途，也就是说，一辆公交车当前旅途完成后，可以 立马开始 下一趟旅途。每辆公交车 独立 运行，也就是说可以同时有多辆公交车在运行且互不影响。

给你一个整数 `totalTrips` ，表示所有公交车 __总共__ 需要完成的旅途数目。请你返回完成 __至少__ `totalTrips` 趟旅途需要花费的 __最少__ 时间。

__示例:__

示例 1：

```text
输入：time = [1,2,3], totalTrips = 5
输出：3
解释：
```

- 时刻 t = 1 ，每辆公交车完成的旅途数分别为 [1,0,0] 。
  已完成的总旅途数为 1 + 0 + 0 = 1 。
- 时刻 t = 2 ，每辆公交车完成的旅途数分别为 [2,1,0] 。
  已完成的总旅途数为 2 + 1 + 0 = 3 。
- 时刻 t = 3 ，每辆公交车完成的旅途数分别为 [3,1,1] 。
  已完成的总旅途数为 3 + 1 + 1 = 5 。

所以总共完成至少 5 趟旅途的最少时间为 3 。

示例 2：

```text
输入：time = [2], totalTrips = 1
输出：2
解释：
只有一辆公交车，它将在时刻 t = 2 完成第一趟旅途。
所以完成 1 趟旅途的最少时间为 2 。
```

__提示：__

- `1 <= time.length <= 10 ^ 5`
- `1 <= time[i], totalTrips <= 10 ^ 7`

__思路:__

```text
二分
如果用时 t 能完成, 那么 t + 1 也能完成
最长用时为 min(time) * totalTrips, 为了使用开区间查找可以加 1
最短用时为 min(time), 这时 totalTrips 为 1
使用二分查找, 每次计算在 t 时刻能完成的旅途数
即计算 mid / t 的总和
如果 trips 不足, left = mid + 1
否则 right = mid
时间复杂度为 O(NlogC), 空间复杂度为 O(1), 其中 C = min(time) * totalTrips
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minimumTime(vector<int>& time, int totalTrips) 
    {
        long long left = (long long)*min_element(time.begin(), time.end()), right = left * totalTrips + 1LL, mid = 0LL;
        auto check = [](auto mid, auto time) { auto result = 0LL; for (const auto& t : time) result += mid / t; return result; };
        while (left != right) 
        {
            if (check(mid = left + ((right - left) >> 1LL), time) < totalTrips) left = mid + 1;
            else right = mid;
        }
        return left;
    }
};
```

__Java__:

```Java
class Solution 
{
public:
    long long minimumTime(vector<int>& time, int totalTrips) 
    {
        long long left = (long long)*min_element(time.begin(), time.end()), right = left * totalTrips + 1LL, mid = 0LL;
        auto check = [](auto mid, auto time) { auto result = 0LL; for (const auto& t : time) result += mid / t; return result; };
        while (left != right) 
        {
            if (check(mid = left + ((right - left) >> 1LL), time) < totalTrips) left = mid + 1;
            else right = mid;
        }
        return left;
    }
};
```

__Python__:

```Python
class Solution:
    def minimumTime(self, time: List[int], totalTrips: int) -> int:
        return bisect_left(range(totalTrips * min(time)), totalTrips, lo=min(time), key=lambda x: sum(x // t for t in time))
```
