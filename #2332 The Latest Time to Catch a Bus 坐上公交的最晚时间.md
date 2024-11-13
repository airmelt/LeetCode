# 2332 The Latest Time to Catch a Bus 坐上公交的最晚时间

__Description:__

You are given a __0-indexed__ integer array `buses` of length `n`, where `buses[i]` represents the departure time of the `i ^ th` bus. You are also given a __0-indexed__ integer array `passengers` of length `m`, where `passengers[j]` represents the arrival time of the `j ^ th` passenger. All bus departure times are unique. All passenger arrival times are unique.

You are given an integer `capacity`, which represents the __maximum__ number of passengers that can get on each bus.

When a passenger arrives, they will wait in line for the next available bus. You can get on a bus that departs at `x` minutes if you arrive at `y` minutes where `y <= x`, and the bus is not full. Passengers with the __earliest__ arrival times get on the bus first.

More formally when a bus arrives, either:

- If `capacity` or fewer passengers are waiting for a bus, they will __all__ get on the bus, or
- The `capacity` passengers with the __earliest__ arrival times will get on the bus.

Return the latest time you may arrive at the bus station to catch a bus. You cannot arrive at the same time as another passenger.

__Note:__ The arrays `buses` and `passengers` are not necessarily sorted.

__Example:__

Example 1:

```text
Input: buses = [10,20], passengers = [2,17,18,19], capacity = 2
Output: 16
Explanation: Suppose you arrive at time 16.
At time 10, the first bus departs with the 0th passenger. 
At time 20, the second bus departs with you and the 1st passenger.
Note that you may not arrive at the same time as another passenger, which is why you must arrive before the 1st passenger to catch the bus.
```

Example 2:

```text
Input: buses = [20,30,10], passengers = [19,13,26,4,25,11,21], capacity = 2
Output: 20
Explanation: Suppose you arrive at time 20.
At time 10, the first bus departs with the 3rd passenger. 
At time 20, the second bus departs with the 5th and 1st passengers.
At time 30, the third bus departs with the 0th passenger and you.
Notice if you had arrived any later, then the 6th passenger would have taken your seat on the third bus.
```

__Constraints:__

- `n == buses.length`
- `m == passengers.length`
- `1 <= n, m, capacity <= 10 ^ 5`
- `2 <= buses[i], passengers[i] <= 10 ^ 9`
- Each element in `buses` is __unique__.
- Each element in `passengers` is __unique__.

__题目描述:__

给你一个下标从 __0__ 开始长度为 `n` 的整数数组 `buses` ，其中 `buses[i]` 表示第 `i` 辆公交车的出发时间。同时给你一个下标从 __0__ 开始长度为 `m` 的整数数组 `passengers` ，其中 `passengers[j]` 表示第 `j` 位乘客的到达时间。所有公交车出发的时间互不相同，所有乘客到达的时间也互不相同。

给你一个整数 `capacity` ，表示每辆公交车 __最多__ 能容纳的乘客数目。

每位乘客都会排队搭乘下一辆有座位的公交车。如果你在 `y` 时刻到达，公交在 `x` 时刻出发，满足 `y <= x` 且公交没有满，那么你可以搭乘这一辆公交。__最早__ 到达的乘客优先上车。

返回你可以搭乘公交车的最晚到达公交站时间。你 不能 跟别的乘客同时刻到达。

__注意:__ 数组 `buses` 和 `passengers` 不一定是有序的。

__示例:__

示例 1：

```text
输入：buses = [10,20], passengers = [2,17,18,19], capacity = 2
输出：16
解释：
第 1 辆公交车载着第 1 位乘客。
第 2 辆公交车载着你和第 2 位乘客。
注意你不能跟其他乘客同一时间到达，所以你必须在第二位乘客之前到达。
```

示例 2：

```text
输入：buses = [20,30,10], passengers = [19,13,26,4,25,11,21], capacity = 2
输出：20
解释：
第 1 辆公交车载着第 4 位乘客。
第 2 辆公交车载着第 6 位和第 2 位乘客。
第 3 辆公交车载着第 1 位乘客和你。
```

__提示：__

- `n == buses.length`
- `m == passengers.length`
- `1 <= n, m, capacity <= 10 ^ 5`
- `2 <= buses[i], passengers[i] <= 10 ^ 9`
- `buses` 中的元素 __互不相同__ 。
- `passengers` 中的元素 __互不相同__ 。

__思路:__

```text
贪心
将公交车和乘客的时间排序
先不考虑自己的时间
遍历公交车的时间, 依次判断乘客的时间是否小于等于公交车的时间
依次将乘客加入公交车, 直到公交车满员或者乘客用完
如果最后一辆公交车没有满员, 则返回最后一辆公交车的时间
否则从当前乘客的时间开始往前遍历
找到第一个不等于当前乘客时间的时间, 返回该时间减一
时间复杂度为 O(MlogM + NlogN), 空间复杂度为 O(logM + logN), 主要是排序的空间复杂度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int latestTimeCatchTheBus(vector<int>& buses, vector<int>& passengers, int capacity) 
    {
        sort(buses.begin(), buses.end());
        sort(passengers.begin(), passengers.end());
        int i = 0, n = buses.size(), m = passengers.size(), c = 0;
        for (const auto& bus : buses) 
        {
            c = capacity;
            while (c > 0 and i < m and passengers[i] <= bus) 
            {
                --c;
                ++i;
            }
        }
        --i;
        int result = c > 0 ? buses[n - 1] : passengers[i];
        while (i > -1 and result == passengers[i--]) result--;
        return result;
    }
};
```

__Java__:

```Java
class Solution:
    def latestTimeCatchTheBus(self, buses: List[int], passengers: List[int], capacity: int) -> int:
        buses.sort()
        passengers.sort()
        i, n, m = 0, len(buses), len(passengers)
        for bus in buses:
            c = capacity
            while c and i < m and passengers[i] <= bus:
                i += 1
                c -= 1
        i -= 1
        result = buses[-1] if c else passengers[i]
        while i > -1 and result == passengers[i]:
            result -= 1
            i -= 1
        return result
```

__Python__:

```Python
class Solution:
    def latestTimeCatchTheBus(self, buses: List[int], passengers: List[int], capacity: int) -> int:
        buses.sort()
        passengers.sort()
        i, n, m = 0, len(buses), len(passengers)
        for bus in buses:
            c = capacity
            while c and i < m and passengers[i] <= bus:
                i += 1
                c -= 1
        i -= 1
        result = buses[-1] if c else passengers[i]
        while i > -1 and result == passengers[i]:
            result -= 1
            i -= 1
        return result
```
