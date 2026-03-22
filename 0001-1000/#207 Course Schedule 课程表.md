# 207 Course Schedule 课程表

__Description__:
There are a total of numCourses courses you have to take, labeled from 0 to numCourses-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

__Example:__

Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take.
             To take course 1 you should have finished course 0. So it is possible.

Example 2:

Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take.
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.

__Constraints:__

The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.
You may assume that there are no duplicate edges in the input prerequisites.
1 <= numCourses <= 10^5

__题目描述__:
你这个学期必须选修 numCourse 门课程，记为 0 到 numCourse-1 。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们：[0,1]

给定课程总量以及它们的先决条件，请你判断是否可能完成所有课程的学习？

__示例 :__

示例 1:

输入: 2, [[1,0]]
输出: true
解释: 总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。所以这是可能的。

示例 2:

输入: 2, [[1,0],[0,1]]
输出: false
解释: 总共有 2 门课程。学习课程 1 之前，你需要先完成​课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。

__提示：__

输入的先决条件是由 边缘列表 表示的图形，而不是 邻接矩阵 。详情请参见图的表示法。
你可以假定输入的先决条件中没有重复的边。
1 <= numCourses <= 10^5

__思路__:

拓扑排序
先将入度为 0的顶点加入一个处理队列
每次取出入度为 0的顶点, 将这个顶点的所有邻接点入度 - 1, 再将入度为 0的点加入队列
比较课程数与处理的顶点数是否相等
时间复杂度O(n + m), 空间复杂度O(n + m), 其中 n为课程数, m为先修课程数

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) 
    {
        if (prerequisites.empty()) return true;
        map<int, vector<int>> adjcent;
        vector<int> indegree(numCourses, 0);
        queue<int> q;
        int count = 0;
        for (auto& course : prerequisites)
        {
            adjcent[course[1]].push_back(course[0]);
            ++indegree[course[0]];
        }
        for (int i = 0; i < numCourses; i++) if (!indegree[i]) q.push(i);
        while (q.size())
        {
            auto cur = q.front();
            q.pop();
            ++count;
            for (auto first : adjcent[cur])
            {
                --indegree[first];
                if (!indegree[first]) q.push(first);
            }
        }
        return count == numCourses;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        if (prerequisites == null || prerequisites.length == 0) return true;
        int count = 0, indrgree[] = new int[numCourses];
        List<List<Integer>> adjcent = new ArrayList<>(numCourses);
        for (int i = 0; i < numCourses; i++) adjcent.add(new ArrayList<Integer>());
        for (int i = 0; i < prerequisites.length; i++) {
            ++indrgree[prerequisites[i][1]];
            adjcent.get(prerequisites[i][0]).add(prerequisites[i][1]);
        }
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) if (indrgree[i] == 0) queue.offer(i);
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            ++count;
            for (int first : adjcent.get(cur)) {
                --indrgree[first];
                if (indrgree[first] == 0) queue.offer(first);
            }
        }
        return count == numCourses;
    }
}
```

__Python__:

```Python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        if not prerequisites:
            return True
        indegree, adjcent = [0] * numCourses, [set() for _ in range(numCourses)]
        for second, first in prerequisites:
            indegree[second] += 1
            adjcent[first].add(second)
        count, queue = 0, [i for i in range(numCourses) if not indegree[i]]
        while queue:
            cur = queue.pop(0)
            count += 1
            for first in adjcent[cur]:
                indegree[first] -= 1
                if not indegree[first]:
                    queue.append(first)
        return count == numCourses
```
