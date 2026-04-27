# 1599 Maximum Profit of Operating a Centennial Wheel 经营摩天轮的最大利润

__Description:__

You are the operator of a Centennial Wheel that has __four gondolas__, and each gondola has room for __up__ __to__ __four people__. You have the ability to rotate the gondolas __counterclockwise__, which costs you `runningCost` dollars.

You are given an array `customers` of length `n` where `customers[i]` is the number of new customers arriving just before the `i ^ th` rotation (0-indexed). This means you __must rotate the wheel__ `i` __times before the__ `customers[i]` __customers arrive__. __You cannot make customers wait if there is room in the gondola__. Each customer pays `boardingCost` dollars when they board on the gondola closest to the ground and will exit once that gondola reaches the ground again.

You can stop the wheel at any time, including before serving all customers. If you decide to stop serving customers, all subsequent rotations are free in order to get all the customers down safely. Note that if there are currently more than four customers waiting at the wheel, only four will board the gondola, and the rest will wait for the next rotation.

Return _the minimum number of rotations you need to perform to maximize your profit._ If there is __no scenario__ where the profit is positive, return `-1`.

__Example:__

Example 1:

![1599-1](https://assets.leetcode.com/uploads/2020/09/09/wheeldiagram12.png)

```text
Input: customers = [8,3], boardingCost = 5, runningCost = 6
Output: 3
Explanation: The numbers written on the gondolas are the number of people currently there.
1. 8 customers arrive, 4 board and 4 wait for the next gondola, the wheel rotates. Current profit is 4 * $5 - 1 * $6 = $14.
2. 3 customers arrive, the 4 waiting board the wheel and the other 3 wait, the wheel rotates. Current profit is 8 * $5 - 2 * $6 = $28.
3. The final 3 customers board the gondola, the wheel rotates. Current profit is 11 * $5 - 3 * $6 = $37.
The highest profit was $37 after rotating the wheel 3 times.
```

Example 2:

```text
Input: customers = [10,9,6], boardingCost = 6, runningCost = 4
Output: 7
Explanation:
1. 10 customers arrive, 4 board and 6 wait for the next gondola, the wheel rotates. Current profit is 4 * $6 - 1 * $4 = $20.
2. 9 customers arrive, 4 board and 11 wait (2 originally waiting, 9 newly waiting), the wheel rotates. Current profit is 8 * $6 - 2 * $4 = $40.
3. The final 6 customers arrive, 4 board and 13 wait, the wheel rotates. Current profit is 12 * $6 - 3 * $4 = $60.
4. 4 board and 9 wait, the wheel rotates. Current profit is 16 * $6 - 4 * $4 = $80.
5. 4 board and 5 wait, the wheel rotates. Current profit is 20 * $6 - 5 * $4 = $100.
6. 4 board and 1 waits, the wheel rotates. Current profit is 24 * $6 - 6 * $4 = $120.
7. 1 boards, the wheel rotates. Current profit is 25 * $6 - 7 * $4 = $122.
The highest profit was $122 after rotating the wheel 7 times.
```

Example 3:

```text
Input: customers = [3,4,0,5,1], boardingCost = 1, runningCost = 92
Output: -1
Explanation:
1. 3 customers arrive, 3 board and 0 wait, the wheel rotates. Current profit is 3 * $1 - 1 * $92 = -$89.
2. 4 customers arrive, 4 board and 0 wait, the wheel rotates. Current profit is 7 * $1 - 2 * $92 = -$177.
3. 0 customers arrive, 0 board and 0 wait, the wheel rotates. Current profit is 7 * $1 - 3 * $92 = -$269.
4. 5 customers arrive, 4 board and 1 waits, the wheel rotates. Current profit is 11 * $1 - 4 * $92 = -$357.
5. 1 customer arrives, 2 board and 0 wait, the wheel rotates. Current profit is 13 * $1 - 5 * $92 = -$447.
The profit was never positive, so return -1.
```

__Constraints:__

- `n == customers.length`
- `1 <= n <= 10 ^ 5`
- `0 <= customers[i] <= 50`
- `1 <= boardingCost, runningCost <= 100`

__题目描述:__

你正在经营一座摩天轮，该摩天轮共有 __4 个座舱__ ，每个座舱 __最多可以容纳 4 位游客__ 。你可以 __逆时针__ 轮转座舱，但每次轮转都需要支付一定的运行成本 `runningCost` 。摩天轮每次轮转都恰好转动 1 / 4 周。

给你一个长度为 `n` 的数组 `customers` ， `customers[i]` 是在第 `i` 次轮转（下标从 0 开始）之前到达的新游客的数量。这也意味着你必须在新游客到来前轮转 `i` 次。每位游客在登上离地面最近的座舱前都会支付登舱成本 `boardingCost` ，一旦该座舱再次抵达地面，他们就会离开座舱结束游玩。

你可以随时停下摩天轮，即便是 在服务所有游客之前 。如果你决定停止运营摩天轮，为了保证所有游客安全着陆，将免费进行所有后续轮转 。注意，如果有超过 4 位游客在等摩天轮，那么只有 4 位游客可以登上摩天轮，其余的需要等待 下一次轮转 。

返回最大化利润所需执行的 __最小轮转次数__ 。 如果不存在利润为正的方案，则返回 `-1` 。

__示例:__

示例 1：

![1599-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/26/wheeldiagram12.png)

```text
输入：customers = [8,3], boardingCost = 5, runningCost = 6
输出：3
解释：座舱上标注的数字是该座舱的当前游客数。
1. 8 位游客抵达，4 位登舱，4 位等待下一舱，摩天轮轮转。当前利润为 4 * $5 - 1 * $6 = $14 。
2. 3 位游客抵达，4 位在等待的游客登舱，其他 3 位等待，摩天轮轮转。当前利润为 8 * $5 - 2 * $6 = $28 。
3. 最后 3 位游客登舱，摩天轮轮转。当前利润为 11 * $5 - 3 * $6 = $37 。
轮转 3 次得到最大利润，最大利润为 $37 。
```

示例 2：

```text
输入：customers = [10,9,6], boardingCost = 6, runningCost = 4
输出：7
解释：
1. 10 位游客抵达，4 位登舱，6 位等待下一舱，摩天轮轮转。当前利润为 4 * $6 - 1 * $4 = $20 。
2. 9 位游客抵达，4 位登舱，11 位等待（2 位是先前就在等待的，9 位新加入等待的），摩天轮轮转。当前利润为 8 * $6 - 2 * $4 = $40 。
3. 最后 6 位游客抵达，4 位登舱，13 位等待，摩天轮轮转。当前利润为 12 * $6 - 3 * $4 = $60 。
4. 4 位登舱，9 位等待，摩天轮轮转。当前利润为 * $6 - 4 * $4 = $80 。
5. 4 位登舱，5 位等待，摩天轮轮转。当前利润为 20 * $6 - 5 * $4 = $100 。
6. 4 位登舱，1 位等待，摩天轮轮转。当前利润为 24 * $6 - 6 * $4 = $120 。
7. 1 位登舱，摩天轮轮转。当前利润为 25 * $6 - 7 * $4 = $122 。
轮转 7 次得到最大利润，最大利润为$122 。
```

示例 3：

```text
输入：customers = [3,4,0,5,1], boardingCost = 1, runningCost = 92
输出：-1
解释：
1. 3 位游客抵达，3 位登舱，0 位等待，摩天轮轮转。当前利润为 3 * $1 - 1 * $92 = -$89 。
2. 4 位游客抵达，4 位登舱，0 位等待，摩天轮轮转。当前利润为 is 7 * $1 - 2 * $92 = -$177 。
3. 0 位游客抵达，0 位登舱，0 位等待，摩天轮轮转。当前利润为 7 * $1 - 3 * $92 = -$269 。
4. 5 位游客抵达，4 位登舱，1 位等待，摩天轮轮转。当前利润为 12 * $1 - 4 * $92 = -$356 。
5. 1 位游客抵达，2 位登舱，0 位等待，摩天轮轮转。当前利润为 13 * $1 - 5 * $92 = -$447 。
利润永不为正，所以返回 -1 。
```

__提示：__

- `n == customers.length`
- `1 <= n <= 10 ^ 5`
- `0 <= customers[i] <= 50`
- `1 <= boardingCost, runningCost <= 100`

__思路:__

```text
模拟
初始化结果为 -1
先计算每批游客到达之后, 最多能赚到多少钱, 每次上一批游客开动摩天轮次数自增
每次游客到达最多上 4 人, 最少可以上当前剩余人数
如果赚的钱变多了, 记录赚的钱数和当前的操作数
如果最后不剩下游客或者之后每次开动摩天轮赚的钱为负数, 直接返回结果
将剩下的游客按 4 人一批分组上摩天轮, 更新结果和赚的钱
最后一批游客因为人数不足开摩天轮可能不赚钱需要单独讨论
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperationsMaxProfit(vector<int>& customers, int boardingCost, int runningCost) 
    {
        int result = -1, total_profit = 0, max_profit = 0, count = 0, operations = 0, cur = 0, profit = (boardingCost << 2) - runningCost;
        for (const auto& c: customers) 
        {
            ++operations;
            count += c;
            cur = min(count, 4);
            count -= cur;
            total_profit += boardingCost * cur - runningCost;
            if (total_profit > max_profit) 
            {
                max_profit = total_profit;
                result = operations;
            }
        }
        if (!count or profit <= 0) return result;
        total_profit += (count >> 2) * profit;
        operations += (count >> 2);
        max_profit = total_profit;
        result = operations;
        return total_profit + (count % 4) * boardingCost - runningCost > max_profit ? ++result : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperationsMaxProfit(int[] customers, int boardingCost, int runningCost) {
        int result = -1, totalProfit = 0, maxProfit = 0, count = 0, operations = 0, cur = 0, profit = (boardingCost << 2) - runningCost;
        for (int c: customers) {
            ++operations;
            count += c;
            cur = Math.min(count, 4);
            count -= cur;
            totalProfit += boardingCost * cur - runningCost;
            if (totalProfit > maxProfit) {
                maxProfit = totalProfit;
                result = operations;
            }
        }
        if (count == 0 || profit <= 0) return result;
        totalProfit += (count >> 2) * profit;
        operations += (count >> 2);
        maxProfit = totalProfit;
        result = operations;
        return totalProfit + (count % 4) * boardingCost - runningCost > maxProfit ? ++result : result;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperationsMaxProfit(self, customers: List[int], boardingCost: int, runningCost: int) -> int:
        result, max_profit, total_profit, operations, count, profit = -1, 0, 0, 0, 0, (boardingCost << 2) - runningCost
        for c in customers:
            operations += 1
            count += c
            count -= (cur := min(count, 4))
            total_profit += boardingCost * cur - runningCost
            if total_profit > max_profit:
                max_profit, result = total_profit, operations
        if not count or profit <= 0:
            return result
        total_profit += profit * (times := count >> 2)
        operations += times
        max_profit, result = total_profit, operations
        return result + 1 if total_profit + (remain_profit := boardingCost * (count % 4) - runningCost) > max_profit else result
```
