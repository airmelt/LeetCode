__Description__:
You have N gardens, labelled 1 to N.  In each garden, you want to plant one of 4 types of flowers.

paths[i] = [x, y] describes the existence of a bidirectional path from garden x to garden y.

Also, there is no garden that has more than 3 paths coming into or leaving it.

Your task is to choose a flower type for each garden such that, for any two gardens connected by a path, they have different types of flowers.

Return any such a choice as an array answer, where answer[i] is the type of flower planted in the (i+1)-th garden.  The flower types are denoted 1, 2, 3, or 4.  It is guaranteed an answer exists.

__Example:__
Example 1:

Input: N = 3, paths = [[1,2],[2,3],[3,1]]
Output: [1,2,3]

Example 2:

Input: N = 4, paths = [[1,2],[3,4]]
Output: [1,2,1,2]

Example 3:

Input: N = 4, paths = [[1,2],[2,3],[3,4],[4,1],[1,3],[2,4]]
Output: [1,2,3,4]
 
__Note:__

1 <= N <= 10000
0 <= paths.size <= 20000
No garden has 4 or more paths coming into or leaving it.
It is guaranteed an answer exists.

__题目描述__:
有 N 个花园，按从 1 到 N 标记。在每个花园中，你打算种下四种花之一。

paths[i] = [x, y] 描述了花园 x 到花园 y 的双向路径。

另外，没有花园有 3 条以上的路径可以进入或者离开。

你需要为每个花园选择一种花，使得通过路径相连的任何两个花园中的花的种类互不相同。

以数组形式返回选择的方案作为答案 answer，其中 answer[i] 为在第 (i+1) 个花园中种植的花的种类。花的种类用  1, 2, 3, 4 表示。保证存在答案。

__示例 :__
示例 1：

输入：N = 3, paths = [[1,2],[2,3],[3,1]]
输出：[1,2,3]
示例 2：

输入：N = 4, paths = [[1,2],[3,4]]
输出：[1,2,1,2]
示例 3：

输入：N = 4, paths = [[1,2],[2,3],[3,4],[4,1],[1,3],[2,4]]
输出：[1,2,3,4]
 
__提示：__

1 <= N <= 10000
0 <= paths.size <= 20000
不存在花园有 4 条或者更多路径可以进入或离开。
保证存在答案。

__思路__:
贪心法涂色, 题目保证了每个结点最多只有 3个出度(入度)
遍历 paths数组, 将比自己小的结点存入表格中, 然后遍历 N个结点, 找到最小的未使用过的颜色加入结果
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<int> gardenNoAdj(int N, vector<vector<int>>& paths) 
    {
        vector<int> result(N, 1);
        vector<vector<int>> record(N);
        for (auto path : paths) record[max(path[0], path[1]) - 1].push_back(min(path[0], path[1]) - 1);
        for (int i = 1; i < N; ++i) 
        {
            int used[5]{0}; 
            for (auto j : record[i]) used[result[j]] = 1;
            for (int j = 4; j > 0; --j) if (!used[j]) result[i] = j;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int[] gardenNoAdj(int N, int[][] paths) {
        int result[] = new int[N];
        Map<Integer, Set<Integer>> record = new HashMap<>(N);
        for (int i = 0; i < N; i++) record.put(i, new HashSet<>());
        for (int[] path : paths) record.get(Math.max(path[0], path[1]) - 1).add(Math.min(path[0], path[1]) - 1);
        for (int i = 0; i < N; ++i) {
            int used[] = new int[5]; 
            for (int j : record.get(i)) used[result[j]] = 1;
            for (int j = 4; j > 0; --j) if (used[j] == 0) result[i] = j;
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def gardenNoAdj(self, N: int, paths: List[List[int]]) -> List[int]:
        result, record = [1] * N, [set() for _ in range(N)]
        for path in paths:
            record[max(path[0], path[1]) - 1].add(min(path[0], path[1]) - 1)
        for i in range(1, N):
            result[i] = ({1, 2, 3, 4} - {result[j] for j in record[i]}).pop()
        return result
```