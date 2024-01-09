# 1947 Maximum Compatibility Score Sum 最大兼容性评分和

__Description:__

There is a survey that consists of `n` questions where each question's answer is either `0` (no) or `1` (yes).

The survey was given to `m` students numbered from `0` to `m - 1` and `m` mentors numbered from `0` to `m - 1`. The answers of the students are represented by a 2D integer array `students` where `students[i]` is an integer array that contains the answers of the `i ^ th` student (__0-indexed__). The answers of the mentors are represented by a 2D integer array `mentors` where `mentors[j]` is an integer array that contains the answers of the `j ^ th` mentor (__0-indexed__).

Each student will be assigned to one mentor, and each mentor will have one student assigned to them. The compatibility score of a student-mentor pair is the number of answers that are the same for both the student and the mentor.

- For example, if the student's answers were `[1, 0, 1]` and the mentor's answers were `[0, 0, 1]`, then their compatibility score is 2 because only the second and the third answers are the same.

You are tasked with finding the optimal student-mentor pairings to maximize the sum of the compatibility scores.

Given `students` and `mentors`, return _the __maximum compatibility score sum__ that can be achieved._

__Example:__

Example 1:

```text
Input: students = [[1,1,0],[1,0,1],[0,0,1]], mentors = [[1,0,0],[0,0,1],[1,1,0]]
Output: 8
Explanation: We assign students to mentors in the following way:
```

- student 0 to mentor 2 with a compatibility score of 3.
- student 1 to mentor 0 with a compatibility score of 2.
- student 2 to mentor 1 with a compatibility score of 3.
The compatibility score sum is 3 + 2 + 3 = 8.

Example 2:

```text
Input: students = [[0,0],[0,0],[0,0]], mentors = [[1,1],[1,1],[1,1]]
Output: 0
Explanation: The compatibility score of any student-mentor pair is 0.
```

__Constraints:__

- `m == students.length == mentors.length`
- `n == students[i].length == mentors[j].length`
- `1 <= m, n <= 8`
- `students[i][k]` is either `0` or `1`.
- `mentors[j][k]` is either `0` or `1`.

__题目描述:__

有一份由 `n` 个问题组成的调查问卷，每个问题的答案要么是 `0`（no，否），要么是 `1`（yes，是）。

这份调查问卷被分发给 `m` 名学生和 `m` 名导师，学生和导师的编号都是从 `0` 到 `m - 1` 。学生的答案用一个二维整数数组 `students` 表示，其中 `students[i]` 是一个整数数组，包含第 `i` 名学生对调查问卷给出的答案（__下标从 0 开始__）。导师的答案用一个二维整数数组 `mentors` 表示，其中 `mentors[j]` 是一个整数数组，包含第 `j` 名导师对调查问卷给出的答案（__下标从 0 开始__）。

每个学生都会被分配给 一名 导师，而每位导师也会分配到 一名 学生。配对的学生与导师之间的兼容性评分等于学生和导师答案相同的次数。

- 例如，学生答案为 [1, ___0___, ___1___] 而导师答案为 [0, ___0___, ___1___] ，那么他们的兼容性评分为 2 ，因为只有第二个和第三个答案相同。

请你找出最优的学生与导师的配对方案，以 最大程度上 提高 兼容性评分和 。

给你 `students` 和 `mentors` ，返回可以得到的 __最大兼容性评分和__ 。

__示例:__

示例 1：

```text
输入：students = [[1,1,0],[1,0,1],[0,0,1]], mentors = [[1,0,0],[0,0,1],[1,1,0]]
输出：8
解释：按下述方式分配学生和导师：
```

- 学生 0 分配给导师 2 ，兼容性评分为 3 。
- 学生 1 分配给导师 0 ，兼容性评分为 2 。
- 学生 2 分配给导师 1 ，兼容性评分为 3 。
最大兼容性评分和为 3 + 2 + 3 = 8 。

示例 2：

```text
输入：students = [[0,0],[0,0],[0,0]], mentors = [[1,1],[1,1],[1,1]]
输出：0
解释：任意学生与导师配对的兼容性评分都是 0 。
```

__提示：__

- `m == students.length == mentors.length`
- `n == students[i].length == mentors[j].length`
- `1 <= m, n <= 8`
- `students[i][k]` 为 `0` 或 `1`
- `mentors[j][k]` 为 `0` 或 `1`

__思路:__

```text
状态压缩 ➕ 动态规划
预处理 points[i][j] 表示学生 i 和导师 j 的兼容性评分
dp[mask] 表示 mask 对应的学生和导师的最大兼容性评分和
dp[mask] = max(dp[mask ^ (1 << i)] + points[i][j]), 其中 mask ^ (1 << i) 是 mask 的一个合法子集
时间复杂度为 O(M ^ 2 * N + M * 2 ^ M), 空间复杂度为 O(2 ^ M)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxCompatibilitySum(vector<vector<int>>& students, vector<vector<int>>& mentors) 
    {
        int m = students.size(), n = students.front().size(), total = 1 << m;
        vector<vector<int>> points(m, vector<int>(m));
        vector<int> dp(total);
        for (int i = 0; i < m; i++) for (int j = 0; j < m; j++) for (int k = 0; k < n; k++) points[i][j] += (students[i][k] == mentors[j][k]);
        for (int mask = 1; mask < total; mask++) for (int i = 0, c = __builtin_popcount(mask); i < m; i++) if (mask & (1 << i)) dp[mask] = max(dp[mask], dp[mask ^ (1 << i)] + points[c - 1][i]);
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int maxCompatibilitySum(int[][] students, int[][] mentors) {
        int m = students.length, n = students[0].length, points[][] = new int[m][m], total = 1 << m, dp[] = new int[total];
        for (int i = 0; i < m; i++) for (int j = 0; j < m; j++) for (int k = 0; k < n; k++) points[i][j] += (students[i][k] == mentors[j][k] ? 1 : 0);
        for (int mask = 1; mask < total; mask++) for (int i = 0, c = helper(mask); i < m; i++) if ((mask & (1 << i)) != 0) dp[mask] = Math.max(dp[mask], dp[mask ^ (1 << i)] + points[c - 1][i]);
        return dp[total - 1];
    }

    private int helper(int n) {
        int result = 0;
        while (n != 0) {
            n &= (n - 1);
            ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxCompatibilitySum(self, students: List[List[int]], mentors: List[List[int]]) -> int:
        m, n = len(students), len(students[0])
        points = [[0] * m for _ in range(m)]
        for i in range(m):
            for j in range(m):
                for k in range(n):
                    points[i][j] += students[i][k] == mentors[j][k]
        
        dp = [0] * (1 << m)
        for i in range((1 << m) - 1):
            count = bin(i).count('1')
            for j in range(m):
                if not (i & (1 << j)):
                    dp[next] = max(dp[(next := (i ^ (1 << j)))], dp[i] + points[count][j])
        return dp[-1]
```
