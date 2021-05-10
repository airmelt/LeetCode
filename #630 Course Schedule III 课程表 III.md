# 630 Course Schedule III 课程表 III

__Description__:
There are n different online courses numbered from 1 to n. You are given an array courses where courses[i] = [durationi, lastDayi] indicate that the ith course should be taken continuously for durationi days and must be finished before or on lastDayi.

You will start on the 1st day and you cannot take two or more courses simultaneously.

Return the maximum number of courses that you can take.

__Example:__

Example 1:

Input: courses = [[100,200],[200,1300],[1000,1250],[2000,3200]]
Output: 3
Explanation:
There are totally 4 courses, but you can take 3 courses at most:
First, take the 1st course, it costs 100 days so you will finish it on the 100th day, and ready to take the next course on the 101st day.
Second, take the 3rd course, it costs 1000 days so you will finish it on the 1100th day, and ready to take the next course on the 1101st day.
Third, take the 2nd course, it costs 200 days so you will finish it on the 1300th day.
The 4th course cannot be taken now, since you will finish it on the 3300th day, which exceeds the closed date.

Example 2:

Input: courses = [[1,2]]
Output: 1

Example 3:

Input: courses = [[3,2],[4,3]]
Output: 0

__Constraints:__

1 <= courses.length <= 10^4
1 <= durationi, lastDayi <= 10^4

__题目描述__:
这里有 n 门不同的在线课程，他们按从 1 到 n 编号。每一门课程有一定的持续上课时间（课程时间）t 以及关闭时间第 d 天。一门课要持续学习 t 天直到第 d 天时要完成，你将会从第 1 天开始。

给出 n 个在线课程用 (t, d) 对表示。你的任务是找出最多可以修几门课。

__示例 :__

输入: [[100, 200], [200, 1300], [1000, 1250], [2000, 3200]]
输出: 3
解释:
这里一共有 4 门课程, 但是你最多可以修 3 门:
首先, 修第一门课时, 它要耗费 100 天，你会在第 100 天完成, 在第 101 天准备下门课。
第二, 修第三门课时, 它会耗费 1000 天，所以你将在第 1100 天的时候完成它, 以及在第 1101 天开始准备下门课程。
第三, 修第二门课时, 它会耗时 200 天，所以你将会在第 1300 天时完成它。
第四门课现在不能修，因为你将会在第 3300 天完成它，这已经超出了关闭日期。

__提示:__

整数 1 <= d, t, n <= 10,000 。
你不能同时修两门课程。

__思路__:

贪心
类似会议安排, 先按照结束时间排序
先假定当前课程可以加入, 直接加入堆, 如果累计时间晚于结束时间, 就弹出堆顶, 并从累计时间减去堆顶的时间
堆中的课程就是能组成课程表的课程, 返回堆的大小即可
时间复杂度 O(nlgn), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int scheduleCourse(vector<vector<int>>& courses) 
    {
        sort(courses.begin(), courses.end(), [](const auto &a, const auto &b) { return a.back() < b.back(); });
        int day = 0;
        priority_queue<int> pq;
        for (auto &course : courses)
        {
            day += course.front();
            pq.emplace(course.front());
            if (day > course.back())
            {
                day -= pq.top();
                pq.pop();
            }
        }
        return pq.size();
    }
};
```

__Java__:

```Java
class Solution {
    public int scheduleCourse(int[][] courses) {
        int day = 0;
        Arrays.sort(courses, (a, b) -> (a[1] - b[1]));
        Queue<Integer> queue = new PriorityQueue<>();
        for (int[] course : courses) {
            day += course[0];
            queue.add(-course[0]);
            if (day > course[1]) day += queue.poll();
        }
        return queue.size();
    }
}
```

__Python__:

```Python
class Solution:
    def scheduleCourse(self, courses: List[List[int]]) -> int:
        courses.sort(key=lambda x: x[1])
        day, heap = 0, []
        for course in courses:
            day += course[0]
            heapq.heappush(heap, -course[0])
            if day > course[1]:
                day += heapq.heappop(heap)
        return len(heap)
```
