# 2585 Number of Ways to Earn Points 获得分数的方法数

__Description:__

There is a test that has `n` types of questions. You are given an integer `target` and a __0-indexed__ 2D integer array `types` where `types[i] = [count_i, marks_i]` indicates that there are `count_i` questions of the `i ^ th` type, and each one of them is worth `marks_i` points.

Return _the number of ways you can earn __exactly___ `target` _points in the exam_. Since the answer may be too large, return it __modulo__ `10 ^ 9 + 7`.

Note that questions of the same type are indistinguishable.

- For example, if there are `3` questions of the same type, then solving the `1 ^ st` and `2 ^ nd` questions is the same as solving the `1 ^ st` and `3 ^ rd` questions, or the `2 ^ nd` and `3 ^ rd` questions.

__Example:__

Example 1:

```text
Input: target = 6, types = [[6,1],[3,2],[2,3]]
Output: 7
Explanation: You can earn 6 points in one of the seven ways:
```

- Solve 6 questions of the 0th type: 1 + 1 + 1 + 1 + 1 + 1 = 6
- Solve 4 questions of the 0th type and 1 question of the 1st type: 1 + 1 + 1 + 1 + 2 = 6
- Solve 2 questions of the 0th type and 2 questions of the 1st type: 1 + 1 + 2 + 2 = 6
- Solve 3 questions of the 0th type and 1 question of the 2nd type: 1 + 1 + 1 + 3 = 6
- Solve 1 question of the 0th type, 1 question of the 1st type and 1 question of the 2nd type: 1 + 2 + 3 = 6
- Solve 3 questions of the 1st type: 2 + 2 + 2 = 6
- Solve 2 questions of the 2nd type: 3 + 3 = 6

Example 2:

```text
Input: target = 5, types = [[50,1],[50,2],[50,5]]
Output: 4
Explanation: You can earn 5 points in one of the four ways:
```

- Solve 5 questions of the 0th type: 1 + 1 + 1 + 1 + 1 = 5
- Solve 3 questions of the 0th type and 1 question of the 1st type: 1 + 1 + 1 + 2 = 5
- Solve 1 questions of the 0th type and 2 questions of the 1st type: 1 + 2 + 2 = 5
- Solve 1 question of the 2nd type: 5

Example 3:

```text
Input: target = 18, types = [[6,1],[3,2],[2,3]]
Output: 1
Explanation: You can only earn 18 points by answering all questions.
```

__Constraints:__

- `1 <= target <= 1000`
- `n == types.length`
- `1 <= n <= 50`
- `types[i].length == 2`
- `1 <= count_i, marks_i <= 50`

__题目描述:__

考试中有 `n` 种类型的题目。给你一个整数 `target` 和一个下标从 __0__ 开始的二维整数数组 `types` ，其中 `types[i] = [count_i, marks_i]` 表示第 `i` 种类型的题目有 `count_i` 道，每道题目对应 `marks_i` 分。

返回你在考试中恰好得到 `target` 分的方法数。由于答案可能很大，结果需要对 `10 ^ 9 +7` 取余。

注意，同类型题目无法区分。

- 比如说，如果有 `3` 道同类型题目，那么解答第 `1` 和第 `2` 道题目与解答第 `1` 和第 `3` 道题目或者第 `2` 和第 `3` 道题目是相同的。

__示例:__

示例 1：

```text
输入：target = 6, types = [[6,1],[3,2],[2,3]]
输出：7
解释：要获得 6 分，你可以选择以下七种方法之一：
```

- 解决 6 道第 0 种类型的题目：1 + 1 + 1 + 1 + 1 + 1 = 6
- 解决 4 道第 0 种类型的题目和 1 道第 1 种类型的题目：1 + 1 + 1 + 1 + 2 = 6
- 解决 2 道第 0 种类型的题目和 2 道第 1 种类型的题目：1 + 1 + 2 + 2 = 6
- 解决 3 道第 0 种类型的题目和 1 道第 2 种类型的题目：1 + 1 + 1 + 3 = 6
- 解决 1 道第 0 种类型的题目、1 道第 1 种类型的题目和 1 道第 2 种类型的题目：1 + 2 + 3 = 6
- 解决 3 道第 1 种类型的题目：2 + 2 + 2 = 6
- 解决 2 道第 2 种类型的题目：3 + 3 = 6

示例 2：

```text
输入：target = 5, types = [[50,1],[50,2],[50,5]]
输出：4
解释：要获得 5 分，你可以选择以下四种方法之一：
```

- 解决 5 道第 0 种类型的题目：1 + 1 + 1 + 1 + 1 = 5
- 解决 3 道第 0 种类型的题目和 1 道第 1 种类型的题目：1 + 1 + 1 + 2 = 5
- 解决 1 道第 0 种类型的题目和 2 道第 1 种类型的题目：1 + 2 + 2 = 5
- 解决 1 道第 2 种类型的题目：5

示例 3：

```text
输入：target = 18, types = [[6,1],[3,2],[2,3]]
输出：1
解释：只有回答所有题目才能获得 18 分。
```

__提示：__

- `1 <= target <= 1000`
- `n == types.length`
- `1 <= n <= 50`
- `types[i].length == 2`
- `1 <= count_i, marks_i <= 50`

__思路:__

```text
动态规划
设 dp[i][j] 表示前 i 种题目, 恰好得到 j 分的方法数
初始化 dp[0][0] = 1, 其他 dp[0][j] = 0
dp[i][j] = dp[i - 1][j] + dp[i - 1][j - marks_i] + ... + dp[i - 1][j - k * marks_i], 其中 0 <= k <= count_i 且 j - k * marks_i >= 0 
由于 dp[i][j] 只与前一行有关, 所以可以使用一维数组 dp[j] 来代替二维数组 dp[i][j]
遍历 types, 对于每个 type, 从后向前遍历 j, 对于每个 j, 从 1 到 min(count_i, j // marks_i) 遍历 k, 更新 dp[j] = (dp[j] + dp[j - k * marks_i]) % (10 ^ 9 + 7)
最后返回 dp[target] 即可
时间复杂度为 O(MN), 空间复杂度为 O(N), 其中 M 为 types 第一维的总和, N 为 target 的值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int waysToReachTarget(int target, vector<vector<int>>& types) 
    {
        vector<int> dp(target + 1);
        dp.front() = 1;
        for (const auto& type : types) for (int count = type.front(), mark = type.back(), j = target; j > 0; j--) for (int k = 1; k <= min(count, j / mark); k++) dp[j] = (dp[j] + dp[j - k * mark]) % (int)(1e9 + 7);
        return dp.back()e;
    }
};
```

__Java__:

```Java
class Solution {
    public int waysToReachTarget(int target, int[][] types) {
        int dp[] = new int[target + 1], MOD = 1_000_000_007;
        dp[0] = 1;
        for (int[] type : types) for (int count = type[0], mark = type[1], j = target; j > 0; j--) for (int k = 1; k <= Math.min(count, j / mark); k++) dp[j] = (dp[j] + dp[j - k * mark]) % MOD;
        return dp[target];
    }
}
```

__Python__:

```Python
class Solution:
    def waysToReachTarget(self, target: int, types: List[List[int]]) -> int:
        dp, MOD = [1] + [0] * target, 10 ** 9 + 7
        for count, mark in types:
            for j in range(target, 0, -1):
                for k in range(1, min(count, j // mark) + 1):
                    dp[j] += dp[j - k * mark]
        return dp[-1] % MOD
```
