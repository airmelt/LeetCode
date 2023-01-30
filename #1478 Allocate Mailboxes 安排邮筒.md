# 1478 Allocate Mailboxes 安排邮筒

__Description:__

Given the array `houses` where `houses[i]` is the location of the `i ^ th` house along a street and an integer `k`, allocate `k` mailboxes in the street.

Return the minimum total distance between each house and its nearest mailbox.

The test cases are generated so that the answer fits in a 32-bit integer.

__Example:__

Example 1:

![1478-1](https://assets.leetcode.com/uploads/2020/05/07/sample_11_1816.png)

```text
Input: houses = [1,4,8,10,20], k = 3
Output: 5
Explanation: Allocate mailboxes in position 3, 9 and 20.
Minimum total distance from each houses to nearest mailboxes is |3-1| + |4-3| + |9-8| + |10-9| + |20-20| = 5
```

Example 2:

![1478-2](https://assets.leetcode.com/uploads/2020/05/07/sample_2_1816.png)

```text
Input: houses = [2,3,5,12,18], k = 2
Output: 9
Explanation: Allocate mailboxes in position 3 and 14.
Minimum total distance from each houses to nearest mailboxes is |2-3| + |3-3| + |5-3| + |12-14| + |18-14| = 9.
```

__Constraints:__

- `1 <= k <= houses.length <= 100`
- `1 <= houses[i] <= 10 ^ 4`
- All the integers of `houses` are __unique__.

__题目描述:__

给你一个房屋数组 `houses` 和一个整数 `k` ，其中 `houses[i]` 是第 `i` 栋房子在一条街上的位置，现需要在这条街上安排 `k` 个邮筒。

请你返回每栋房子与离它最近的邮筒之间的距离的 最小 总和。

答案保证在 32 位有符号整数范围以内。

__示例:__

示例 1：

![1478-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/06/13/sample_11_1816.png)

```text
输入：houses = [1,4,8,10,20], k = 3
输出：5
解释：将邮筒分别安放在位置 3， 9 和 20 处。
每个房子到最近邮筒的距离和为 |3-1| + |4-3| + |9-8| + |10-9| + |20-20| = 5 。
```

示例 2：

![1478-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/06/13/sample_2_1816.png)

```text
输入：houses = [2,3,5,12,18], k = 2
输出：9
解释：将邮筒分别安放在位置 3 和 14 处。
每个房子到最近邮筒距离和为 |2-3| + |3-3| + |5-3| + |12-14| + |18-14| = 9 。
```

示例 3：

```text
输入：houses = [7,4,6,1], k = 1
输出：8
```

示例 4：

```text
输入：houses = [3,6,14,10], k = 4
输出：0
```

__提示：__

- `n == houses.length`
- `1 <= n <= 100`
- `1 <= houses[i] <= 10 ^ 4`
- `1 <= k <= n`
- 数组 `houses` 中的整数互不相同。

__思路:__

```text
动态规划
考虑只有一个邮筒的情况, 根据绝对值的性质很容易得到在中位数处能取到最小值(偶数个数的时候任意选择一个)
那么先将房子排序, 就可以转化为在房子数组中选择子数组放入邮筒求距离最小值
设 dp[i][j] 表示邮筒 j 对应房子 i 时能够取得的距离最小值
dp[i][j] = min(dp[i0][j - 1] + cost[i0 + 1][i]), 其中 i0 的取值范围为 [0, i)
边界条件 dp[0][j] = cost[0][j]
这里 cost[i][j] 表示 house[i] 到 house[j] 中位数的距离
可以用 cost[i][j] = cost[i + 1][j - 1] + houses[j] - houses[i] 计算
也可以用 cost[i][j] = abs(cost[i] - cost[i - 1]) + ... + abs(cost[j - 1], cost[j]) 计算
注意这里不需要计算邮筒数量比房子数量多的情况也就是说 j > i 的情况, 因为如果邮筒的数量大于等于房子的数量, 只要把邮筒和房子一一对应就能取到最小值 0
最后返回 dp[n - 1][k] 即可
时间复杂度为 O(KN ^ 2), 空间复杂度为 O(N ^ 2 + NK)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minDistance(vector<int>& houses, int k) 
    {
        int n = houses.size(), INF = 0x3f3f3f3f;
        sort(houses.begin(), houses.end());
        vector<vector<int>> cost(n, vector<int>(n)), dp(n, vector<int>(k + 1, INF));
        for (int i = n - 2; i > -1; i--) for (int j = i + 1; j < n; j++) cost[i][j] = cost[i + 1][j - 1] + houses[j] - houses[i];
        for (int i = 0; i < n; i++) {
            dp[i][1] = cost[0][i];
            for (int j = 2; j <= k and j <= i + 1; j++) for (int i0 = 0; i0 < i; i0++) dp[i][j] = min(dp[i][j], dp[i0][j - 1] + cost[i0 + 1][i]);
        }
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minDistance(int[] houses, int k) {
        int n = houses.length, cost[][] = new int[n][n], dp[][] = new int[n][k + 1], INF = 0x3f3f3f3f;
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], INF);
        Arrays.sort(houses);
        for (int i = n - 2; i > -1; i--) for (int j = i + 1; j < n; j++) cost[i][j] = cost[i + 1][j - 1] + houses[j] - houses[i];
        for (int i = 0; i < n; i++) {
            dp[i][1] = cost[0][i];
            for (int j = 2; j <= k && j <= i + 1; j++) for (int i0 = 0; i0 < i; i0++) dp[i][j] = Math.min(dp[i][j], dp[i0][j - 1] + cost[i0 + 1][i]);
        }
        return dp[n - 1][k];
    }
}
```

__Python__:

```Python
class Solution:
    def minDistance(self, houses: List[int], k: int) -> int:
        dp = [[float('inf')] * (n := len(houses)) for _ in range(k)]
        houses.sort()
        
        @lru_cache(None)
        def cost(start: int, end: int) -> int:
            result = 0
            while start < end:
                result += houses[end] - houses[start]
                start += 1
                end -= 1
            return result
        for i in range(k):
            for j in range(i, n - (k - i - 1)):
                for mid in range(i - 1, j):
                    dp[i][j] = min(dp[i][j], dp[i - 1][mid] + cost(mid + 1, j)) if i else cost(0, j)
        return dp[-1][-1]
```
