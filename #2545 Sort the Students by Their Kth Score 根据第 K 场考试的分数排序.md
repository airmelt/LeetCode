# 2545 Sort the Students by Their Kth Score 根据第 K 场考试的分数排序

__Description:__

There is a class with `m` students and `n` exams. You are given a __0-indexed__ `m x n` integer matrix `score`, where each row represents one student and `score[i][j]` denotes the score the `i ^ th` student got in the `j ^ th` exam. The matrix `score` contains __distinct__ integers only.

You are also given an integer `k`. Sort the students (i.e., the rows of the matrix) by their scores in the `k ^ th` (__0-indexed__) exam from the highest to the lowest.

Return the matrix after sorting it.

__Example:__

Example 1:

![2545-1](https://assets.leetcode.com/uploads/2022/11/30/example1.png)

```text
Input: score = [[10,6,9,1],[7,5,11,2],[4,8,3,15]], k = 2
Output: [[7,5,11,2],[10,6,9,1],[4,8,3,15]]
Explanation: In the above diagram, S denotes the student, while E denotes the exam.
```

- The student with index 1 scored 11 in exam 2, which is the highest score, so they got first place.
- The student with index 0 scored 9 in exam 2, which is the second highest score, so they got second place.
- The student with index 2 scored 3 in exam 2, which is the lowest score, so they got third place.

Example 2:

![2545-2](https://assets.leetcode.com/uploads/2022/11/30/example2.png)

```text
Input: score = [[3,4],[5,6]], k = 0
Output: [[5,6],[3,4]]
Explanation: In the above diagram, S denotes the student, while E denotes the exam.
```

- The student with index 1 scored 5 in exam 0, which is the highest score, so they got first place.
- The student with index 0 scored 3 in exam 0, which is the lowest score, so they got second place.

__Constraints:__

- `m == score.length`
- `n == score[i].length`
- `1 <= m, n <= 250`
- `1 <= score[i][j] <= 10 ^ 5`
- `score` consists of __distinct__ integers.
- `0 <= k < n`

__题目描述:__

班里有 `m` 位学生，共计划组织 `n` 场考试。给你一个下标从 __0__ 开始、大小为 `m x n` 的整数矩阵 `score` ，其中每一行对应一位学生，而 `score[i][j]` 表示第 `i` 位学生在第 `j` 场考试取得的分数。矩阵 `score` 包含的整数 __互不相同__ 。

另给你一个整数 `k` 。请你按第 `k` 场考试分数从高到低完成对这些学生（矩阵中的行）的排序。

返回排序后的矩阵。

__示例:__

示例 1：

![2545-3](https://assets.leetcode.com/uploads/2022/11/30/example1.png)

```text
输入：score = [[10,6,9,1],[7,5,11,2],[4,8,3,15]], k = 2
输出：[[7,5,11,2],[10,6,9,1],[4,8,3,15]]
解释：在上图中，S 表示学生，E 表示考试。
```

- 下标为 1 的学生在第 2 场考试取得的分数为 11 ，这是考试的最高分，所以 TA 需要排在第一。
- 下标为 0 的学生在第 2 场考试取得的分数为 9 ，这是考试的第二高分，所以 TA 需要排在第二。
- 下标为 2 的学生在第 2 场考试取得的分数为 3 ，这是考试的最低分，所以 TA 需要排在第三。

示例 2：

![2545-4](https://assets.leetcode.com/uploads/2022/11/30/example2.png)

```text
输入：score = [[3,4],[5,6]], k = 0
输出：[[5,6],[3,4]]
解释：在上图中，S 表示学生，E 表示考试。
```

- 下标为 1 的学生在第 0 场考试取得的分数为 5 ，这是考试的最高分，所以 TA 需要排在第一。
- 下标为 0 的学生在第 0 场考试取得的分数为 3 ，这是考试的最低分，所以 TA 需要排在第二。

__提示：__

- `m == score.length`
- `n == score[i].length`
- `1 <= m, n <= 250`
- `1 <= score[i][j] <= 10 ^ 5`
- `score` 由 __不同__ 的整数组成
- `0 <= k < n`

__思路:__

```text
排序
自定义排序
将第 k 场考试的分数作为排序依据
时间复杂度为 O(MlogM), 空间复杂度为 O(logM)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> sortTheStudents(vector<vector<int>>& score, int k) 
    {
        sort(score.begin(), score.end(), [&](const auto a, const auto b) { return a[k] > b[k]; });
        return score;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] sortTheStudents(int[][] score, int k) {
        Arrays.sort(score, (a, b) -> b[k] - a[k]);
        return score;
    }
}
```

__Python__:

```Python
class Solution:
    def sortTheStudents(self, score: List[List[int]], k: int) -> List[List[int]]:
        return sorted(score, key=lambda s: -s[k])
```
