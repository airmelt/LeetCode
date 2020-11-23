__Description__:
We are playing the Guessing Game. The game will work as follows:

I pick a number between 1 and n.
You guess a number.
If you guess the right number, you win the game.
If you guess the wrong number, then I will tell you whether the number I picked is higher or lower, and you will continue guessing.
Every time you guess a wrong number x, you will pay x dollars. If you run out of money, you lose the game.
Given a particular n, return the minimum amount of money you need to guarantee a win regardless of what number I pick.

__Example:__
Example 1:
![Guess Tree](https://assets.leetcode.com/uploads/2020/09/10/graph.png)
Input: n = 10
Output: 16
Explanation: The winning strategy is as follows:
- The range is [1,10]. Guess 7.
    - If this is my number, your total is \$0. Otherwise, you pay \$7.
    - If my number is higher, the range is [8,10]. Guess 9.
        - If this is my number, your total is \$7. Otherwise, you pay \$9.
        - If my number is higher, it must be 10. Guess 10. Your total is \$7 + \$9 = \$16.
        - If my number is lower, it must be 8. Guess 8. Your total is \$7 + \$9 = \$16.
    - If my number is lower, the range is [1,6]. Guess 3.
        - If this is my number, your total is \$7. Otherwise, you pay \$3.
        - If my number is higher, the range is [4,6]. Guess 5.
            - If this is my number, your total is \$7 + \$3 = \$10. Otherwise, you pay \$5.
            - If my number is higher, it must be 6. Guess 6. Your total is \$7 + \$3 + \$5 = \$15.
            - If my number is lower, it must be 4. Guess 4. Your total is \$7 + \$3 + \$5 = \$15.
        - If my number is lower, the range is [1,2]. Guess 1.
            - If this is my number, your total is \$7 + \$3 = \$10. Otherwise, you pay \$1.
            - If my number is higher, it must be 2. Guess 2. Your total is \$7 + \$3 + \$1 = \$11.
The worst case in all these scenarios is that you pay \$16. Hence, you only need \$16 to guarantee a win.

Example 2:

Input: n = 1
Output: 0
Explanation: There is only one possible number, so you can guess 1 and not have to pay anything.

Example 3:

Input: n = 2
Output: 1
Explanation: There are two possible numbers, 1 and 2.
- Guess 1.
    - If this is my number, your total is $0. Otherwise, you pay $1.
    - If my number is higher, it must be 2. Guess 2. Your total is $1.
The worst case is that you pay $1.
 
__Constraints:__

1 <= n <= 200

__题目描述__:
我们正在玩一个猜数游戏，游戏规则如下：

我从 1 到 n 之间选择一个数字，你来猜我选了哪个数字。

每次你猜错了，我都会告诉你，我选的数字比你的大了或者小了。

然而，当你猜了数字 x 并且猜错了的时候，你需要支付金额为 x 的现金。直到你猜到我选的数字，你才算赢得了这个游戏。

__示例 :__

n = 10, 我选择了8.

第一轮: 你猜我选择的数字是5，我会告诉你，我的数字更大一些，然后你需要支付5块。
第二轮: 你猜是7，我告诉你，我的数字更大一些，你支付7块。
第三轮: 你猜是9，我告诉你，我的数字更小一些，你支付9块。

游戏结束。8 就是我选的数字。

你最终要支付 5 + 7 + 9 = 21 块钱。
给定 n ≥ 1，计算你至少需要拥有多少现金才能确保你能赢得这个游戏。

__思路__:
动态规划
dp[i][j]表示落入 [i, j]时, 需要支付的最小的钱的数量
比如 dp[1][1]表示当前区间为 [1, 1], 这时只用猜 1必定猜中, 所以 dp[1][1] = 0
类似的有 dp[i][i] = 0
dp[2][3]表示当前区间为 [2, 3], 这时如果答案是 2, 猜 2不用给钱, 猜 3需要给 3块, 如果答案是 3, 猜 2给 2块, 猜 3不用给, 那么选择猜 2, 最少支付 2块一定猜中
动态转移方程 dp[i][j] = max(dp[i][k - 1], dp[k + 1][j]) + k, i < k < j, dp[i][i] = 0
时间复杂度O(n ^ 3), 空间复杂度O(n ^ 2)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int getMoneyAmount(int n) 
    {
        vector<vector<int>> dp(n + 2, vector<int>(n + 2, 0));
        for (int i = n; i > 0; i--) for (int j = i + 1; j < n + 1; j++) 
        {
            dp[i][j] = INT_MAX;
            for (int k = i; k < j + 1; k++) dp[i][j] = min(dp[i][j], max(dp[i][k - 1], dp[k + 1][j]) + k);
        }
        return dp[1][n];
    }
};
```

__Java__:
```Java
class Solution {
    public int getMoneyAmount(int n) {
        int dp[][] = new int[n + 2][n + 2];
        for (int i = n; i > 0; i--) for (int j = i + 1; j < n + 1; j++) {
            dp[i][j] = Integer.MAX_VALUE;
            for (int k = i; k < j + 1; k++) dp[i][j] = Math.min(dp[i][j], Math.max(dp[i][k - 1], dp[k + 1][j]) + k);
        }
        return dp[1][n];
    }
}
```

__Python__:
```Python
class Solution:
    def getMoneyAmount(self, n: int) -> int:
        dp = [[0] * (n + 2) for _ in range(n + 2)]
        for i in range(n, 0, -1):
            for j in range(i + 1, n + 1):
                dp[i][j] = float('inf')
                for k in range(i, j + 1):
                    dp[i][j] = min(dp[i][j], max(dp[i][k - 1], dp[k + 1][j]) + k)
        return dp[1][n]
```