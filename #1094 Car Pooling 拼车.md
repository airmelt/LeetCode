# 1094 Car Pooling 拼车

__Description__:
There is a car with capacity empty seats. The vehicle only drives east (i.e., it cannot turn around and drive west).

You are given the integer capacity and an array trips where trips[i] = [numPassengersi, fromi, toi] indicates that the ith trip has numPassengersi passengers and the locations to pick them up and drop them off are fromi and toi respectively. The locations are given as the number of kilometers due east from the car's initial location.

Return true if it is possible to pick up and drop off all passengers for all the given trips, or false otherwise.

__Example:__

Example 1:

Input: trips = [[2,1,5],[3,3,7]], capacity = 4
Output: false

Example 2:

Input: trips = [[2,1,5],[3,3,7]], capacity = 5
Output: true

__Constraints:__

1 <= trips.length <= 1000
trips[i].length == 3
1 <= numPassengersi <= 100
0 <= fromi < toi <= 1000
1 <= capacity <= 10^5

__题目描述__:
车上最初有 capacity 个空座位。车 只能 向一个方向行驶（也就是说，不允许掉头或改变方向）

给定整数 capacity 和一个数组 trips ,  trip[i] = [numPassengersi, fromi, toi] 表示第 i 次旅行有 numPassengersi 乘客，接他们和放他们的位置分别是 fromi 和 toi 。这些位置是从汽车的初始位置向东的公里数。

当且仅当你可以在所有给定的行程中接送所有乘客时，返回 true，否则请返回 false。

__示例 :__

示例 1：

输入：trips = [[2,1,5],[3,3,7]], capacity = 4
输出：false

示例 2：

输入：trips = [[2,1,5],[3,3,7]], capacity = 5
输出：true

__提示:__

1 <= trips.length <= 1000
trips[i].length == 3
1 <= numPassengersi <= 100
0 <= fromi < toi <= 1000
1 <= capacity <= 10^5

__思路__:

差分数组
由于最大的站只有1000, 可以初始化一个 1001 的数组
上车和下车的站分别加上及减去相应乘客数
所有站的乘客数都不能大于 capacity
时间复杂度O(n), 空间复杂度O(1), 这里 n 最大为 1000

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool carPooling(vector<vector<int>>& trips, int capacity) 
    {
        vector<int> road(1001, 0);
        for (const auto& t: trips)
        {
            road[t[1]] += t[0];
            road[t[2]] -= t[0];
        }
        int count = 0;
        for (const auto& i: road)
        {
            count += i;
            if (count > capacity) return false;
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        int road[] = new int[1001], count = 0;
        for (int[] t: trips) {
            road[t[1]] += t[0];
            road[t[2]] -= t[0];
        }
        for (int i: road) {
            count += i;
            if (count > capacity) return false;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def carPooling(self, trips: List[List[int]], capacity: int) -> bool:
        roads = [0] * 1001
        for t in trips:
            roads[t[1]] += t[0]
            roads[t[2]] -= t[0]
        return all(r <= capacity for r in list(accumulate(roads)))
```
