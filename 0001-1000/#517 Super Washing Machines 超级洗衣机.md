# 517 Super Washing Machines 超级洗衣机

__Description__:
You have n super washing machines on a line. Initially, each washing machine has some dresses or is empty.

For each move, you could choose any m (1 ≤ m ≤ n) washing machines, and pass one dress of each washing machine to one of its adjacent washing machines at the same time .

Given an integer array representing the number of dresses in each washing machine from left to right on the line, you should find the minimum number of moves to make all the washing machines have the same number of dresses. If it is not possible to do it, return -1.

__Example:__

Example1

Input: [1,0,5]

Output: 3

Explanation:

```text
1st move:    1     0 <-- 5    =>    1     1     4
2nd move:    1 <-- 1 <-- 4    =>    2     1     3    
3rd move:    2     1 <-- 3    =>    2     2     2   
```

Example2

Input: [0,3,0]

Output: 2

Explanation:

```text
1st move:    0 <-- 3     0    =>    1     2     0    
2nd move:    1     2 --> 0    =>    1     1     1     
```

Example3

Input: [0,2,0]

Output: -1

Explanation:
It's impossible to make all the three washing machines have the same number of dresses.

__Note:__
The range of n is [1, 10000].
The range of dresses number in a super washing machine is [0, 1e5].

__题目描述__:
假设有 n 台超级洗衣机放在同一排上。开始的时候，每台洗衣机内可能有一定量的衣服，也可能是空的。

在每一步操作中，你可以选择任意 m （1 ≤ m ≤ n） 台洗衣机，与此同时将每台洗衣机的一件衣服送到相邻的一台洗衣机。

给定一个非负整数数组代表从左至右每台洗衣机中的衣物数量，请给出能让所有洗衣机中剩下的衣物的数量相等的最少的操作步数。如果不能使每台洗衣机中衣物的数量相等，则返回 -1。

__示例 :__

示例 1：

输入: [1,0,5]

输出: 3

解释:

```text
第一步:    1     0 <-- 5    =>    1     1     4
第二步:    1 <-- 1 <-- 4    =>    2     1     3    
第三步:    2     1 <-- 3    =>    2     2     2  
```

示例 2：

输入: [0,3,0]

输出: 2

解释:

```text
第一步:    0 <-- 3     0    =>    1     2     0    
第二步:    1     2 --> 0    =>    1     1     1  
```

示例 3:

输入: [0,2,0]

输出: -1

解释:
不可能让所有三个洗衣机同时剩下相同数量的衣物。

__提示:__

n 的范围是 [1, 10000]。
在每台超级洗衣机中，衣物数量的范围是 [0, 1e5]。

__思路__:

贪心
计算数组的总和 sum
如果 sum % n != 0直接返回 -1, 这时不能均分衣服
记录 target = sum / n
分别求每个机器与 target的差值, 用 balance累加, 求出 balance绝对值和差值的最大值即可
比如 [1, 0, 5] -> [-1, -2, 2] -> balance[0] = -1 -> balance[1] = -3 -> balance[2] = 0
返回 3
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findMinMoves(vector<int>& machines) 
    {
        int sum = accumulate(machines.begin(), machines.end(), 0), n = machines.size();
        if (sum % n) return -1;
        int target = sum / n, result = 0, balance = 0;
        for (const auto& machine : machines) 
        {
            balance += machine - target;
            result = max({result, abs(balance), machine - target});
        }

        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findMinMoves(int[] machines) {
        int sum = 0, n = machines.length;
        for (int machine : machines) sum += machine;
        if (sum % n != 0) return -1;
        int target = sum / n, result = 0, balance = 0;
        for (int machine : machines) {
            balance += machine - target;
            result = Math.max(result, Math.max(Math.abs(balance), machine - target));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findMinMoves(self, machines: List[int]) -> int:
        s, n = sum(machines), len(machines)
        if s % n:
            return -1
        target, result, balance = s // n, 0, 0
        for machine in machines:
            balance += machine - target
            result = max(result, machine - target, abs(balance))
        return result
```
