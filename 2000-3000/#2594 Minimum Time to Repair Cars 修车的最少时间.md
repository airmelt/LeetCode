# 2594 Minimum Time to Repair Cars 修车的最少时间

__Description:__

You are given an integer array `ranks` representing the __ranks__ of some mechanics. ranks_i is the rank of the i ^ th mechanic. A mechanic with a rank `r` can repair n cars in `r * n ^ 2` minutes.

You are also given an integer `cars` representing the total number of cars waiting in the garage to be repaired.

Return the minimum time taken to repair all the cars.

Note: All the mechanics can repair the cars simultaneously.

__Example:__

Example 1:

```text
Input: ranks = [4,2,3,1], cars = 10
Output: 16
Explanation: 
```

- The first mechanic will repair two cars. The time required is 4 \* 2 \* 2 = 16 minutes.
- The second mechanic will repair two cars. The time required is 2 \* 2 \* 2 = 8 minutes.
- The third mechanic will repair two cars. The time required is 3 \* 2 \* 2 = 12 minutes.
- The fourth mechanic will repair four cars. The time required is 1 \* 4 \* 4 = 16 minutes.

It can be proved that the cars cannot be repaired in less than 16 minutes.

Example 2:

```text
Input: ranks = [5,1,8], cars = 6
Output: 16
Explanation: 
```

- The first mechanic will repair one car. The time required is 5 \* 1 \* 1 = 5 minutes.
- The second mechanic will repair four cars. The time required is 1 \* 4 \* 4 = 16 minutes.
- The third mechanic will repair one car. The time required is 8 \* 1 \* 1 = 8 minutes.

It can be proved that the cars cannot be repaired in less than 16 minutes.

__Constraints:__

- `1 <= ranks.length <= 10 ^ 5`
- `1 <= ranks[i] <= 100`
- `1 <= cars <= 10 ^ 6`

__题目描述:__

给你一个整数数组 `ranks` ，表示一些机械工的 __能力值__ 。 `ranks_i` 是第 `i` 位机械工的能力值。能力值为 `r` 的机械工可以在 `r \* n ^ 2` 分钟内修好 `n` 辆车。

同时给你一个整数 `cars` ，表示总共需要修理的汽车数目。

请你返回修理所有汽车 最少 需要多少时间。

注意：所有机械工可以同时修理汽车。

__示例:__

示例 1：

```text
输入：ranks = [4,2,3,1], cars = 10
输出：16
解释：
```

- 第一位机械工修 2 辆车，需要 4 \* 2 \* 2 = 16 分钟。
- 第二位机械工修 2 辆车，需要 2 \* 2 \* 2 = 8 分钟。
- 第三位机械工修 2 辆车，需要 3 \* 2 \* 2 = 12 分钟。
- 第四位机械工修 4 辆车，需要 1 \* 4 \* 4 = 16 分钟。

16 分钟是修理完所有车需要的最少时间。

示例 2：

```text
输入：ranks = [5,1,8], cars = 6
输出：16
解释：
```

- 第一位机械工修 1 辆车，需要 5 \* 1 \* 1 = 5 分钟。
- 第二位机械工修 4 辆车，需要 1 \* 4 \* 4 = 16 分钟。
- 第三位机械工修 1 辆车，需要 8 \* 1 \* 1 = 8 分钟。

16 分钟时修理完所有车需要的最少时间。

__提示：__

- `1 <= ranks.length <= 10 ^ 5`
- `1 <= ranks[i] <= 100`
- `1 <= cars <= 10 ^ 6`

__思路:__

```text
二分
找到最小时间使得修理工刚好能够修完所有的车
下界为 1
上届可以取 min(ranks) * cars ^ 2
时间复杂度为 O(NlogMC), 空间复杂度为 O(N), M 为 ranks 的最小值, C 为 cars 
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long repairCars(vector<int>& ranks, int cars) 
    {
        long long left = 1LL, right = 1LL << 62LL, mid = 0LL;
        while (left <= right) 
        {
            if (check(mid = left + ((right - left) >> 1LL), ranks, cars)) right = mid - 1LL;
            else left = mid + 1LL;
        }
        return left;
    }
private:
    bool check(long long mid, vector<int>& ranks, long long cars) 
    {
        long long s = 0LL;
        for (const auto& rank : ranks) s += sqrt(mid / rank);
        return s >= cars;
    }
};
```

__Java__:

```Java
class Solution {
    public long repairCars(int[] ranks, int cars) {
        long left = 1L, right = 1L << 62L, mid = 0L;
        while (left <= right) {
            if (check(mid = left + ((right - left) >>> 1L), ranks, cars)) right = mid - 1L;
            else left = mid + 1L;
        }
        return left;
    }

    private boolean check(long mid, int[] ranks, long cars) {
        long s = 0L;
        for (int rank : ranks) s += Math.sqrt(mid / rank);
        return s >= cars;
    }
}
```

__Python__:

```Python
class Solution:
    def repairCars(self, ranks: List[int], cars: int) -> int:
        return 0 if (f := lambda mid: sum(floor(sqrt(mid / r)) for r in ranks)) and not ranks else bisect_left(range(ranks[0] * cars ** 2), cars, key=f)
```
