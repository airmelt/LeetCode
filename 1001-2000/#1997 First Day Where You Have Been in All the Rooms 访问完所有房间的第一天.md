# 1997 First Day Where You Have Been in All the Rooms 访问完所有房间的第一天

__Description:__

There are `n` rooms you need to visit, labeled from `0` to `n - 1`. Each day is labeled, starting from `0`. You will go in and visit one room a day.

Initially on day `0`, you visit room `0`. The __order__ you visit the rooms for the coming days is determined by the following __rules__ and a given __0-indexed__ array `nextVisit` of length `n`:

- Assuming that on a day, you visit room `i`,
- if you have been in room `i` an __odd__ number of times (__including__ the current visit), on the __next__ day you will visit a room with a __lower or equal room number__ specified by `nextVisit[i]` where `0 <= nextVisit[i] <= i`;
- if you have been in room `i` an __even__ number of times (__including__ the current visit), on the __next__ day you will visit room `(i + 1) mod n`.

Return _the label of the __first__ day where you have been in __all__ the rooms_. It can be shown that such a day exists. Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: nextVisit = [0,0]
Output: 2
Explanation:
```

- On day 0, you visit room 0. The total times you have been in room 0 is 1, which is odd.
  On the next day you will visit room nextVisit[0] = 0
- On day 1, you visit room 0, The total times you have been in room 0 is 2, which is even.
  On the next day you will visit room (0 + 1) mod 2 = 1
- On day 2, you visit room 1. This is the first day where you have been in all the rooms.

Example 2:

```text
Input: nextVisit = [0,0,2]
Output: 6
Explanation:
Your room visiting order for each day is: [0,0,1,0,0,1,2,...].
Day 6 is the first day where you have been in all the rooms.
```

Example 3:

```text
Input: nextVisit = [0,1,2,0]
Output: 6
Explanation:
Your room visiting order for each day is: [0,0,1,1,2,2,3,...].
Day 6 is the first day where you have been in all the rooms.
```

__Constraints:__

- `n == nextVisit.length`
- `2 <= n <= 10 ^ 5`
- `0 <= nextVisit[i] <= i`

__题目描述:__

你需要访问 `n` 个房间，房间从 `0` 到 `n - 1` 编号。同时，每一天都有一个日期编号，从 `0` 开始，依天数递增。你每天都会访问一个房间。

最开始的第 `0` 天，你访问 `0` 号房间。给你一个长度为 `n` 且 __下标从 0 开始__ 的数组 `nextVisit` 。在接下来的几天中，你访问房间的 __次序__ 将根据下面的 __规则__ 决定:

- 假设某一天，你访问 `i` 号房间。
- 如果算上本次访问，访问 `i` 号房间的次数为 __奇数__ ，那么 __第二天__ 需要访问 `nextVisit[i]` 所指定的房间，其中 `0 <= nextVisit[i] <= i` 。
- 如果算上本次访问，访问 `i` 号房间的次数为 __偶数__ ，那么 __第二天__ 需要访问 `(i + 1) mod n` 号房间。

请返回你访问完所有房间的第一天的日期编号。题目数据保证总是存在这样的一天。由于答案可能很大，返回对 `10 ^ 9 + 7` 取余后的结果。

__示例:__

示例 1：

```text
输入：nextVisit = [0,0]
输出：2
解释：
```

- 第 0 天，你访问房间 0 。访问 0 号房间的总次数为 1 ，次数为奇数。
  下一天你需要访问房间的编号是 nextVisit[0] = 0
- 第 1 天，你访问房间 0 。访问 0 号房间的总次数为 2 ，次数为偶数。
  下一天你需要访问房间的编号是 (0 + 1) mod 2 = 1
- 第 2 天，你访问房间 1 。这是你第一次完成访问所有房间的那天。

示例 2：

```text
输入：nextVisit = [0,0,2]
输出：6
解释：
你每天访问房间的次序是 [0,0,1,0,0,1,2,...] 。
第 6 天是你访问完所有房间的第一天。
```

示例 3：

```text
输入：nextVisit = [0,1,2,0]
输出：6
解释：
你每天访问房间的次序是 [0,0,1,1,2,2,3,...] 。
第 6 天是你访问完所有房间的第一天。
```

__提示：__

- `n == nextVisit.length`
- `2 <= n <= 10 ^ 5`
- `0 <= nextVisit[i] <= i`

__思路:__

```text
动态规划
0 号第 0 天会被访问
根据规则要访问 i(i > 0) 号房, 前一天必须访问 i - 1 号房, 而且此时一定是偶数次访问 i - 1 号房
设 dp[i] 表示访问 i 号房的天数
那么 dp[i] 为第二次到达 i - 1 号房的天数 + 1
第二次到达 i - 1 号房的天数为 dp[i - 1] + (i - 1 号房跳到 nextVisit[i - 1] 号房再回到 i - 1 号房的天数)
从第 i - 1 号房跳到 nextVisit[i - 1] 号房再回到 i - 1 号房的天数为 dp[i - 1] - dp[nextVisit[i - 1]] + 1, 只需要看结果即可
那么 dp[i] = dp[i - 1] + dp[i - 1] - dp[nextVisit[i - 1]] + 1 + 1
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int firstDayBeenInAllRooms(vector<int>& nextVisit) 
    {
        int MOD = 1e9 + 7, n = nextVisit.size(); 
        vector<long long> dp(n);
        for (int i = 1; i < n; i++) dp[i] = (MOD + (dp[i - 1] << 1) - dp[nextVisit[i - 1]] + 2) % MOD;
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int firstDayBeenInAllRooms(int[] nextVisit) {
        long MOD = 1_000_000_007L, dp[] = new long[nextVisit.length];
        for (int i = 1, n = nextVisit.length; i < n; i++) dp[i] = (MOD + (dp[i - 1] << 1) - dp[nextVisit[i - 1]] + 2) % MOD;
        return (int)((dp[nextVisit.length - 1] + MOD) % MOD);
    }
}
```

__Python__:

```Python
class Solution:
    def firstDayBeenInAllRooms(self, nextVisit: List[int]) -> int:
        dp, MOD = [0] * (n := len(nextVisit)), 10 ** 9 + 7 
        for i in range(1, n):
            dp[i] = (dp[i - 1] * 2 - dp[nextVisit[i - 1]] + 2) % MOD
        return dp[-1] % MOD
```
