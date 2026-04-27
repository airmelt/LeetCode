# 2110 Number of Smooth Descent Periods of a Stock 股票平滑下跌阶段的数目

__Description:__

You are given an integer array `prices` representing the daily price history of a stock, where `prices[i]` is the stock price on the `i ^ th` day.

A __smooth descent period__ of a stock consists of __one or more contiguous__ days such that the price on each day is __lower__ than the price on the __preceding day__ by __exactly__ `1`. The first day of the period is exempted from this rule.

Return the number of smooth descent periods.

__Example:__

Example 1:

```text
Input: prices = [3,2,1,4]
Output: 7
Explanation: There are 7 smooth descent periods:
[3], [2], [1], [4], [3,2], [2,1], and [3,2,1]
Note that a period with one day is a smooth descent period by the definition.
```

Example 2:

```text
Input: prices = [8,6,7,7]
Output: 4
Explanation: There are 4 smooth descent periods: [8], [6], [7], and [7]
Note that [8,6] is not a smooth descent period as 8 - 6 ≠ 1.
```

Example 3:

```text
Input: prices = [1]
Output: 1
Explanation: There is 1 smooth descent period: [1]
```

__Constraints:__

- `1 <= prices.length <= 10 ^ 5`
- `1 <= prices[i] <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `prices` ，表示一支股票的历史每日股价，其中 `prices[i]` 是这支股票第 `i` 天的价格。

一个 __平滑下降的阶段__ 定义为:对于 __连续一天或者多天__ ，每日股价都比 __前一日股价恰好少__ `1` ，这个阶段第一天的股价没有限制。

请你返回 平滑下降阶段 的数目。

__示例:__

示例 1：

```text
输入：prices = [3,2,1,4]
输出：7
解释：总共有 7 个平滑下降阶段：
[3], [2], [1], [4], [3,2], [2,1] 和 [3,2,1]
注意，仅一天按照定义也是平滑下降阶段。
```

示例 2：

```text
输入：prices = [8,6,7,7]
输出：4
解释：总共有 4 个连续平滑下降阶段：[8], [6], [7] 和 [7]
由于 8 - 6 ≠ 1 ，所以 [8,6] 不是平滑下降阶段。
```

示例 3：

```text
输入：prices = [1]
输出：1
解释：总共有 1 个平滑下降阶段：[1]
```

__提示：__

- `1 <= prices.length <= 10 ^ 5`
- `1 <= prices[i] <= 10 ^ 5`

__思路:__

```text
模拟
统计每一段下降的天数 cur
累加 cur * (cur + 1) / 2
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long getDescentPeriods(vector<int>& prices) 
    {
        long long result = 0, cur = 1;
        for (int i = 1, n = prices.size(); i < n; i++) 
        {
            if (prices[i] - prices[i - 1] == -1) ++cur;
            else 
            {
                result += (cur + 1L) * cur >> 1;
                cur = 1;
            }
        }
        return result + ((cur + 1) * cur >> 1);
    }
};
```

__Java__:

```Java
class Solution {
    public long getDescentPeriods(int[] prices) {
        long result = 0, cur = 1;
        for (int i = 1, n = prices.length; i < n; i++) {
            if (prices[i] - prices[i - 1] == -1) ++cur;
            else {
                result += (cur + 1) * cur >>> 1;
                cur = 1;
            }
        }
        return result + ((cur + 1) * cur >>> 1);
    }
}
```

__Python__:

```Python
class Solution:
    def getDescentPeriods(self, prices: List[int]) -> int:
        result, cur, n = 0, 1, len(prices)
        for i in range(1, n):
            if prices[i] - prices[i - 1] == -1:
                cur += 1
            else:
                result += (cur + 1) * cur >> 1
                cur = 1
        return result + ((cur + 1) * cur >> 1)
```
