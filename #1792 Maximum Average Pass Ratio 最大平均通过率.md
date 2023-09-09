# 1792 Maximum Average Pass Ratio 最大平均通过率

__Description:__

There is a school that has classes of students and each class will be having a final exam. You are given a 2D integer array `classes`, where `classes[i] = [passi, totali]`. You know beforehand that in the `i ^ th` class, there are `totali` total students, but only `passi` number of students will pass the exam.

You are also given an integer `extraStudents`. There are another `extraStudents` brilliant students that are __guaranteed__ to pass the exam of any class they are assigned to. You want to assign each of the `extraStudents` students to a class in a way that __maximizes__ the __average__ pass ratio across __all__ the classes.

The pass ratio of a class is equal to the number of students of the class that will pass the exam divided by the total number of students of the class. The average pass ratio is the sum of pass ratios of all the classes divided by the number of the classes.

Return _the __maximum__ possible average pass ratio after assigning the_ `extraStudents` _students._ Answers within `10 ^ -5` of the actual answer will be accepted.

__Example:__

Example 1:

```text
Input:  classes = [[1,2],[3,5],[2,2]], extraStudents = 2
Output:  0.78333
Explanation:  You can assign the two extra students to the first class. The average pass ratio will be equal to (3/4 + 3/5 + 2/2) / 3 = 0.78333.
```

Example 2:

```text
Input:  classes = [[2,4],[3,9],[4,5],[2,10]], extraStudents = 4
Output:  0.53485
```

__Constraints:__

- `1 <= classes.length <= 10 ^ 5`
- `classes[i].length == 2`
- `1 <= passi <= totali <= 10 ^ 5`
- `1 <= extraStudents <= 10 ^ 5`

__题目描述:__

一所学校里有一些班级，每个班级里有一些学生，现在每个班都会进行一场期末考试。给你一个二维数组 `classes` ，其中 `classes[i] = [passi, totali]` ，表示你提前知道了第 `i` 个班级总共有 `totali` 个学生，其中只有 `passi` 个学生可以通过考试。

给你一个整数 `extraStudents` ，表示额外有 `extraStudents` 个聪明的学生，他们 __一定__ 能通过任何班级的期末考。你需要给这 `extraStudents` 个学生每人都安排一个班级，使得 __所有__ 班级的 __平均__ 通过率 __最大__ 。

一个班级的 通过率 等于这个班级通过考试的学生人数除以这个班级的总人数。平均通过率 是所有班级的通过率之和除以班级数目。

请你返回在安排这 `<span>extraStudents</span>` 个学生去对应班级后的 __最大__ 平均通过率。与标准答案误差范围在 `10 ^ -5` 以内的结果都会视为正确结果。

__示例:__

示例 1：

```text
输入: classes = [[1,2],[3,5],[2,2]], extraStudents = 2
输出: 0.78333
解释: 你可以将额外的两个学生都安排到第一个班级，平均通过率为 (3/4 + 3/5 + 2/2) / 3 = 0.78333 。
```

示例 2：

```text
输入: classes = [[2,4],[3,9],[4,5],[2,10]], `extraStudents` = 4
输出: 0.53485
```

__提示：__

- `1 <= classes.length <= 10 ^ 5`
- `classes[i].length == 2`
- `1 <= passi <= totali <= 10 ^ 5`
- `1 <= extraStudents <= 10 ^ 5`

__思路:__

```text
优先队列
每次选取增加后平均通过率最大的班级
即选取 (a + 1) / (b + 1) - a / b 最大的班级
使用优先队列记录每个班级的 (a + 1) / (b + 1) - a / b, a, b
将每一个人分配到最大的班级中
最后统计平均通过率
时间复杂度为 O(N + MlogN), 空间复杂度为 O(N), M 为 extraStudents
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    double maxAverageRatio(vector<vector<int>>& classes, int extraStudents) 
    {
        priority_queue<tuple<double, int, int>> pq;
        for (const auto& c : classes) pq.push({(double)(c[0] + 1) / (c[1] + 1) - (double)c[0] / c[1], c[0], c[1]});
        while (extraStudents--) 
        {
            auto [_, a, b] = pq.top();
            pq.pop();
            pq.push({(double)(a + 2) / (b + 2) - (double)(a + 1) / (b + 1), a + 1, b + 1});
        }
        double result = 0;
        while (!pq.empty()) 
        {
            auto [_, a, b] = pq.top();
            pq.pop();
            result += (double)a / b;
        }
        return result / classes.size();
    }
};
```

__Java__:

```Java
class Solution {
    public double maxAverageRatio(int[][] classes, int extraStudents) {
        double result = 0;
        PriorityQueue<double[]> pq = new PriorityQueue<double[]>((a, b) -> { return Double.compare((b[0] + 1) / (b[1] + 1) - b[0] / b[1], (a[0] + 1) / (a[1] + 1) - a[0] / a[1]); });
        for (int[] c : classes) pq.offer(new double[]{c[0], c[1]});
        while (extraStudents-- > 0) {
            double c[] = pq.poll(), a = c[0] + 1, b = c[1] + 1;
            pq.offer(new double[]{a, b});
        }
        while (!pq.isEmpty()) {
            double[] arr = pq.poll();
            result += arr[0] / arr[1];
        }
        return result / classes.length;
    }
}
```

__Python__:

```Python
class Solution:
    def maxAverageRatio(self, classes: List[List[int]], extraStudents: int) -> float:
        rank, n = [[(c[0] - c[1]) / (c[1] ** 2 + c[1])] + c for c in classes], len(classes)
        heapify(rank)
        for _ in range(extraStudents):
            c = heappop(rank)
            a, b = c[1] + 1, c[2] + 1
            heappush(rank, [(a - b) / (b ** 2 + b), a, b])
        return sum(r[1] / r[2] for r in rank) / n
```
