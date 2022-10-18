# 1349 Maximum Students Taking Exam 参加考试的最大学生数

__Description:__

Given a m * n matrix seats  that represent seats distributions in a classroom. If a seat is broken, it is denoted by '#' character otherwise it is denoted by a '.' character.

Students can see the answers of those sitting next to the left, right, upper left and upper right, but he cannot see the answers of the student sitting directly in front or behind him. Return the maximum number of students that can take the exam together without any cheating being possible..

Students must be placed in seats in good condition.

__Example:__

Example 1:

![Exam](https://assets.leetcode.com/uploads/2020/01/29/image.png)

Input: seats = [["#",".","#","#",".","#"],
                [".","#","#","#","#","."],
                ["#",".","#","#",".","#"]]
Output: 4
Explanation: Teacher can place 4 students in available seats so they don't cheat on the exam.

Example 2:

Input: seats = [[".","#"],
                ["#","#"],
                ["#","."],
                ["#","#"],
                [".","#"]]
Output: 3
Explanation: Place all students in available seats.

Example 3:

Input: seats = [["#",".",".",".","#"],
                [".","#",".","#","."],
                [".",".","#",".","."],
                [".","#",".","#","."],
                ["#",".",".",".","#"]]
Output: 10
Explanation: Place students in available seats in column 1, 3 and 5.

__Constraints:__

seats contains only characters '.' and'#'.
m == seats.length
n == seats[i].length
1 <= m <= 8
1 <= n <= 8

__题目描述：__

给你一个 m * n 的矩阵 seats 表示教室中的座位分布。如果座位是坏的（不可用），就用 '#' 表示；否则，用 '.' 表示。

学生可以看到左侧、右侧、左上、右上这四个方向上紧邻他的学生的答卷，但是看不到直接坐在他前面或者后面的学生的答卷。请你计算并返回该考场可以容纳的一起参加考试且无法作弊的最大学生人数。

学生必须坐在状况良好的座位上。

__示例：__

示例 1：

![考试](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/09/image.png)

输入：seats = [["#",".","#","#",".","#"],
              [".","#","#","#","#","."],
              ["#",".","#","#",".","#"]]
输出：4
解释：教师可以让 4 个学生坐在可用的座位上，这样他们就无法在考试中作弊。

示例 2：

输入：seats = [[".","#"],
              ["#","#"],
              ["#","."],
              ["#","#"],
              [".","#"]]
输出：3
解释：让所有学生坐在可用的座位上。

示例 3：

输入：seats = [["#",".",".",".","#"],
              [".","#",".","#","."],
              [".",".","#",".","."],
              [".","#",".","#","."],
              ["#",".",".",".","#"]]
输出：10
解释：让学生坐在第 1、3 和 5 列的可用座位上。

__提示：__

seats 只包含字符 '.' 和'#'
m == seats.length
n == seats[i].length
1 <= m <= 8
1 <= n <= 8

__思路：__

状态压缩 ➕ 动态规划
设 dp\[i][j] 表示前 i 行中第 i 行分布为 j 时的最大学生数, 其中 j 为二进制数, 其位为 1 表示有人, 否则表示没有人
动态转移方程为 dp\[i][j] = max(dp\[i][j], dp\[i - 1][k] + num), 其中 num 为该行的人的数量, 即 j 中 1 的次数
注意每次都需要检查位置是否合法
时间复杂度为 O(m \* 2 ^ n), 空间复杂度为 O(m \* 2 ^ n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int maxStudents(vector<vector<char>>& seats) 
    {
        int m = seats.size(), n = seats.front().size(), result = 0, pre = 0;
        vector<vector<int>> dp(m + 1, vector<int>(1 << n));
        for (int i = 1; i <= m; i++)
        {
            int cur = 0;
            for (int j = 0; j < n; j++) cur |= ((seats[i - 1][j] == '.') << j);
            for (int j = cur; j > 0; j = (j - 1) & cur)
            {
                if ((!(j & (j >> 1))) and (!(j & (j << 1))))
                {
                    for (int k = 0; k < (1 << n); k++)
                    {
                        if ((!(j & (k >> 1))) and (!(j & (k << 1))))
                        {
                            dp[i][j] = max(dp[i][j], dp[i - 1][k] + __builtin_popcount(j));
                            result = max(result, dp[i][j]);
                        }
                    }
                }
            }
            dp[i].front() = pre;
            pre = result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxStudents(char[][] seats) {
        int m = seats.length, n = seats[0].length, result = 0, pre = 0, dp[][] = new int[m + 1][1 << n];
        for (int i = 1; i <= m; i++) {
            int cur = 0;
            for (int j = 0; j < n; j++) cur |= ((seats[i - 1][j] == '.' ? 1 : 0) << j);
            for (int j = cur; j > 0; j = (j - 1) & cur) {
                if ((j & (j >> 1)) == 0 && (j & (j << 1)) == 0) {
                    int count = 0, s = j;
                    while (s != 0) {
                        count += ((s & 1) == 1 ? 1 : 0);
                        s >>= 1;
                    }
                    for (int k = 0; k < (1 << n); k++) {
                        if ((j & (k << 1)) == 0 && (j & (k >> 1)) == 0) {
                            dp[i][j] = Math.max(dp[i][j], dp[i - 1][k] + count);
                            result = Math.max(result, dp[i][j]);
                        }
                    }
                }
            }
            dp[i][0] = pre;
            pre = result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxStudents(self, seats: List[List[str]]) -> int:
        m, n, result, pre = len(seats), len(seats[0]), 0, 0
        dp = [[0] * (1 << n) for _ in range(m + 1)]
        for i in range(1, m + 1):
            cur = 0
            for j in range(n):
                cur |= ((seats[i - 1][j] == '.') << j)
            j = cur
            while j:
                if not (j & (j << 1)) and not (j & (j >> 1)):
                    for k in range(1 << n):
                        if not (j & (k << 1)) and not (j & (k >> 1)):
                            dp[i][j] = max(dp[i][j], dp[i - 1][k] + bin(j).count('1'))
                            result = max(result, dp[i][j])
                j = (j - 1) & cur
            dp[i][0] = pre
            pre = result
        return result
```
