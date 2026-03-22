# 1801 Number of Orders in the Backlog 积压订单中的订单总数

__Description:__

You are given a 2D integer array `orders`, where each `orders[i] = [pricei, amounti, orderTypei]` denotes that `amounti` orders have been placed of type `orderTypei` at the price `pricei`. The `orderTypei` is:

- `0` if it is a batch of `buy` orders, or
- `1` if it is a batch of `sell` orders.

Note that `orders[i]` represents a batch of `amounti` independent orders with the same price and order type. All orders represented by `orders[i]` will be placed before all orders represented by `orders[i+1]` for all valid `i`.

There is a backlog that consists of orders that have not been executed. The backlog is initially empty. When an order is placed, the following happens:

- If the order is a `buy` order, you look at the `sell` order with the __smallest__ price in the backlog. If that `sell` order's price is __smaller than or equal to__ the current `buy` order's price, they will match and be executed, and that `sell` order will be removed from the backlog. Else, the `buy` order is added to the backlog.
- Vice versa, if the order is a `sell` order, you look at the `buy` order with the __largest__ price in the backlog. If that `buy` order's price is __larger than or equal to__ the current `sell` order's price, they will match and be executed, and that `buy` order will be removed from the backlog. Else, the `sell` order is added to the backlog.

Return _the total __amount__ of orders in the backlog after placing all the orders from the input_. Since this number can be large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

