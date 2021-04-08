# 546 Remove Boxes 移除盒子

__Description__:
You are given several boxes with different colors represented by different positive numbers.

You may experience several rounds to remove boxes until there is no box left. Each time you can choose some continuous boxes with the same color (i.e., composed of k boxes, k >= 1), remove them and get k * k points.

Return the maximum points you can get.

__Example:__

Example 1:

Input: boxes = [1,3,2,2,2,3,4,3,1]
Output: 23
Explanation:
[1, 3, 2, 2, 2, 3, 4, 3, 1]
----> [1, 3, 3, 4, 3, 1] (3*3=9 points)
----> [1, 3, 3, 3, 1] (1*1=1 points)
----> [1, 1] (3*3=9 points)
----> [] (2*2=4 points)

Example 2:

Input: boxes = [1,1,1]
Output: 9
Example 3:

Input: boxes = [1]
Output: 1

__Constraints:__

1 <= boxes.length <= 100
1 <= boxes[i] <= 100

__题目描述__:
给出一些不同颜色的盒子，盒子的颜色由数字表示，即不同的数字表示不同的颜色。

你将经过若干轮操作去去掉盒子，直到所有的盒子都去掉为止。每一轮你可以移除具有相同颜色的连续 k 个盒子（k >= 1），这样一轮之后你将得到 k * k 个积分。

当你将所有盒子都去掉之后，求你能获得的最大积分和。

__示例 :__

示例 1：

输入：boxes = [1,3,2,2,2,3,4,3,1]
输出：23
解释：
[1, 3, 2, 2, 2, 3, 4, 3, 1]
----> [1, 3, 3, 4, 3, 1] (3*3=9 分)
----> [1, 3, 3, 3, 1] (1*1=1 分)
----> [1, 1] (3*3=9 分)
----> [] (2*2=4 分)

示例 2：

输入：boxes = [1,1,1]
输出：9
示例 3：

输入：boxes = [1]
输出：1

__提示:__

1 <= boxes.length <= 100
1 <= boxes[i] <= 100

__思路__:

动态规划
首先想到的是区间问题 dp[l][r], 但是这里是不行的, 因为 dp[l][r] 会受到 boxes[r + 1:] 之后的数的影响
比如 [5, 6, 5, 6, 6, 6] 后面的 3 个 6 可以和第 1 个 6 组合获得最大的分数
所以可以再加上一维, 记录 [l, r] 区间之后还有多少个元素与 boxes[r] 相同
dp[l][r][k] 表示在 [l, r] 区间获得的最大分数, 且这时 boxes[r + 1:] 中有 k 个数与 boxes[r] 相等
动态转移方程为 dp[l][r][k] = max(dp[l][r - 1][0] + (k + 1) ^ 2, max(dp[l][i][k + 1] + dp[i + 1][r - 1][0]) * (boxes[i] == boxes[r]), 其中 l <= i < r
因为要么最后 k 个数是可以跟 boxes[r] 一起组合的, 这时得分为 dp[l][r - 1][0] + (k + 1) ^ 2, 要么在 [l, r] 中搜索是否有能和最后 k 个数组合的消除方式
比如 [3, 1, 2, 3, 4, 3, 3], 搜索 dp[0][3][2], 区间为 [0, 3], 即 [3, 1, 2, 3], k = 2, 表示在 boxes[r + 1:], 即 [4, 3, 3] 中有 2个数与 boxes[3] 相等, 要么直接删除最后一个数, 分数为 dp[0][2][0] + (2 + 1) ^ 2, 注意这里当作 [0, 2] 区间以外的数字全部被删除, 否则无法删除后面连续的 3; 要么将 boxes[0] 和后面的一起删除, 即 [3], [1, 2], [3, 4, 3, 3], 分数为 dp[0][0][3] + dp[1][2][0], 前者为 [0, 0] 区间和 [3, 6] 区间所有等于 3 的一起删除的分数, 后者为 [1, 2] 这个区间删除的分数
注意到 boxes[r] 有可能等于 boxes[r - 1], 可以从第一个相邻不等的数组元素开始初始化
时间复杂度 O(n ^ 4), 空间复杂度 O(n ^ 3)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    int dp[100][100][100];
    int helper(vector<int>& boxes, int l, int r, int k)
    {
        if (l > r) return 0;
        if (!dp[l][r][k])
        {
            int r1 = r, k1 = k;
            while (r1 > l and boxes[r1] == boxes[r1 - 1])
            {
                --r1;
                ++k1;
            }
            dp[l][r][k] = helper(boxes, l, r1 - 1, 0) + (k1 + 1) * (k1 + 1);
            for (int i = l; i < r1; i++) if (boxes[r1] == boxes[i]) dp[l][r][k] = max(dp[l][r][k], helper(boxes, l, i, k1 + 1) + helper(boxes, i + 1, r1 - 1, 0));
        }
        return dp[l][r][k];
    }
public:
    int removeBoxes(vector<int>& boxes) 
    {
        memset(dp, 0, sizeof dp);
        return helper(boxes, 0, boxes.size() - 1, 0);
    }
};
```

__Java__:

```Java
class Solution {
    private int[][][] dp;
    
    public int removeBoxes(int[] boxes) {
        dp = new int[boxes.length][boxes.length][boxes.length];
        return helper(boxes, 0, boxes.length - 1, 0);
    }
    
    private int helper(int[] boxes, int l, int r, int k) {
        if (l > r) return 0;
        if (dp[l][r][k] == 0) {
            dp[l][r][k] = helper(boxes, l, r - 1, 0) + (k + 1) * (k + 1);
            for (int i = l; i < r; i++) if (boxes[i] == boxes[r]) dp[l][r][k] = Math.max(dp[l][r][k], helper(boxes, l, i, k + 1) + helper(boxes, i + 1, r - 1, 0));
        }
        return dp[l][r][k];
    }
}
```

__Python__:

```Python
class Solution:
    def removeBoxes(self, boxes: List[int]) -> int:
        n = len(boxes)
        dp = [[[0] * n for _ in range(n)] for _ in range(n)]
        def helper(l: int, r: int, k: int) -> int:
            if l > r:
                return 0
            if not dp[l][r][k]:
                r1, k1 = r, k
                while r1 > l and boxes[r1] == boxes[r1 - 1]:
                    r1 -= 1
                    k1 += 1
                dp[l][r][k] = helper(l, r1 - 1, 0) + (k1 + 1) ** 2
                for i in range(l, r1):
                    if boxes[r1] == boxes[i]:
                        dp[l][r][k] = max(dp[l][r][k], helper(l, i, k1 + 1) + helper(i + 1, r1 - 1, 0))
            return dp[l][r][k]
        return helper(0, n - 1, 0)
```
