# 547 Number of Provinces 省份数量

__Description__:
There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.

A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise.

Return the total number of provinces.

__Example:__

Example 1:

![Provinces 1](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
Output: 2

Example 2:

![Provinces 2](https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg)

Input: isConnected = [[1,0,0],[0,1,0],[0,0,1]]
Output: 3

__Constraints:__

1 <= n <= 200
n == isConnected.length
n == isConnected[i].length
isConnected[i][j] is 1 or 0.
isConnected[i][i] == 1
isConnected[i][j] == isConnected[j][i]

__题目描述__:
有 n 个城市，其中一些彼此相连，另一些没有相连。如果城市 a 与城市 b 直接相连，且城市 b 与城市 c 直接相连，那么城市 a 与城市 c 间接相连。

省份 是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给你一个 n x n 的矩阵 isConnected ，其中 isConnected[i][j] = 1 表示第 i 个城市和第 j 个城市直接相连，而 isConnected[i][j] = 0 表示二者不直接相连。

返回矩阵中 省份 的数量。

__示例 :__

示例 1：

![省份 1](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

输入：isConnected = [[1,1,0],[1,1,0],[0,0,1]]
输出：2

示例 2：

![省份 2](https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg)

输入：isConnected = [[1,0,0],[0,1,0],[0,0,1]]
输出：3

__提示:__

1 <= n <= 200
n == isConnected.length
n == isConnected[i].length
isConnected[i][j] 为 1 或 0
isConnected[i][i] == 1
isConnected[i][j] == isConnected[j][i]

__思路__:

1. dfs
设置一个 visited数组进行 dfs
每次到 dfs 结束结果 + 1
时间复杂度 O(n ^ 2), 空间复杂度 O(n)
2. 并查集
有几个根节点就有几个省份
时间复杂度 O(n ^ 2lgn), 空间复杂度 O(n ^ 3)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    int n;
    vector<int> father = vector<int>(200);
    
    void init()
    {
        for (int i = 0; i < n; i++) father[i] = i;
    }
    
    int find(int p)
    {
        return p == father[p] ? p : father[p] = find(father[p]);
    }
    
    void connect(int p, int q)
    {
        p = find(p);
        q = find(q);
        if (p == q) return;
        father[q] = p;
    }
    
    bool connected(int p, int q)
    {
        return find(p) == find(q);
    }
public:
    int findCircleNum(vector<vector<int>>& isConnected) 
    {
        n = isConnected.size();
        init();
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) if (isConnected[i][j]) connect(i, j);
        int result = 0;
        for (int i = 0; i < n; i++) if (father[i] == i) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        boolean[] visited = new boolean[isConnected.length];
        int result = 0;
        for (int i = 0; i < isConnected.length; i++) {
            if (!visited[i]) {
                dfs(isConnected, visited, i);
                result++;
            }
        }
        return result;
    }
    
    private void dfs(int[][] isConnected, boolean[] visited, int i) {
        for (int j = 0; j < isConnected.length; j++) {
            if (isConnected[i][j] == 1 && !visited[j]) {
                visited[j] = true;
                dfs(isConnected, visited, j);
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        def dfs(i: int) -> None:
            for j in range(n):
                if isConnected[i][j] and not d[j]:
                    d[j] = True
                    dfs(j)
        d, result, n = collections.defaultdict(bool), 0, len(isConnected)
        for i in range(n):
            if not d[i]:
                d[i] = True
                dfs(i)
                result += 1
        return result
```