![1801-1](https://assets.leetcode.com/uploads/2021/03/11/ex1.png)

```text
Input: orders = [[10,5,0],[15,2,1],[25,1,1],[30,4,0]]
Output: 6
Explanation: Here is what happens with the orders:
- 5 orders of type buy with price 10 are placed. There are no sell orders, so the 5 orders are added to the backlog.
- 2 orders of type sell with price 15 are placed. There are no buy orders with prices larger than or equal to 15, so the 2 orders are added to the backlog.
- 1 order of type sell with price 25 is placed. There are no buy orders with prices larger than or equal to 25 in the backlog, so this order is added to the backlog.
- 4 orders of type buy with price 30 are placed. The first 2 orders are matched with the 2 sell orders of the least price, which is 15 and these 2 sell orders are removed from the backlog. The 3rd order is matched with the sell order of the least price, which is 25 and this sell order is removed from the backlog. Then, there are no more sell orders in the backlog, so the 4th order is added to the backlog.
Finally, the backlog has 5 buy orders with price 10, and 1 buy order with price 30. So the total number of orders in the backlog is 6.
```

Example 2:

![1801-2](https://assets.leetcode.com/uploads/2021/03/11/ex2.png)

```text
Input: orders = [[7,1000000000,1],[15,3,0],[5,999999995,0],[5,1,1]]
Output: 999999984
Explanation: Here is what happens with the orders:
- 109 orders of type sell with price 7 are placed. There are no buy orders, so the 109 orders are added to the backlog.
- 3 orders of type buy with price 15 are placed. They are matched with the 3 sell orders with the least price which is 7, and those 3 sell orders are removed from the backlog.
- 999999995 orders of type buy with price 5 are placed. The least price of a sell order is 7, so the 999999995 orders are added to the backlog.
- 1 order of type sell with price 5 is placed. It is matched with the buy order of the highest price, which is 5, and that buy order is removed from the backlog.
Finally, the backlog has (1000000000-3) sell orders with price 7, and (999999995-1) buy orders with price 5. So the total number of orders = 1999999991, which is equal to 999999984 % (10 ^ 9 + 7).
```

__Constraints:__

- `1 <= orders.length <= 10 ^ 5`
- `orders[i].length == 3`
- `1 <= pricei, amounti <= 10 ^ 9`
- `orderTypei` is either `0` or `1`.

__题目描述:__

给你一个二维整数数组 `orders` ，其中每个 `orders[i] = [pricei, amounti, orderTypei]` 表示有 `amounti` 笔类型为 `orderTypei` 、价格为 `pricei` 的订单。

订单类型 `orderTypei` 可以分为两种:

- `0` 表示这是一批采购订单 `buy`
- `1` 表示这是一批销售订单 `sell`

注意， `orders[i]` 表示一批共计 `amounti` 笔的独立订单，这些订单的价格和类型相同。对于所有有效的 `i` ，由 `orders[i]` 表示的所有订单提交时间均早于 `orders[i+1]` 表示的所有订单。

存在由未执行订单组成的 积压订单 。积压订单最初是空的。提交订单时，会发生以下情况：

- 如果该订单是一笔采购订单 `buy` ，则可以查看积压订单中价格 __最低__ 的销售订单 `sell` 。如果该销售订单 `sell` 的价格 __低于或等于__ 当前采购订单 `buy` 的价格，则匹配并执行这两笔订单，并将销售订单 `sell` 从积压订单中删除。否则，采购订单 `buy` 将会添加到积压订单中。
- 反之亦然，如果该订单是一笔销售订单 `sell` ，则可以查看积压订单中价格 __最高__ 的采购订单 `buy` 。如果该采购订单 `buy` 的价格 __高于或等于__ 当前销售订单 `sell` 的价格，则匹配并执行这两笔订单，并将采购订单 `buy` 从积压订单中删除。否则，销售订单 `sell` 将会添加到积压订单中。

输入所有订单后，返回积压订单中的 __订单总数__ 。由于数字可能很大，所以需要返回对 `10 ^ 9 + 7` 取余的结果。

__示例:__

示例 1：

![1801-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/03/21/ex1.png)

```text
输入：orders = [[10,5,0],[15,2,1],[25,1,1],[30,4,0]]
输出：6
解释：输入订单后会发生下述情况：
- 提交 5 笔采购订单，价格为 10 。没有销售订单，所以这 5 笔订单添加到积压订单中。
- 提交 2 笔销售订单，价格为 15 。没有采购订单的价格大于或等于 15 ，所以这 2 笔订单添加到积压订单中。
- 提交 1 笔销售订单，价格为 25 。没有采购订单的价格大于或等于 25 ，所以这 1 笔订单添加到积压订单中。
- 提交 4 笔采购订单，价格为 30 。前 2 笔采购订单与价格最低（价格为 15）的 2 笔销售订单匹配，从积压订单中删除这 2 笔销售订单。第 3 笔采购订单与价格最低的 1 笔销售订单匹配，销售订单价格为 25 ，从积压订单中删除这 1 笔销售订单。积压订单中不存在更多销售订单，所以第 4 笔采购订单需要添加到积压订单中。
最终，积压订单中有 5 笔价格为 10 的采购订单，和 1 笔价格为 30 的采购订单。所以积压订单中的订单总数为 6 。
```

示例 2：

![1801-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/03/21/ex2.png)

```text
输入：orders = [[7,1000000000,1],[15,3,0],[5,999999995,0],[5,1,1]]
输出：999999984
解释：输入订单后会发生下述情况：
- 提交 109 笔销售订单，价格为 7 。没有采购订单，所以这 109 笔订单添加到积压订单中。
- 提交 3 笔采购订单，价格为 15 。这些采购订单与价格最低（价格为 7 ）的 3 笔销售订单匹配，从积压订单中删除这 3 笔销售订单。
- 提交 999999995 笔采购订单，价格为 5 。销售订单的最低价为 7 ，所以这 999999995 笔订单添加到积压订单中。
- 提交 1 笔销售订单，价格为 5 。这笔销售订单与价格最高（价格为 5 ）的 1 笔采购订单匹配，从积压订单中删除这 1 笔采购订单。
最终，积压订单中有 (1000000000-3) 笔价格为 7 的销售订单，和 (999999995-1) 笔价格为 5 的采购订单。所以积压订单中的订单总数为 1999999991 ，等于 999999984 % (10 ^ 9 + 7) 。
```

__提示：__

- `1 <= orders.length <= 10 ^ 5`
- `orders[i].length == 3`
- `1 <= pricei, amounti <= 10 ^ 9`
- `orderTypei` 为 `0` 或 `1`

__思路:__

```text
优先队列
使用两个优先队列模拟
buy 为大根堆记录价格最高的采购订单
sell 为小根堆记录价格最低的销售订单
按照 order[2] 是否为 1 决定是哪一种类型的订单
如果是采购订单, 就从 sell 中取出最低价格的订单, 如果价格小于等于当前采购订单的价格, 就匹配并执行这两笔订单, 并减少当前订单的数量直到为 0，最后如果还剩余订单就加到 buy 队列中
最后遍历 buy 和 sell 队列, 计算订单总数
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
constexpr int MOD = 1e9 + 7;
class Solution 
{
public:
    int getNumberOfBacklogOrders(vector<vector<int>>& orders) 
    {
        priority_queue<pair<int, int>> buy;
        priority_queue sell{ greater<>{}, vector<pair<int, int>>{} };
        return accumulate(orders.begin(), orders.end(), 0, [&](int a, auto& v) 
        {
            auto func = [&](auto& q1, auto& q2, auto&& op) 
            {
                while (v[1] and !q1.empty() and op(q1.top().first, v[0])) 
                {
                    auto [price, num] = q1.top();
                    q1.pop();
                    auto cur = min(num, v[1]);
                    v[1] -= cur;
                    num -= cur;
                    a = (a + MOD - cur) % MOD;
                    if (num) q1.emplace(price, num);
                }
                if (v[1]) q2.emplace(v[0], v[1]);
                return (a + v[1]) % MOD;
            };
            return v[2] ? func(buy, sell, greater_equal<>{}) : func(sell, buy, less_equal<>{});
        });
    }
};
```

__Java__:

```Java
class Solution {
    public int getNumberOfBacklogOrders(int[][] orders) {
        PriorityQueue<int[]> buy = new PriorityQueue<>((a, b) -> b[0] - a[0]), sell = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        int MOD = 1_000_000_007, result = 0;
        for (int[] order : orders) {
            if (order[2] != 0) {
                while (!buy.isEmpty() && buy.peek()[0] >= order[0]) {
                    if (buy.peek()[1] >= order[1]) {
                        buy.peek()[1] -= order[1];
                        order[1] = 0;
                        break;
                    }
                    else order[1] -= buy.poll()[1];
                }
                if (order[1] != 0) sell.offer(order); 
            } else {
                while (!sell.isEmpty() && sell.peek()[0] <= order[0]) {
                    if (sell.peek()[1] >= order[1]) {
                        sell.peek()[1] -= order[1];
                        order[1] = 0;
                        break;
                    }
                    else order[1] -= sell.poll()[1];
                }
                if (order[1] != 0) buy.offer(order);
            }
        } 
        for (PriorityQueue<int[]> pq : Arrays.asList(buy, sell)) while (!pq.isEmpty()) result = (result + pq.poll()[1]) % MOD;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getNumberOfBacklogOrders(self, orders: List[List[int]]) -> int:
        buy, sell, MOD = [], [], (10 ** 9 + 7)
        for order in orders:
            if order[2]:
                while buy and -buy[0][0] >= order[0]:
                    if buy[0][1] >= order[1]:
                        buy[0][1] -= order[1]
                        order[1] = 0
                        break
                    else:
                        order[1] -= heapq.heappop(buy)[1]
                if order[1]:
                    heappush(sell, order)
            else:
                while sell and sell[0][0] <= order[0]:
                    if sell[0][1] >= order[1]:
                        sell[0][1] -= order[1]
                        order[1] = 0
                        break
                    else:
                        order[1] -= heappop(sell)[1]
                order[0] *= -1
                if order[1]:
                    heappush(buy, order)
        return (sum(b[1] for b in buy) + sum(s[1] for s in sell)) % MOD
```
