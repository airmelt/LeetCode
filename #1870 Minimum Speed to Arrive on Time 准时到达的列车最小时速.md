# 1870 Minimum Speed to Arrive on Time 准时到达的列车最小时速

__Description:__

You are given a floating-point number `hour`, representing the amount of time you have to reach the office. To commute to the office, you must take `n` trains in sequential order. You are also given an integer array `dist` of length `n`, where `dist[i]` describes the distance (in kilometers) of the `i ^ th` train ride.

Each train can only depart at an integer hour, so you may need to wait in between each train ride.

- For example, if the `1 ^ st` train ride takes `1.5` hours, you must wait for an additional `0.5` hours before you can depart on the `2 ^ nd` train ride at the 2 hour mark.

Return _the __minimum positive integer__ speed __(in kilometers per hour)__ that all the trains must travel at for you to reach the office on time, or_ `-1` _if it is impossible to be on time_.

Tests are generated such that the answer will not exceed `10 ^ 7` and `hour` will have __at most two digits after the decimal point__.

__Example:__

Example 1:

```text
Input: dist = [1,3,2], hour = 6
Output: 1
Explanation: At speed 1:
```

- The first train ride takes 1/1 = 1 hour.
- Since we are already at an integer hour, we depart immediately at the 1 hour mark. The second train takes 3/1 = 3 hours.
- Since we are already at an integer hour, we depart immediately at the 4 hour mark. The third train takes 2/1 = 2 hours.
- You will arrive at exactly the 6 hour mark.

Example 2:

```text
Input: dist = [1,3,2], hour = 2.7
Output: 3
Explanation: At speed 3:
```

- The first train ride takes 1/3 = 0.33333 hours.
- Since we are not at an integer hour, we wait until the 1 hour mark to depart. The second train ride takes 3/3 = 1 hour.
- Since we are already at an integer hour, we depart immediately at the 2 hour mark. The third train takes 2/3 = 0.66667 hours.
- You will arrive at the 2.66667 hour mark.

Example 3:

```text
Input: dist = [1,3,2], hour = 1.9
Output: -1
Explanation: It is impossible because the earliest the third train can depart is at the 2 hour mark.
```

__Constraints:__

- `n == dist.length`
- `1 <= n <= 10 ^ 5`
- `1 <= dist[i] <= 10 ^ 5`
- `1 <= hour <= 10 ^ 9`
- There will be at most two digits after the decimal point in `hour`.

__题目描述:__

给你一个浮点数 `hour` ，表示你到达办公室可用的总通勤时间。要到达办公室，你必须按给定次序乘坐 `n` 趟列车。另给你一个长度为 `n` 的整数数组 `dist` ，其中 `dist[i]` 表示第 `i` 趟列车的行驶距离（单位是千米）。

每趟列车均只能在整点发车，所以你可能需要在两趟列车之间等待一段时间。

- 例如，第 `1` 趟列车需要 `1.5` 小时，那你必须再等待 `0.5` 小时，搭乘在第 2 小时发车的第 `2` 趟列车。

返回能满足你准时到达办公室所要求全部列车的 __最小正整数__ 时速（单位:千米每小时），如果无法准时到达，则返回 `-1` 。

生成的测试用例保证答案不超过 `10 ^ 7` ，且 `hour` 的 __小数点后最多存在两位数字__ 。

__示例:__

示例 1：

```text
输入：dist = [1,3,2], hour = 6
输出：1
解释：速度为 1 时：
```

- 第 1 趟列车运行需要 1/1 = 1 小时。
- 由于是在整数时间到达，可以立即换乘在第 1 小时发车的列车。第 2 趟列车运行需要 3/1 = 3 小时。
- 由于是在整数时间到达，可以立即换乘在第 4 小时发车的列车。第 3 趟列车运行需要 2/1 = 2 小时。
- 你将会恰好在第 6 小时到达。

示例 2：

```text
输入：dist = [1,3,2], hour = 2.7
输出：3
解释：速度为 3 时：
```

- 第 1 趟列车运行需要 1/3 = 0.33333 小时。
- 由于不是在整数时间到达，故需要等待至第 1 小时才能搭乘列车。第 2 趟列车运行需要 3/3 = 1 小时。
- 由于是在整数时间到达，可以立即换乘在第 2 小时发车的列车。第 3 趟列车运行需要 2/3 = 0.66667 小时。
- 你将会在第 2.66667 小时到达。

示例 3：

```text
输入：dist = [1,3,2], hour = 1.9
输出：-1
解释：不可能准时到达，因为第 3 趟列车最早是在第 2 小时发车。
```

__提示：__

- `n == dist.length`
- `1 <= n <= 10 ^ 5`
- `1 <= dist[i] <= 10 ^ 5`
- `1 <= hour <= 10 ^ 9`
- `hours` 中，小数点后最多存在两位数字

__思路:__

```text
二分查找
题目中已经明确查找上限为 1e7
将 hour 转化为整数
如果 hour 比 n - 1 还小必然不能到达
否则计算某个速度是否能够到达
除了最后一段前面的都是 floor(dist[i] / speed)
最后一段先乘上速度再加上时间再与总路程比较
如果能 right 更新为 mid
否则 left 更新为 mid + 1
时间复杂度为 O(NlogC), 空间复杂度为 O(1), C 为查找区间 [1, 1e7]
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minSpeedOnTime(vector<int>& dist, double hour) 
    {
        int n = dist.size(), left = 1, right = 1e7;
        long long hr = llround(hour * 100);
        if (hr <= (n - 1) * 100) return -1;
        while (left < right)
        {
            int mid = left + ((right - left) >> 1);
            long long t = 0;
            for (int i = 0; i < n - 1; i++) t += (dist[i] - 1) / mid + 1;
            t *= mid;
            t += dist.back();
            if (t * 100 <= hr * mid) right = mid;
            else left = mid + 1;
        }
        return left;
    }
};
```

__Java__:

```Java
class Solution {
    public int minSpeedOnTime(int[] dist, double hour) {
        int left = 1, right = Arrays.stream(dist).max().getAsInt() * 100;
        while (left < right) {
            int mid = left + ((right - left) >>> 1);
            if (check(dist, mid) <= hour) right = mid;
            else left = mid + 1;
        }
        return check(dist, left) <= hour ? left : -1;
    }

    public double check(int[] dist, int speed) {
        double result = 0;
        for (int i = 0, n = dist.length; i < n - 1; i++) result += (dist[i] + speed - 1) / speed;
        return result + (double)dist[dist.length - 1] / speed;
    }
}
```

__Python__:

```Python
class Solution:
    def minSpeedOnTime(self, dist: List[int], hour: float) -> int:
        if (hour := round(hour * 100)) <= 100 * ((n := len(dist)) - 1):
            return -1
        left, right, f = 1, 10 ** 7, lambda speed: (reduce(lambda x, y: x + (y - 1) // speed + 1, [0] + dist[:-1]) * speed + dist[-1]) * 100 <= hour * speed
        while left < right:
            mid = left + ((right - left) >> 1)
            if f(mid):
                right = mid
            else:
                left = mid + 1
        return left
```
