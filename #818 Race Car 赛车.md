# 818 Race Car 赛车

__Description__:
Your car starts at position 0 and speed +1 on an infinite number line. Your car can go into negative positions. Your car drives automatically according to a sequence of instructions 'A' (accelerate) and 'R' (reverse):

When you get an instruction 'A', your car does the following:
position += speed
speed *= 2
When you get an instruction 'R', your car does the following:
If your speed is positive then speed = -1
otherwise speed = 1
Your position stays the same.
For example, after commands "AAR", your car goes to positions 0 --> 1 --> 3 --> 3, and your speed goes to 1 --> 2 --> 4 --> -1.

Given a target position target, return the length of the shortest sequence of instructions to get there.

__Example:__

Example 1:

Input: target = 3
Output: 2
Explanation:
The shortest instruction sequence is "AA".
Your position goes from 0 --> 1 --> 3.

Example 2:

Input: target = 6
Output: 5
Explanation:
The shortest instruction sequence is "AAARA".
Your position goes from 0 --> 1 --> 3 --> 7 --> 7 --> 6.

__Constraints:__

1 <= target <= 10^4

__题目描述__:
你的赛车起始停留在位置 0，速度为 +1，正行驶在一个无限长的数轴上。（车也可以向负数方向行驶。）

你的车会根据一系列由 A（加速）和 R（倒车）组成的指令进行自动驾驶 。

当车得到指令 "A" 时, 将会做出以下操作： position += speed, speed *= 2。

当车得到指令 "R" 时, 将会做出以下操作：如果当前速度是正数，则将车速调整为 speed = -1 ；否则将车速调整为 speed = 1。  (当前所处位置不变。)

例如，当得到一系列指令 "AAR" 后, 你的车将会走过位置 0->1->3->3，并且速度变化为 1->2->4->-1。

现在给定一个目标位置，请给出能够到达目标位置的最短指令列表的长度。

__示例 :__

示例 1:
输入:
target = 3
输出: 2
解释:
最短指令列表为 "AA"
位置变化为 0->1->3

示例 2:
输入:
target = 6
输出: 5
解释:
最短指令列表为 "AAARA"
位置变化为 0->1->3->7->7->6

__说明:__

1 <= target（目标位置） <= 10000。

__思路__:

动态规划
设 dp[t] 表示到达 t 的最短指令长度
首先可以计算出 2 ^ k - 1 是等于 k 的, 使用 A * k 即可, 即 dp[2 ^ k - 1] = k
然后搜索在 2 ^ (k - 1) <= t < 2 ^ k 范围内的步数
有两种方式可以到达 t
先到达 2 ^ (k - 1) - 1, 然后往回走一点, 再掉头到达 t, 往前走的步数包括回头最多为 k, 不然到达 2 ^ (k - 1) - 1 等于白走, 然后再回头走到终点, dp[t] = min(dp[t], k + 1 + back + 1 + dp[t - 2 ^ k + 1 - 2 ^ back + 1), 其中 -1 < back < k - 1
或者直接超过 t, 再往回走, 这时dp[t] = min(dp[t], k + 1 + dp[2 ^ k - 1 - t]), 回头需要消耗一个 R
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    int dp[10001];
public:
    int racecar(int t) 
    {
        if (dp[t] > 0) return dp[t];
        int n = floor(log2(t)) + 1;
        if (1 << n == t + 1) dp[t] = n;
        else 
        {
            dp[t] = racecar((1 << n) - 1 - t) + n + 1;
            for (int m = 0; m < n - 1; ++m) dp[t] = min(dp[t], racecar(t - (1 << (n - 1)) + (1 << m)) + n + m + 1);
        }
        return dp[t];
    }
};
```

__Java__:

```Java
class Solution {
    public int racecar(int target) {
        int[] dp = new int[target + 3];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0; 
        dp[1] = 1; 
        dp[2] = 4;
        for (int t = 3; t <= target; t++) {
            int k = 32 - Integer.numberOfLeadingZeros(t);
            if (t == (1 << k) - 1) {
                dp[t] = k;
                continue;
            }
            for (int j = 0; j < k-1; j++) dp[t] = Math.min(dp[t], dp[t - (1<<(k-1)) + (1<<j)] + k-1 + j + 2);
            if ((1 << k) - 1 - t < t) dp[t] = Math.min(dp[t], dp[(1 << k) - 1 - t] + k + 1);
        }
        return dp[target];  
    }
}
```

__Python__:

```Python
class Solution:
    def racecar(self, target: int) -> int:
        dp = [0, 1, 4] + [float('inf')] * target
        for t in range(3, target + 1):
            k = t.bit_length()
            if t == 2 ** k - 1:
                dp[t] = k
            else:
                for j in range(k - 1):
                    dp[t] = min(dp[t], dp[t - 2 ** (k - 1) + 2 ** j] + k - 1 + j + 2)
                if 2 ** k - 1 - t < t:
                    dp[t] = min(dp[t], dp[2 ** k - 1 - t] + k + 1)
        return dp[target]
```
