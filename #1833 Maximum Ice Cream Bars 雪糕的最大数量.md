# 1833 Maximum Ice Cream Bars 雪糕的最大数量

__Description:__

It is a sweltering summer day, and a boy wants to buy some ice cream bars.

At the store, there are `n` ice cream bars. You are given an array `costs` of length `n`, where `costs[i]` is the price of the `i ^ th` ice cream bar in coins. The boy initially has `coins` coins to spend, and he wants to buy as many ice cream bars as possible.

Note: The boy can buy the ice cream bars in any order.

Return _the __maximum__ number of ice cream bars the boy can buy with_ `coins` _coins._

You must solve the problem by counting sort.

__Example:__

Example 1:

```text
Input: costs = [1,3,2,4,1], coins = 7
Output: 4
Explanation: The boy can buy ice cream bars at indices 0,1,2,4 for a total price of 1 + 3 + 2 + 1 = 7.
```

Example 2:

```text
Input: costs = [10,6,8,7,7,8], coins = 5
Output: 0
Explanation: The boy cannot afford any of the ice cream bars.
```

Example 3:

```text
Input: costs = [1,6,3,1,2,5], coins = 20
Output: 6
Explanation: The boy can buy all the ice cream bars for a total price of 1 + 6 + 3 + 1 + 2 + 5 = 18.
```

__Constraints:__

- `costs.length == n`
- `1 <= n <= 10 ^ 5`
- `1 <= costs[i] <= 10 ^ 5`
- `1 <= coins <= 10 ^ 8`

__题目描述:__

夏日炎炎，小男孩 Tony 想买一些雪糕消消暑。

商店中新到 `n` 支雪糕，用长度为 `n` 的数组 `costs` 表示雪糕的定价，其中 `costs[i]` 表示第 `i` 支雪糕的现金价格。Tony 一共有 `coins` 现金可以用于消费，他想要买尽可能多的雪糕。

注意：Tony 可以按任意顺序购买雪糕。

给你价格数组 `costs` 和现金量 `coins` ，请你计算并返回 Tony 用 `coins` 现金能够买到的雪糕的 __最大数量__ 。

你必须使用计数排序解决此问题。

__示例:__

示例 1：

```text
输入：costs = [1,3,2,4,1], coins = 7
输出：4
解释：Tony 可以买下标为 0、1、2、4 的雪糕，总价为 1 + 3 + 2 + 1 = 7
```

示例 2：

```text
输入：costs = [10,6,8,7,7,8], coins = 5
输出：0
解释：Tony 没有足够的钱买任何一支雪糕。
```

示例 3：

```text
输入：costs = [1,6,3,1,2,5], coins = 20
输出：6
解释：Tony 可以买下所有的雪糕，总价为 1 + 6 + 3 + 1 + 2 + 5 = 18 。
```

__提示：__

- `costs.length == n`
- `1 <= n <= 10 ^ 5`
- `1 <= costs[i] <= 10 ^ 5`
- `1 <= coins <= 10 ^ 8`

__思路:__

```text
排序 ➕ 贪心
使用计数排序或者直接将整个数组排序
贪心的购买当前最便宜的雪糕
时间复杂度为 O(NlogN), 空间复杂度为 O(lgN)
计数排序时间复杂度为 O(N + M), 空间复杂度为 O(M), M 为数组的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxIceCream(vector<int>& costs, int coins) 
    {
        int m = *max_element(costs.begin(), costs.end()), result = 0, cur = 0;
        vector<int> count(m + 1);
        for (const auto &cost : costs) ++count[cost];
        for (int i = 1; i <= m; i++)
        {
            if (coins >= i)
            {
                result += (cur = min(count[i], coins / i));
                coins -= i * cur;
            }
            else break;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxIceCream(int[] costs, int coins) {
        Arrays.sort(costs);
        int result = 0;
        for (int cost : costs) {
            coins -= cost;
            if (coins > -1) ++result;
            else break;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxIceCream(self, costs: List[int], coins: int) -> int:
        return bisect_right(list(accumulate(sorted(costs))), coins)
```
