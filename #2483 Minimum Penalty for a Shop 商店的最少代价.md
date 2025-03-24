# 2483 Minimum Penalty for a Shop 商店的最少代价

__Description:__

You are given the customer visit log of a shop represented by a __0-indexed__ string `customers` consisting only of characters `'N'` and `'Y'`:

- if the `i ^ th` character is `'Y'`, it means that customers come at the `i ^ th` hour
- whereas `'N'` indicates that no customers come at the `i ^ th` hour.

If the shop closes at the `j ^ th` hour ( `0 <= j <= n`), the __penalty__ is calculated as follows:

- For every hour when the shop is open and no customers come, the penalty increases by `1`.
- For every hour when the shop is closed and customers come, the penalty increases by `1`.

Return the earliest hour at which the shop must be closed to incur a minimum penalty.

__Note__ that if a shop closes at the `j ^ th` hour, it means the shop is closed at the hour `j`.

__Example:__

Example 1:

```text
Input: customers = "YYNY"
Output: 2
Explanation: 
```

- Closing the shop at the 0th hour incurs in 1+1+0+1 = 3 penalty.
- Closing the shop at the 1st hour incurs in 0+1+0+1 = 2 penalty.
- Closing the shop at the 2nd hour incurs in 0+0+0+1 = 1 penalty.
- Closing the shop at the 3rd hour incurs in 0+0+1+1 = 2 penalty.
- Closing the shop at the 4th hour incurs in 0+0+1+0 = 1 penalty.

Closing the shop at 2nd or 4th hour gives a minimum penalty. Since 2 is earlier, the optimal closing time is 2.

Example 2:

```text
Input: customers = "NNNNN"
Output: 0
Explanation: It is best to close the shop at the 0th hour as no customers arrive.
```

Example 3:

```text
Input: customers = "YYYY"
Output: 4
Explanation: It is best to close the shop at the 4th hour as customers arrive at each hour.
```

__Constraints:__

- `1 <= customers.length <= 10 ^ 5`
- `customers` consists only of characters `'Y'` and `'N'`.

__题目描述:__

给你一个顾客访问商店的日志，用一个下标从 __0__ 开始且只包含字符 `'N'` 和 `'Y'` 的字符串 `customers` 表示:

- 如果第 `i` 个字符是 `'Y'` ，它表示第 `i` 小时有顾客到达。
- 如果第 `i` 个字符是 `'N'` ，它表示第 `i` 小时没有顾客到达。

如果商店在第 `j` 小时关门（ `0 <= j <= n`），代价按如下方式计算:

- 在开门期间，如果某一个小时没有顾客到达，代价增加 `1` 。
- 在关门期间，如果某一个小时有顾客到达，代价增加 `1` 。

请你返回在确保代价 最小 的前提下，商店的 最早 关门时间。

注意，商店在第 `j` 小时关门表示在第 `j` 小时以及之后商店处于关门状态。

__示例:__

示例 1：

```text
输入：customers = "YYNY"
输出：2
解释：
```

- 第 0 小时关门，总共 1+1+0+1 = 3 代价。
- 第 1 小时关门，总共 0+1+0+1 = 2 代价。
- 第 2 小时关门，总共 0+0+0+1 = 1 代价。
- 第 3 小时关门，总共 0+0+1+1 = 2 代价。
- 第 4 小时关门，总共 0+0+1+0 = 1 代价。

在第 2 或第 4 小时关门代价都最小。由于第 2 小时更早，所以最优关门时间是 2 。

示例 2：

```text
输入：customers = "NNNNN"
输出：0
解释：最优关门时间是 0 ，因为自始至终没有顾客到达。
```

示例 3：

```text
输入：customers = "YYYY"
输出：4
解释：最优关门时间是 4 ，因为每一小时均有顾客到达。
```

__提示：__

- `1 <= customers.length <= 10 ^ 5`
- `customers` 只包含字符 `'Y'` 和 `'N'` 。

__思路:__

```text
枚举
假设 0 时刻关门
那么代价就是 'Y' 的个数
如果 1 时刻是 'Y'
那么代价就要减 1
如果 1 时刻是 'N'
那么代价就要加 1
依次类推
找到最小的代价, 记录当前的时刻 i + 1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int bestClosingTime(string customers) 
    {
        int result = 0, min_cost = count(customers.begin(), customers.end(), 'Y'), cur = min_cost, n = customers.size();
        for (int i = 0; i < n; i++) 
        {
            if (customers[i] == 'N') ++cur;
            else 
            {
                --cur;
                if (cur < min_cost) 
                {
                    min_cost = cur;
                    result = i + 1;
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int bestClosingTime(String customers) {
        int result = 0, minCost = (int)customers.chars().filter(c -> c == 'Y').count(), cur = minCost, n = customers.length();
        for (int i = 0; i < n; i++) {
            if (customers.charAt(i) == 'N') ++cur;
            else {
                --cur;
                if (cur < minCost) {
                    minCost = cur;
                    result = i + 1;
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def bestClosingTime(self, customers: str) -> int:
        result, min_cost, cur = 0, customers.count('Y'), customers.count('Y')
        for i, c in enumerate(customers, 1):
            if c == 'N': 
                cur += 1
            else:
                cur -= 1
                if cur < min_cost:
                    min_cost = cur
                    result = i
        return result
```
