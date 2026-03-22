# 2127 Maximum Employees to Be Invited to a Meeting 参加会议的最多员工数

__Description:__

A company is organizing a meeting and has a list of `n` employees, waiting to be invited. They have arranged for a large __circular__ table, capable of seating __any number__ of employees.

The employees are numbered from `0` to `n - 1`. Each employee has a __favorite__ person and they will attend the meeting __only if__ they can sit next to their favorite person at the table. The favorite person of an employee is __not__ themself.

Given a __0-indexed__ integer array `favorite`, where `favorite[i]` denotes the favorite person of the `i ^ th` employee, return _the __maximum number of employees__ that can be invited to the meeting_.

__Example:__

Example 1:

![2127-1](https://assets.leetcode.com/uploads/2021/12/14/ex1.png)

```text
Input: favorite = [2,2,1,2]
Output: 3
Explanation:
The above figure shows how the company can invite employees 0, 1, and 2, and seat them at the round table.
All employees cannot be invited because employee 2 cannot sit beside employees 0, 1, and 3, simultaneously.
Note that the company can also invite employees 1, 2, and 3, and give them their desired seats.
The maximum number of employees that can be invited to the meeting is 3.
```

Example 2:

```text
Input: favorite = [1,2,0]
Output: 3
Explanation: 
Each employee is the favorite person of at least one other employee, and the only way the company can invite them is if they invite every employee.
The seating arrangement will be the same as that in the figure given in example 1:
```

- Employee 0 will sit between employees 2 and 1.
- Employee 1 will sit between employees 0 and 2.
- Employee 2 will sit between employees 1 and 0.

The maximum number of employees that can be invited to the meeting is 3.

Example 3:

![2127-2](https://assets.leetcode.com/uploads/2021/12/14/ex2.png)

```text
Input: favorite = [3,0,1,4,1]
Output: 4
Explanation:
The above figure shows how the company will invite employees 0, 1, 3, and 4, and seat them at the round table.
Employee 2 cannot be invited because the two spots next to their favorite employee 1 are taken.
So the company leaves them out of the meeting.
The maximum number of employees that can be invited to the meeting is 4.
```

__Constraints:__

- `n == favorite.length`
- `2 <= n <= 10 ^ 5`
- `0 <= favorite[i] <= n - 1`
- `favorite[i] != i`

__题目描述:__

一个公司准备组织一场会议，邀请名单上有 `n` 位员工。公司准备了一张 __圆形__ 的桌子，可以坐下 __任意数目__ 的员工。

员工编号为 `0` 到 `n - 1` 。每位员工都有一位 __喜欢__ 的员工，每位员工 __当且仅当__ 他被安排在喜欢员工的旁边，他才会参加会议。每位员工喜欢的员工 __不会__ 是他自己。

给你一个下标从 __0__ 开始的整数数组 `favorite` ，其中 `favorite[i]` 表示第 `i` 位员工喜欢的员工。请你返回参加会议的 __最多员工数目__ 。

__示例:__

示例 1：

![2127-3](https://assets.leetcode.com/uploads/2021/12/14/ex1.png)

```text
输入：favorite = [2,2,1,2]
输出：3
解释：
上图展示了公司邀请员工 0，1 和 2 参加会议以及他们在圆桌上的座位。
没办法邀请所有员工参与会议，因为员工 2 没办法同时坐在 0，1 和 3 员工的旁边。
注意，公司也可以邀请员工 1，2 和 3 参加会议。
所以最多参加会议的员工数目为 3 。
```

示例 2：

```text
输入：favorite = [1,2,0]
输出：3
解释：
每个员工都至少是另一个员工喜欢的员工。所以公司邀请他们所有人参加会议的前提是所有人都参加了会议。
座位安排同图 1 所示：
```

- 员工 0 坐在员工 2 和 1 之间。
- 员工 1 坐在员工 0 和 2 之间。
- 员工 2 坐在员工 1 和 0 之间。

参与会议的最多员工数目为 3 。

示例 3：

![2127-4](https://assets.leetcode.com/uploads/2021/12/14/ex2.png)

```text
输入：favorite = [3,0,1,4,1]
输出：4
解释：
上图展示了公司可以邀请员工 0，1，3 和 4 参加会议以及他们在圆桌上的座位。
员工 2 无法参加，因为他喜欢的员工 1 旁边的座位已经被占领了。
所以公司只能不邀请员工 2 。
参加会议的最多员工数目为 4 。
```

__提示：__

- `n == favorite.length`
- `2 <= n <= 10 ^ 5`
- `0 <= favorite[i] <= n - 1`
- `favorite[i] != i`

__思路:__

```text
内向基环树
将 i 节点指向 favorite[i]
要么大于 2 个人构成一个环
要么两个人构成一个环, 且可以从两端延伸出两条链
拓扑排序的同时记录下能够延伸的链的长度
最后计算每个环的长度
如果为 2 可以加入链
否则自己成环, 记录环的最大长度
返回链或者环的最大值
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumInvitations(vector<int>& favorite) 
    {
        int n = favorite.size(), ring = 0, total = 0;
        vector<int> indegree(n), max_depth(n, 1);
        for (int f : favorite) ++indegree[f];
        queue<int> q;
        for (int i = 0; i < n; i++) if (!indegree[i]) q.push(i);
        while (!q.empty()) 
        {
            int x = q.front(), y = favorite[x];
            q.pop();
            max_depth[y] = max_depth[x] + 1;
            --indegree[y];
            if (!indegree[y]) q.push(y);
        }
        for (int i = 0; i < n; i++) 
        {
            if (indegree[i]) 
            {
                indegree[i] = 0;
                int cur = 1, x = favorite[i];
                while (x != i) 
                {
                    indegree[x] = 0;
                    ++cur;
                    x = favorite[x];
                }
                if (cur == 2) total += max_depth[i] + max_depth[favorite[i]];
                else ring = max(ring, cur);
            }
        }
        return max(ring, total);
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumInvitations(int[] favorite) {
        int n = favorite.length, indegree[] = new int[n], maxDepth[] = new int[n], ring = 0, total = 0;
        Arrays.fill(maxDepth, 1);
        for (int f : favorite) ++indegree[f];
        Deque<Integer> q = new ArrayDeque<>();
        for (int i = 0; i < n; i++) if (indegree[i] == 0) q.add(i);
        while (!q.isEmpty()) {
            int x = q.pollFirst(), y = favorite[x];
            maxDepth[y] = maxDepth[x] + 1;
            --indegree[y];
            if (indegree[y] == 0) q.add(y);
        }
        for (int i = 0; i < n; i++) {
            if (indegree[i] != 0) {
                indegree[i] = 0;
                int cur = 1, x = favorite[i];
                while (x != i) {
                    indegree[x] = 0;
                    ++cur;
                    x = favorite[x];
                }
                if (cur == 2) total += maxDepth[i] + maxDepth[favorite[i]];
                else ring = Math.max(ring, cur);
            }
        }
        return Math.max(ring, total);
    }
}
```

__Python__:

```Python
class Solution:
    def maximumInvitations(self, favorite: List[int]) -> int:
        indegree, max_depth, ring, total = [0] * (n := len(favorite)), [1] * n, 0, 0
        for f in favorite:
            indegree[f] += 1
        q = deque(i for i in range(n) if not indegree[i])
        while q:
            max_depth[y := favorite[x]] = max_depth[x := q.popleft()] + 1
            indegree[y] -= 1
            if not indegree[y]:
                q.append(y)
        for i, d in enumerate(indegree):
            if d:
                indegree[i], cur, x = 0, 1, favorite[i]
                while x != i:
                    indegree[x] = 0
                    cur += 1
                    x = favorite[x]
                if cur == 2:
                    total += max_depth[i] + max_depth[favorite[i]]
                else:
                    ring = max(ring, cur)
        return max(ring, total)
```
