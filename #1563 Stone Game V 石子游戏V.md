# 1563 Stone Game V 石子游戏V

__Description:__

There are several stones __arranged in a row__, and each stone has an associated value which is an integer given in the array `stoneValue`.

In each round of the game, Alice divides the row into two non-empty rows (i.e. left row and right row), then Bob calculates the value of each row which is the sum of the values of all the stones in this row. Bob throws away the row which has the maximum value, and Alice's score increases by the value of the remaining row. If the value of the two rows are equal, Bob lets Alice decide which row will be thrown away. The next round starts with the remaining row.

The game ends when there is only one stone remaining. Alice's is initially zero.

Return the maximum score that Alice can obtain.

__Example:__

Example 1:

```text
Input: stoneValue = [6,2,3,4,5,5]
Output: 18
Explanation: In the first round, Alice divides the row to [6,2,3], [4,5,5]. The left row has the value 11 and the right row has value 14. Bob throws away the right row and Alice's score is now 11.
In the second round Alice divides the row to [6], [2,3]. This time Bob throws away the left row and Alice's score becomes 16 (11 + 5).
The last round Alice has only one choice to divide the row which is [2], [3]. Bob throws away the right row and Alice's score is now 18 (16 + 2). The game ends because only one stone is remaining in the row.
```

Example 2:

```text
Input: stoneValue = [7,7,7,7,7,7,7]
Output: 28
```

Example 3:

```text
Input: stoneValue = [4]
Output: 0
```

__Constraints:__

- `1 <= stoneValue.length <= 500`
- `1 <= stoneValue[i] <= 10 ^ 6`

__题目描述:__

几块石子 __排成一行__ ，每块石子都有一个关联值，关联值为整数，由数组 `stoneValue` 给出。

游戏中的每一轮：Alice 会将这行石子分成两个 非空行（即，左侧行和右侧行）；Bob 负责计算每一行的值，即此行中所有石子的值的总和。Bob 会丢弃值最大的行，Alice 的得分为剩下那行的值（每轮累加）。如果两行的值相等，Bob 让 Alice 决定丢弃哪一行。下一轮从剩下的那一行开始。

只 __剩下一块石子__ 时，游戏结束。Alice 的分数最初为 __`0`__ 。

返回 Alice 能够获得的最大分数 。

__示例:__

示例 1：

```text
输入：stoneValue = [6,2,3,4,5,5]
输出：18
解释：在第一轮中，Alice 将行划分为 [6，2，3]，[4，5，5] 。左行的值是 11 ，右行的值是 14 。Bob 丢弃了右行，Alice 的分数现在是 11 。
在第二轮中，Alice 将行分成 [6]，[2，3] 。这一次 Bob 扔掉了左行，Alice 的分数变成了 16（11 + 5）。
最后一轮 Alice 只能将行分成 [2]，[3] 。Bob 扔掉右行，Alice 的分数现在是 18（16 + 2）。游戏结束，因为这行只剩下一块石头了。
```

示例 2：

```text
输入：stoneValue = [7,7,7,7,7,7,7]
输出：28
```

示例 3：

```text
输入：stoneValue = [4]
输出：0
```

__提示：__

- `1 <= stoneValue.length <= 500`
- `1 <= stoneValue[i] <= 10 ^ 6`

__思路:__

