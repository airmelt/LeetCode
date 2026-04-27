# 1109 Corporate Flight Bookings 航班预订统计

__Description__:
There are n flights that are labeled from 1 to n.

You are given an array of flight bookings bookings, where bookings[i] = [firsti, lasti, seatsi] represents a booking for flights firsti through lasti (inclusive) with seatsi seats reserved for each flight in the range.

Return an array answer of length n, where answer[i] is the total number of seats reserved for flight i.

__Example:__

Example 1:

Input: bookings = [[1,2,10],[2,3,20],[2,5,25]], n = 5
Output: [10,55,45,25,25]
Explanation:

```text
Flight labels:        1   2   3   4   5
Booking 1 reserved:  10  10
Booking 2 reserved:      20  20
Booking 3 reserved:      25  25  25  25
Total seats:         10  55  45  25  25
```

Hence, answer = [10,55,45,25,25]

Example 2:

Input: bookings = [[1,2,10],[2,2,15]], n = 2
Output: [10,25]
Explanation:

```text
Flight labels:        1   2
Booking 1 reserved:  10  10
Booking 2 reserved:      15
Total seats:         10  25
```

Hence, answer = [10,25]

__Constraints:__

1 <= n <= 2 \* 10^4
1 <= bookings.length <= 2 \* 10^4
bookings[i].length == 3
1 <= firsti <= lasti <= n
1 <= seatsi <= 10^4

__题目描述__:
这里有 n 个航班，它们分别从 1 到 n 进行编号。

有一份航班预订表 bookings ，表中第 i 条预订记录 bookings[i] = [firsti, lasti, seatsi] 意味着在从 firsti 到 lasti （包含 firsti 和 lasti ）的 每个航班 上预订了 seatsi 个座位。

请你返回一个长度为 n 的数组 answer，里面的元素是每个航班预定的座位总数。

__示例 :__

示例 1：

输入：bookings = [[1,2,10],[2,3,20],[2,5,25]], n = 5
输出：[10,55,45,25,25]
解释：

```text
航班编号        1   2   3   4   5
预订记录 1 ：   10  10
预订记录 2 ：       20  20
预订记录 3 ：       25  25  25  25
总座位数：      10  55  45  25  25
```

因此，answer = [10,55,45,25,25]

示例 2：

输入：bookings = [[1,2,10],[2,2,15]], n = 2
输出：[10,25]
解释：

```text
航班编号        1   2
预订记录 1 ：   10  10
预订记录 2 ：       15
总座位数：      10  25
```

因此，answer = [10,25]

__提示:__

1 <= n <= 2 \* 10^4
1 <= bookings.length <= 2 \* 10^4
bookings[i].length == 3
1 <= firsti <= lasti <= n
1 <= seatsi <= 10^4

__思路__:

差分数组
diff[i] = result[i] - result[i - 1]
对每一个 booking
diff[booking[0]] += booking[2]
diff[booking[1] + 1] -= booking[2]
也就是区间端点进行处理
最后将 diff 求取前缀和即为结果
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> corpFlightBookings(vector<vector<int>>& bookings, int n) 
    {
        vector<int> result(n);
        for (const auto& booking : bookings) 
        {
            result[booking[0] - 1] += booking[2];
            if (booking[1] < n) result[booking[1]] -= booking[2];
        }
        for (int i = 1; i < n; i++) result[i] += result[i - 1];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] corpFlightBookings(int[][] bookings, int n) {
        int[] result = new int[n];
        for (int[] booking : bookings) {
            result[booking[0] - 1] += booking[2];
            if (booking[1] < n) result[booking[1]] -= booking[2];
        }
        for (int i = 1; i < n; i++) result[i] += result[i - 1];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def corpFlightBookings(self, bookings: List[List[int]], n: int) -> List[int]:
        result = [0] * n
        for i, j, v in bookings:
            result[i - 1] += v
            if j < n:
                result[j] -= v
        for i in range(1, n):
            result[i] += result[i - 1]
        return result
```
