__Description__:
On a staircase, the i-th step has some non-negative cost cost[i] assigned (0 indexed).

Once you pay the cost, you can either climb one or two steps. You need to find minimum cost to reach the top of the floor, and you can either start from the step with index 0, or the step with index 1.

__Example:__
Example 1:

Input: cost = [10, 15, 20]
Output: 15
Explanation: Cheapest is start on cost[1], pay that cost and go to the top.

Example 2:

Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
Output: 6
Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[3].

__Note:__

cost will have a length in the range [2, 1000].
Every cost[i] will be an integer in the range [0, 999].

__题目描述__:
数组的每个索引做为一个阶梯，第 i个阶梯对应着一个非负数的体力花费值 cost[i](索引从0开始)。

每当你爬上一个阶梯你都要花费对应的体力花费值，然后你可以选择继续爬一个阶梯或者爬两个阶梯。

您需要找到达到楼层顶部的最低花费。在开始时，你可以选择从索引为 0 或 1 的元素作为初始阶梯。

__示例 :__
示例 1:

输入: cost = [10, 15, 20]
输出: 15
解释: 最低花费是从cost[1]开始，然后走两步即可到阶梯顶，一共花费15。

 示例 2:

输入: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
输出: 6
解释: 最低花费方式是从cost[0]开始，逐个经过那些1，跳过cost[3]，一共花费6。

__注意：__

cost 的长度将会在 [2, 1000]。
每一个 cost[i] 将会是一个Integer类型，范围为 [0, 999]。

__思路__:
参考[LeetCode #70 Climbing Stairs 爬楼梯](https://www.jianshu.com/p/8d7ceb7b7cf6)
加上每次选择最小的代价即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        for (int i = 2; i < cost.size(); i++) cost[i] += min(cost[i - 1], cost[i - 2]);
        return cost[cost.size() - 1] < cost[cost.size() - 2] ? cost[cost.size() - 1] : cost[cost.size() - 2];
    }
};
```

__Java__:
```
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        for (int i = 2; i < cost.length; i++) cost[i] += Math.min(cost[i - 1], cost[i - 2]);
        return cost[cost.length - 1] < cost[cost.length - 2] ? cost[cost.length - 1] : cost[cost.length - 2];
    }
}
```

__Python__:
```
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        temp, result = 0, 0
        for i in range(2, len(cost) + 1):
            temp, result = result, min(result + cost[i - 1], temp + cost[i - 2]) 
        return result
```