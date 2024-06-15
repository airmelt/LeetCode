# 2140 Solving Questions With Brainpower 解决智力问题

__Description:__

You are given a __0-indexed__ 2D integer array `questions` where `questions[i] = [pointsi, brainpoweri]`.

The array describes the questions of an exam, where you have to process the questions __in order__ (i.e., starting from question `0`) and make a decision whether to __solve__ or __skip__ each question. Solving question `i` will __earn__ you `pointsi` points but you will be __unable__ to solve each of the next `brainpoweri` questions. If you skip question `i`, you get to make the decision on the next question.

- For example, given `questions = [[3, 2], [4, 3], [4, 4], [2, 5]]`:
  - If question `0` is solved, you will earn `3` points but you will be unable to solve questions `1` and `2`.
  - If instead, question `0` is skipped and question `1` is solved, you will earn `4` points but you will be unable to solve questions `2` and `3`.

Return the maximum points you can earn for the exam.

__Example:__

Example 1:

```text
Input: questions = [[3,2],[4,3],[4,4],[2,5]]
Output: 5
Explanation: The maximum points can be earned by solving questions 0 and 3.
```

- Solve question 0: Earn 3 points, will be unable to solve the next 2 questions
- Unable to solve questions 1 and 2
- Solve question 3: Earn 2 points

Total points earned: 3 + 2 = 5. There is no other way to earn 5 or more points.

Example 2:

```text
Input: questions = [[1,1],[2,2],[3,3],[4,4],[5,5]]
Output: 7
Explanation: The maximum points can be earned by solving questions 1 and 4.
```

- Skip question 0
- Solve question 1: Earn 2 points, will be unable to solve the next 2 questions
- Unable to solve questions 2 and 3
- Solve question 4: Earn 5 points

Total points earned: 2 + 5 = 7. There is no other way to earn 7 or more points.

__Constraints:__

- `1 <= questions.length <= 10 ^ 5`
- `questions[i].length == 2`
- `1 <= pointsi, brainpoweri <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始的二维整数数组 `questions` ，其中 `questions[i] = [pointsi, brainpoweri]` 。

这个数组表示一场考试里的一系列题目，你需要 __按顺序__ （也就是从问题 `0` 开始依次解决），针对每个问题选择 __解决__ 或者 __跳过__ 操作。解决问题 `i` 将让你 _获得_  `pointsi` 的分数，但是你将 __无法__ 解决接下来的 `brainpoweri` 个问题（即只能跳过接下来的 `brainpoweri` 个问题）。如果你跳过问题 `i` ，你可以对下一个问题决定使用哪种操作。

- 比方说，给你 `questions = [[3, 2], [4, 3], [4, 4], [2, 5]]` :
  - 如果问题 `0` 被解决了， 那么你可以获得 `3` 分，但你不能解决问题 `1` 和 `2` 。
  - 如果你跳过问题 `0` ，且解决问题 `1` ，你将获得 `4` 分但是不能解决问题 `2` 和 `3` 。

请你返回这场考试里你能获得的 最高 分数。

__示例:__

示例 1：

```text
输入：questions = [[3,2],[4,3],[4,4],[2,5]]
输出：5
解释：解决问题 0 和 3 得到最高分。
```

- 解决问题 0 ：获得 3 分，但接下来 2 个问题都不能解决。
- 不能解决问题 1 和 2
- 解决问题 3 ：获得 2 分

总得分为：3 + 2 = 5 。没有别的办法获得 5 分或者多于 5 分。

示例 2：

```text
输入：questions = [[1,1],[2,2],[3,3],[4,4],[5,5]]
输出：7
解释：解决问题 1 和 4 得到最高分。
```

- 跳过问题 0
- 解决问题 1 ：获得 2 分，但接下来 2 个问题都不能解决。
- 不能解决问题 2 和 3
- 解决问题 4 ：获得 5 分

总得分为：2 + 5 = 7 。没有别的办法获得 7 分或者多于 7 分。

__提示：__

- `1 <= questions.length <= 10 ^ 5`
- `questions[i].length == 2`
- `1 <= pointsi, brainpoweri <= 10 ^ 5`

__思路:__

```text
动态规划
dp[i] 表示解决前 i 个问题能获得的最高分数
如果不选 i, dp[i + 1] = max(dp[i], dp[i + 1])
如果选 i, 令 j = min(n, questions[i][1] + i + 1), 因为不能选接下来的 brainpower 个问题, 其中 brainpower = questions[i][1]
dp[j] = max(dp[j], dp[i] + questions[i][0])
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long mostPoints(vector<vector<int>>& questions) 
    {
        vector<long long> dp(questions.size() + 1);
        for (int i = 0, j = 0, n = questions.size(); i < n; i++)
        {
            dp[i + 1] = max(dp[i], dp[i + 1]);
            dp[j] = max(dp[j = min(n, questions[i].back() + i + 1)], questions[i].front() + dp[i]);
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public long mostPoints(int[][] questions) {
        long[] dp = new long[questions.length + 1];
        for (int i = 0, j = 0, n = questions.length; i < n; i++) {
            dp[i + 1] = Math.max(dp[i + 1], dp[i]);
            dp[j = Math.min(n, questions[i][1] + i + 1)] = Math.max(dp[j], dp[i] + questions[i][0]);
        }
        return dp[questions.length];
    }
}
```

__Python__:

```Python
class Solution:
    def mostPoints(self, questions: List[List[int]]) -> int:
        dp = [0] * ((n := len(questions)) + 1)
        for i, (point, brainpower) in enumerate(questions):
            dp[i + 1] = max(dp[i + 1], dp[i])
            dp[j] = max(dp[j := min(n, brainpower + i + 1)], dp[i] + point)
        return dp[n]
```