```text
1. 动态规划
设区间 [left, right] 的得分为 dp[left][right]
并记区间和为 sum(left, right) 表示 sum(stoneValue[left:right + 1])
边界条件为 dp[left][left] = 0, 表示只有一个石子时得分为 0
对于 Alice 选择的区间内某个点 i
如果 sum(left, i) < sum(i, right), dp[left][right] = dp[left][right] + sum(l, i)
如果 sum(left, i) > sum(i, right), dp[left][right] = dp[left][right] + sum(i, right)
否则 sum(left, i) == sum(i, right), 任取一段, dp[left][right] = max(dp[left][i], dp[i + 1][right]) + sum(l, i)
通过遍历 [left, right] 中的每一个 i 的取值即可得到 dp[left][right]
快速求取 sum(left, right) 可以用前缀和
最后返回 dp[0][n - 1] 即可
这种区间动态规划第一维一般用区间长度, 可能可以使用二分法优化
时间复杂度为 O(N ^ 3), 空间复杂度为 O(N ^ 2)
2. 优化的动态规划
注意到 stoneValue 中的元素全部为正, 在方法一中进行了大量的重复计算
因为如果 sum(left, i) < sum(i, right), 显然有 sum(left, i) < sum(i, right) + stoneValue[right + 1] = sum(i, right + 1), 那么此时也有 dp[left][right] = dp[left][right + 1] = dp[left][i] + sum(i, right)
可以用两个数组记录
maxl[left][right] = max(dp[left][i] + sum(l, i)), 其中 left <= i <= right
maxr[left][right] = max(dp[i][right] + sum(i, right)), 其中 left <= i <= right
那么总可以找到一个 i, 使得 sum(left, i) <= sum(i + 1, right), 同时 sum(left, i + 1) > sum(i + 2, right), 其中 left - 1 <= i < right
这样就能在 O(1) 的时间计算出 dp[left][right]
注意到 maxl[left][right + 1] = max(maxl[left][right], dp[left][right + 1] + sum(left, right + 1)), 可以用前缀和的方式计算 maxl 和 maxr 数组
初始值 maxl[left][left] = maxr[right][right] = sum(left, right) = stoneValue[left] = stoneValue[right], left = right
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    vector<vector<int>> dp;
    vector<int> pre;
    
    int dfs(const vector<int>& stoneValue, int left, int right) 
    {
        if (left == right) return 0;
        if (dp[left][right]) return dp[left][right];
        int sum = pre[right + 1] - pre[left], suml = 0;
        for (int i = left; i < right; i++) 
        {
            suml += stoneValue[i];
            int sumr = sum - suml;
            if (suml < sumr) dp[left][right] = max(dp[left][right], dfs(stoneValue, left, i) + suml);
            else if (suml > sumr) dp[left][right] = max(dp[left][right], dfs(stoneValue, i + 1, right) + sumr);
            else dp[left][right] = max(dp[left][right], max(dfs(stoneValue, left, i), dfs(stoneValue, i + 1, right)) + suml);
        }
        return dp[left][right];
    }
public:
    int stoneGameV(vector<int>& stoneValue) 
    {
        int n = stoneValue.size();
        pre.resize(n + 1);
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + stoneValue[i];
        dp.assign(n, vector<int>(n));
        return dfs(stoneValue, 0, n - 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int stoneGameV(int[] stoneValue) {
        int n = stoneValue.length, result = 0, dp[][] = new int[n][n], pre[] = new int[n + 1];
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + stoneValue[i];
        for (int len = 2; len <= n; len++) {
            for (int left = 0; left + len - 1 < n; left++) {
                for (int i = left, right = left + len - 1; i < right; i++) {
                    int suml = pre[i + 1] - pre[left], sumr = pre[right + 1] - pre[i + 1];
                    if (suml > sumr) dp[left][right] = Math.max(dp[left][right], sumr + dp[i + 1][right]);
                    else if (suml < sumr) dp[left][right] = Math.max(dp[left][right], suml + dp[left][i]);
                    else dp[left][right] = Math.max(dp[left][right], Math.max(suml + dp[left][i], sumr + dp[i + 1][right]));
                }
            }
        }
        return dp[0][n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def stoneGameV(self, stoneValue: List[int]) -> int:
        n = len(stoneValue)
        dp, maxl, maxr = [[0] * n for _ in range(n)], [[0] * n for _ in range(n)], [[0] * n for _ in range(n)]
        for left in range(n - 1, -1, -1):
            total = maxl[left][left] = maxr[left][left] = stoneValue[left]
            suml, i = 0, left - 1
            for right in range(left + 1, n):
                total += stoneValue[right]
                while i + 1 < right and (suml + stoneValue[i + 1]) << 1 <= total:
                    suml += stoneValue[i + 1]
                    i += 1
                if left <= i:
                    dp[left][right] = max(dp[left][right], maxl[left][i])
                if i + 1 < right:
                    dp[left][right] = max(dp[left][right], maxr[i + 2][right])
                if suml << 1 == total:
                    dp[left][right] = max(dp[left][right], maxr[i + 1][right])
                maxl[left][right], maxr[left][right] = max(maxl[left][right - 1], total + dp[left][right]), max(maxr[left + 1][right], total + dp[left][right])
        return dp[0][-1]
```
