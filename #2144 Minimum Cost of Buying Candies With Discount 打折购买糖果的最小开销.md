# 2144 Minimum Cost of Buying Candies With Discount 打折购买糖果的最小开销

__Description:__

A shop is selling candies at a discount. For every two candies sold, the shop gives a third candy for free.

The customer can choose any candy to take away for free as long as the cost of the chosen candy is less than or equal to the minimum cost of the two candies bought.

- For example, if there are `4` candies with costs `1`, `2`, `3`, and `4`, and the customer buys candies with costs `2` and `3`, they can take the candy with cost `1` for free, but not the candy with cost `4`.

Given a __0-indexed__ integer array `cost`, where `cost[i]` denotes the cost of the `i ^ th` candy, return _the __minimum cost__ of buying __all__ the candies_.

__Example:__

Example 1:

```text
Input: cost = [1,2,3]
Output: 5
Explanation: We buy the candies with costs 2 and 3, and take the candy with cost 1 for free.
The total cost of buying all candies is 2 + 3 = 5. This is the only way we can buy the candies.
Note that we cannot buy candies with costs 1 and 3, and then take the candy with cost 2 for free.
The cost of the free candy has to be less than or equal to the minimum cost of the purchased candies.
```

Example 2:

```text
Input: cost = [6,5,7,9,2,2]
Output: 23
Explanation: The way in which we can get the minimum cost is described below:
```

- Buy candies with costs 9 and 7
- Take the candy with cost 6 for free
- We buy candies with costs 5 and 2
- Take the last remaining candy with cost 2 for free

Hence, the minimum cost to buy all candies is 9 + 7 + 5 + 2 = 23.

Example 3:

```text
Input: cost = [5,5]
Output: 10
Explanation: Since there are only 2 candies, we buy both of them. There is not a third candy we can take for free.
Hence, the minimum cost to buy all candies is 5 + 5 = 10.
```

__Constraints:__

- `1 <= cost.length <= 100`
- `1 <= cost[i] <= 100`

__题目描述:__

一家商店正在打折销售糖果。每购买 两个 糖果，商店会 免费 送一个糖果。

免费送的糖果唯一的限制是：它的价格需要小于等于购买的两个糖果价格的 较小值 。

- 比方说，总共有 `4` 个糖果，价格分别为 `1` ， `2` ， `3` 和 `4` ，一位顾客买了价格为 `2` 和 `3` 的糖果，那么他可以免费获得价格为 `1` 的糖果，但不能获得价格为 `4` 的糖果。

给你一个下标从 __0__ 开始的整数数组 `cost` ，其中 `cost[i]` 表示第 `i` 个糖果的价格，请你返回获得 __所有__ 糖果的 __最小__ 总开销。

__示例:__

示例 1：

```text
输入：cost = [1,2,3]
输出：5
解释：我们购买价格为 2 和 3 的糖果，然后免费获得价格为 1 的糖果。
总开销为 2 + 3 = 5 。这是开销最小的 唯一 方案。
注意，我们不能购买价格为 1 和 3 的糖果，并免费获得价格为 2 的糖果。
这是因为免费糖果的价格必须小于等于购买的 2 个糖果价格的较小值。
```

示例 2：

```text
输入：cost = [6,5,7,9,2,2]
输出：23
解释：最小总开销购买糖果方案为：
```

- 购买价格为 9 和 7 的糖果
- 免费获得价格为 6 的糖果
- 购买价格为 5 和 2 的糖果
- 免费获得价格为 2 的最后一个糖果

因此，最小总开销为 9 + 7 + 5 + 2 = 23 。

示例 3：

```text
输入：cost = [5,5]
输出：10
解释：由于只有 2 个糖果，我们需要将它们都购买，而且没有免费糖果。
所以总最小开销为 5 + 5 = 10 。
```

__提示：__

- `1 <= cost.length <= 100`
- `1 <= cost[i] <= 100`

__思路:__

```text
排序
从大到小排序
每次买 2 个贵的就送一个便宜的
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumCost(vector<int>& cost) 
    {
        sort(cost.begin(), cost.end());
        int result = 0, n = cost.size();
        for (int i = n - 1; i > -1; i--) result += ((n - i) % 3 ? cost[i] : 0);
        return result; 
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumCost(int[] cost) {
        Arrays.sort(cost);
        int result = 0, n = cost.length;
        for (int i = n - 1; i > -1; i--) result += ((n - i) % 3 != 0 ? cost[i] : 0);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumCost(self, cost: List[int]) -> int:
        return sum([j for i, j in enumerate(sorted(cost, reverse=True)) if (i + 1) % 3])
```
