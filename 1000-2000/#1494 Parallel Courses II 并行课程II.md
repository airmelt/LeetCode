# 1494 Parallel Courses II 并行课程II

__Description:__

You are given an integer `n`, which indicates that there are `n` courses labeled from `1` to `n`. You are also given an array `relations` where `relations[i] = [prevCoursei, nextCoursei]`, representing a prerequisite relationship between course `prevCoursei` and course `nextCoursei`: course `prevCoursei` has to be taken before course `nextCoursei`. Also, you are given the integer `k`.

In one semester, you can take __at most__ `k` courses as long as you have taken all the prerequisites in the __previous__ semesters for the courses you are taking.

Return the minimum number of semesters needed to take all courses. The testcases will be generated such that it is possible to take every course.

__Example:__

Example 1:

![1494-1](https://assets.leetcode.com/uploads/2020/05/22/leetcode_parallel_courses_1.png)

```text
Input: n = 4, relations = [[2,1],[3,1],[1,4]], k = 2
Output: 3
Explanation: The figure above represents the given graph.
In the first semester, you can take courses 2 and 3.
In the second semester, you can take course 1.
In the third semester, you can take course 4.
```

Example 2:

![1494-2](https://assets.leetcode.com/uploads/2020/05/22/leetcode_parallel_courses_2.png)

```text
Input: n = 5, relations = [[2,1],[3,1],[4,1],[1,5]], k = 2
Output: 4
Explanation: The figure above represents the given graph.
In the first semester, you can only take courses 2 and 3 since you cannot take more than two per semester.
In the second semester, you can take course 4.
In the third semester, you can take course 1.
In the fourth semester, you can take course 5.
```

__Constraints:__

- `1 <= n <= 15`
- `1 <= k <= n`
- `0 <= relations.length <= n * (n-1) / 2`
- `relations[i].length == 2`
- `1 <= prevCoursei, nextCoursei <= n`
- `prevCoursei != nextCoursei`
- All the pairs `[prevCoursei, nextCoursei]` are __unique__.
- The given graph is a directed acyclic graph.

__题目描述:__

给你一个整数 `n` 表示某所大学里课程的数目，编号为 `1` 到 `n` ，数组 `relations` 中， `relations[i] = [xi, yi]` 表示一个先修课的关系，也就是课程 `xi` 必须在课程 `yi` 之前上。同时你还有一个整数 `k` 。

在一个学期中，你 __最多__ 可以同时上 `k` 门课，前提是这些课的先修课在之前的学期里已经上过了。

请你返回上完所有课最少需要多少个学期。题目保证一定存在一种上完所有课的方式。

__示例:__

示例 1：

![1494-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/06/27/leetcode_parallel_courses_1.png)

```text
输入：n = 4, relations = [[2,1],[3,1],[1,4]], k = 2
输出：3 
解释：上图展示了题目输入的图。在第一个学期中，我们可以上课程 2 和课程 3 。然后第二个学期上课程 1 ，第三个学期上课程 4 。
```

示例 2：

![1494-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/06/27/leetcode_parallel_courses_2.png)

```text
输入：n = 5, relations = [[2,1],[3,1],[4,1],[1,5]], k = 2
输出：4 
解释：上图展示了题目输入的图。一个最优方案是：第一学期上课程 2 和 3，第二学期上课程 4 ，第三学期上课程 1 ，第四学期上课程 5 。
```

示例 3：

```text
输入：n = 11, relations = [], k = 2
输出：6
```

__提示：__

- `1 <= n <= 15`
- `1 <= k <= n`
- `0 <= relations.length <= n * (n-1) / 2`
- `relations[i].length == 2`
- `1 <= xi, yi <= n`
- `xi != yi`
- 所有先修关系都是不同的，也就是说 `relations[i] != relations[j]` 。
- 题目输入的图是个有向无环图。

__思路:__

```text
动态规划 ➕ 状态压缩
注意到 n 小于 16, 提示使用状态压缩解决
1. 求所有数字的 1 的个数(数位 DP)
count[i] = count[1 >> i] + (i & 1)
也可以使用 __builtin_popcount()(C++) 或者 bin().count('1')(Python) 或者 Integer.bitCount() 计算
2. 计算子集
for (int sub = state; sub; sub = (sub - 1) & state)
3. 计算 lowbit
lowbit = a & -a
如 a = 0b001010100, a & -a = 0b100
原理是补码
4. 判断子集
x & y == x
5. 去重
a &= ~b
如 a = 0b1100, b = 0b1101, ~b = -0b1110, a & ~b = 0b0
先记录前置课程, 由于课程编号从 1 开始, 可以减小 1 记录
用一个 int 型就能存下前置情况
当前学过的课程记录为 state
先判断下一个可以学的课程列表 next
通过与 pre 数组判断是否为子集来判断是否能够学习第 i 个课程
这里可以使用去重, 因为已经学过的课程可以不必再学习
最后查询 next 的全部子集
如果学习的课程不大于 k, 则可以尝试进行学习
设 dp[i] 表示学习课程为 i 时, 学习的天数
初始化 dp[i] = n, dp[0] = 0, 学习 0 门课程需要 0 天, 学习 i 门课程最多不会超过 n 天
dp[state | sub] = min(dp[state | sub], dp[state] + 1)
要么不用这个状态学习, 要么使用这个状态学习需要 1 天时间
最后返回 dp[(1 << n) - 1] 即可
时间复杂度为 O(2 ** N), 空间复杂度为 O(2 ** N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minNumberOfSemesters(int n, vector<vector<int>>& relations, int k) 
    {
        vector<int> pre(n), dp(1 << n, n);
        dp.front() = 0;
        for (auto& r: relations) pre[--r.back()] |= (1 << --r.front());
        for (int state = 0, total = (1 << n); state < total; state++)
        {
            int next = 0;
            for (int i = 0; i < n; i++) if ((state & pre[i]) == pre[i]) next |= (1 << i);
            next &= ~state;
            for (int sub = next; sub > 0; sub = (sub - 1) & next) if (__builtin_popcount(sub) <= k) dp[state | sub] = min(dp[state | sub], dp[state] + 1);
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minNumberOfSemesters(int n, int[][] relations, int k) {
        int pre[] = new int[n], total = 1 << n, dp[] = new int[total]; 
        for (int[] dependency : relations) pre[--dependency[1]] |= (1 << --dependency[0]);
        for (int i = 1; i < total; i++) dp[i] = n;
        for (int state = 0; state < total; state++) {
            int next = 0; 
            for (int i = 0; i < n; i++) if ((state & pre[i]) == pre[i]) next |= 1 << i;
            next &= ~state; 
            for (int sub = next; sub > 0; sub = (sub - 1) & next) { 
                if (Integer.bitCount(sub) <= k) {
                    dp[state | sub] = Math.min(dp[state | sub], dp[state] + 1);
                }
            }
        }
        return dp[total - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def minNumberOfSemesters(self, n: int, relations: List[List[int]], k: int) -> int:
        @lru_cache(None)
        def dfs(state: int) -> int:
            if state == (1 << n) - 1:
                return 0
            next, result = [i for i in range(n) if not state & (1 << i) and state & pre[i] == pre[i]], n + 1
            for sub in combinations(next, min(k, len(next))):
                result = min(result, 1 + dfs(state | sum([1 << i for i in sub])))
            return result
        pre = [0] * n
        for before, back in relations:
            pre[back - 1] |= 1 << (before - 1)
        return dfs(0)
```
