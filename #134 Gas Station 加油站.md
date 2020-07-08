__Description__:
There are N gas stations along a circular route, where the amount of gas at station i is gas[i].

You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from station i to its next station (i+1). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1.

__Note:__

If there exists a solution, it is guaranteed to be unique.
Both input arrays are non-empty and have the same length.
Each element in the input arrays is a non-negative integer.

__Example:__
Example 1:

Input: 
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]

Output: 3

Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.

Example 2:

Input: 
gas  = [2,3,4]
cost = [3,4,3]

Output: -1

Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.

__题目描述__:
在一条环路上有 N 个加油站，其中第 i 个加油站有汽油 gas[i] 升。

你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。

如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1。

__说明：__

如果题目有解，该答案即为唯一答案。
输入数组均为非空数组，且长度相同。
输入数组中的元素均为非负数。

__示例 :__
示例 1:

输入: 
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]

输出: 3

解释:
从 3 号加油站(索引为 3 处)出发，可获得 4 升汽油。此时油箱有 = 0 + 4 = 4 升汽油
开往 4 号加油站，此时油箱有 4 - 1 + 5 = 8 升汽油
开往 0 号加油站，此时油箱有 8 - 2 + 1 = 7 升汽油
开往 1 号加油站，此时油箱有 7 - 3 + 2 = 6 升汽油
开往 2 号加油站，此时油箱有 6 - 4 + 3 = 5 升汽油
开往 3 号加油站，你需要消耗 5 升汽油，正好足够你返回到 3 号加油站。
因此，3 可为起始索引。

示例 2:

输入: 
gas  = [2,3,4]
cost = [3,4,3]

输出: -1

解释:
你不能从 0 号或 1 号加油站出发，因为没有足够的汽油可以让你行驶到下一个加油站。
我们从 2 号加油站出发，可以获得 4 升汽油。 此时油箱有 = 0 + 4 = 4 升汽油
开往 0 号加油站，此时油箱有 4 - 3 + 2 = 3 升汽油
开往 1 号加油站，此时油箱有 3 - 3 + 3 = 3 升汽油
你无法返回 2 号加油站，因为返程需要消耗 4 升汽油，但是你的油箱只有 3 升汽油。
因此，无论怎样，你都不可能绕环路行驶一周。

__思路__:
1.  对每一个点进行测试, 看能否按照规则跑一圈
时间复杂度O(n ^ 2), 空间复杂度O(1)
2. 首先, 每个站点的富余油量应该是 gas[i] - cost[i], 表示经过该点可以额外获得多少汽油, 负值表示消耗量
如果 sum(gas[i] - cost[i]) < 0则油量不够, 一定不能跑一圈
用 rest记录富余油量, run记录已经经过的点需要的油量, 初始化均为 0
比如
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]
rest = [-2, -2, -2, 3, 3]
i = 0, rest = -2, run = 0, rest < run
rest < run表示剩余的油量不足以支撑汽车跑过这个点
令 run = rest, 重新从下一个开始计算, 这个时候 run表示从该点出发如果能覆盖前面的点需要的油量
最后检查 rest如果不为负, 说明可以跑一圈
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) 
    {
        int result = 0, rest = 0, run = 0;
        for (int i = 0; i < gas.size(); i++) 
        {
            rest += gas[i] - cost[i];
            if (rest < run) 
            {
                run = rest;
                result = i + 1;
            }
        }
        return rest < 0 ? -1 : result;
    }
};
```

__Java__:
```Java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int result = 0, rest = 0, run = 0;
        for (int i = 0; i < gas.length; i++) {
            rest += gas[i] - cost[i];
            if (rest < run) {
                run = rest;
                result = i + 1;
            }
        }
        return rest < 0 ? -1 : result;
    }
}
```

__Python__:
```Python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        rest, run, result = 0, 0, 0
        for i in range(len(gas)):
            rest += gas[i] - cost[i]
            if rest < run:
                run = rest
                result = i + 1
        return -1 if rest < 0 else result
```