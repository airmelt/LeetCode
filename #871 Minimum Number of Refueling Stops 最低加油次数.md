# 871 Minimum Number of Refueling Stops 最低加油次数

__Description__:
A car travels from a starting position to a destination which is target miles east of the starting position.

There are gas stations along the way. The gas stations are represented as an array stations where stations[i] = [positioni, fueli] indicates that the ith gas station is positioni miles east of the starting position and has fueli liters of gas.

The car starts with an infinite tank of gas, which initially has startFuel liters of fuel in it. It uses one liter of gas per one mile that it drives. When the car reaches a gas station, it may stop and refuel, transferring all the gas from the station into the car.

Return the minimum number of refueling stops the car must make in order to reach its destination. If it cannot reach the destination, return -1.

Note that if the car reaches a gas station with 0 fuel left, the car can still refuel there. If the car reaches the destination with 0 fuel left, it is still considered to have arrived.

__Example:__

Example 1:

Input: target = 1, startFuel = 1, stations = []
Output: 0
Explanation: We can reach the target without refueling.

Example 2:

Input: target = 100, startFuel = 1, stations = [[10,100]]
Output: -1
Explanation: We can not reach the target (or even the first gas station).

Example 3:

Input: target = 100, startFuel = 10, stations = [[10,60],[20,30],[30,30],[60,40]]
Output: 2
Explanation: We start with 10 liters of fuel.
We drive to position 10, expending 10 liters of fuel.  We refuel from 0 liters to 60 liters of gas.
Then, we drive from position 10 to position 60 (expending 50 liters of fuel),
and refuel from 10 liters to 50 liters of gas.  We then drive to and reach the target.
We made 2 refueling stops along the way, so we return 2.

__Constraints:__

1 <= target, startFuel <= 10^9
0 <= stations.length <= 500
0 <= positioni <= positioni+1 < target
1 <= fueli < 10^9

__题目描述__:
汽车从起点出发驶向目的地，该目的地位于出发位置东面 target 英里处。

沿途有加油站，每个 station[i] 代表一个加油站，它位于出发位置东面 station[i][0] 英里处，并且有 station[i][1] 升汽油。

假设汽车油箱的容量是无限的，其中最初有 startFuel 升燃料。它每行驶 1 英里就会用掉 1 升汽油。

当汽车到达加油站时，它可能停下来加油，将所有汽油从加油站转移到汽车中。

为了到达目的地，汽车所必要的最低加油次数是多少？如果无法到达目的地，则返回 -1 。

注意：如果汽车到达加油站时剩余燃料为 0，它仍然可以在那里加油。如果汽车到达目的地时剩余燃料为 0，仍然认为它已经到达目的地。

__示例 :__

示例 1：

输入：target = 1, startFuel = 1, stations = []
输出：0
解释：我们可以在不加油的情况下到达目的地。

示例 2：

输入：target = 100, startFuel = 1, stations = [[10,100]]
输出：-1
解释：我们无法抵达目的地，甚至无法到达第一个加油站。

示例 3：

输入：target = 100, startFuel = 10, stations = [[10,60],[20,30],[30,30],[60,40]]
输出：2
解释：
我们出发时有 10 升燃料。
我们开车来到距起点 10 英里处的加油站，消耗 10 升燃料。将汽油从 0 升加到 60 升。
然后，我们从 10 英里处的加油站开到 60 英里处的加油站（消耗 50 升燃料），
并将汽油从 10 升加到 50 升。然后我们开车抵达目的地。
我们沿途在1两个加油站停靠，所以返回 2 。

__提示:__

1 <= target, startFuel, stations[i][1] <= 10^9
0 <= stations.length <= 500
0 < stations[0][0] < stations[1][0] < ... < stations[stations.length-1][0] < target

__思路__:

本题是本人 19 年华工机试原题, 很遗憾没做出来
贪心 ➕ 优先队列
想象一下将加油站看作油桶, 直接带车上, 每次没油就加油最多的
油桶可以用堆维护
如果油用完了还不能到达 target 返回 -1
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minRefuelStops(int target, int startFuel, vector<vector<int>>& stations) 
    {
        int reach = startFuel, result = 0, n = stations.size();
        vector<bool> station_used(n, false);
        while (reach < target) 
        {
            int i = -1, pos = 0, max_fuel = -1, add_fuel = 0;
            while (++i < n and reach >= stations[i][0]) 
            {
                if (!station_used[i] and stations[i][1] > max_fuel) 
                {
                    pos = i;
                    max_fuel = stations[i][1];
                    add_fuel = 1;
                }
            }
            if (!add_fuel) return -1;
            reach += max_fuel;
            station_used[pos] = true;
            ++result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minRefuelStops(int target, int startFuel, int[][] stations) {
        int reach = startFuel, result = 0, n = stations.length;
        boolean stationUsed[] = new boolean[n];
        while (reach < target) {
            int i = -1, pos = 0, maxFuel = -1, addFuel = 0;
            while (++i < n && reach >= stations[i][0]) {
                if (!stationUsed[i] && stations[i][1] > maxFuel) {
                    pos = i;
                    maxFuel = stations[i][1];
                    addFuel = 1;
                }
            }
            if (addFuel == 0) return -1;
            reach += maxFuel;
            stationUsed[pos] = true;
            ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minRefuelStops(self, target: int, startFuel: int, stations: List[List[int]]) -> int:
        heap, remain, cur, result = [], startFuel, 0, 0
        while remain < target:
            for i in range(cur, len(stations)):
                if remain >= stations[i][0]:
                    heapq.heappush(heap, -stations[i][1])
                    cur += 1
            if remain < target:
                if not heap:
                    return -1
                remain -= heapq.heappop(heap)
                result += 1
        return result
```
