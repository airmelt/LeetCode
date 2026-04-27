# 1001 Grid Illumination 网格照明

__Description__:
There is a 2D grid of size n x n where each cell of this grid has a lamp that is initially turned off.

You are given a 2D array of lamp positions lamps, where lamps[i] = [rowi, coli] indicates that the lamp at grid[rowi][coli] is turned on. Even if the same lamp is listed more than once, it is turned on.

When a lamp is turned on, it illuminates its cell and all other cells in the same row, column, or diagonal.

You are also given another 2D array queries, where queries[j] = [rowj, colj]. For the jth query, determine whether grid[rowj][colj] is illuminated or not. After answering the jth query, turn off the lamp at grid[rowj][colj] and its 8 adjacent lamps if they exist. A lamp is adjacent if its cell shares either a side or corner with grid[rowj][colj].

Return an array of integers ans, where ans[j] should be 1 if the cell in the jth query was illuminated, or 0 if the lamp was not.

__Example:__

Example 1:

![Illumination](https://assets.leetcode.com/uploads/2020/08/19/illu_1.jpg)

Input: n = 5, lamps = [[0,0],[4,4]], queries = [[1,1],[1,0]]
Output: [1,0]
Explanation: We have the initial grid with all lamps turned off. In the above picture we see the grid after turning on the lamp at grid[0][0] then turning on the lamp at grid[4][4].
The 0th query asks if the lamp at grid[1][1] is illuminated or not (the blue square). It is illuminated, so set ans[0] = 1. Then, we turn off all lamps in the red square.

![step 1](https://assets.leetcode.com/uploads/2020/08/19/illu_step1.jpg)

The 1st query asks if the lamp at grid[1][0] is illuminated or not (the blue square). It is not illuminated, so set ans[1] = 0. Then, we turn off all lamps in the red rectangle.

![step 2](https://assets.leetcode.com/uploads/2020/08/19/illu_step2.jpg)

Example 2:

Input: n = 5, lamps = [[0,0],[4,4]], queries = [[1,1],[1,1]]
Output: [1,1]

Example 3:

Input: n = 5, lamps = [[0,0],[0,4]], queries = [[0,4],[0,1],[1,4]]
Output: [1,1,0]

__Constraints:__

1 <= n <= 10^9
0 <= lamps.length <= 20000
0 <= queries.length <= 20000
lamps[i].length == 2
0 <= rowi, coli < n
queries[j].length == 2
0 <= rowj, colj < n

__题目描述__:
在大小为 n x n 的网格 grid 上，每个单元格都有一盏灯，最初灯都处于 关闭 状态。

给你一个由灯的位置组成的二维数组 lamps ，其中 lamps[i] = [rowi, coli] 表示 打开 位于 grid[rowi][coli] 的灯。即便同一盏灯可能在 lamps 中多次列出，不会影响这盏灯处于 打开 状态。

当一盏灯处于打开状态，它将会照亮 自身所在单元格 以及同一 行 、同一 列 和两条 对角线 上的 所有其他单元格 。

另给你一个二维数组 queries ，其中 queries[j] = [rowj, colj] 。对于第 j 个查询，如果单元格 [rowj, colj] 是被照亮的，则查询结果为 1 ，否则为 0 。在第 j 次查询之后 [按照查询的顺序] ，关闭 位于单元格 grid[rowj][colj] 上及相邻 8 个方向上（与单元格 grid[rowi][coli] 共享角或边）的任何灯。

返回一个整数数组 ans 作为答案， ans[j] 应等于第 j 次查询 queries[j] 的结果，1 表示照亮，0 表示未照亮。

__示例 :__

示例 1：

![照明](https://assets.leetcode.com/uploads/2020/08/19/illu_1.jpg)

输入：n = 5, lamps = [[0,0],[4,4]], queries = [[1,1],[1,0]]
输出：[1,0]
解释：最初所有灯都是关闭的。在执行查询之前，打开位于 [0, 0] 和 [4, 4] 的灯。第 0 次查询检查 grid[1][1] 是否被照亮（蓝色方框）。该单元格被照亮，所以 ans[0] = 1 。然后，关闭红色方框中的所有灯。

![步骤 1](https://assets.leetcode.com/uploads/2020/08/19/illu_step1.jpg)

第 1 次查询检查 grid[1][0] 是否被照亮（蓝色方框）。该单元格没有被照亮，所以 ans[1] = 0 。然后，关闭红色矩形中的所有灯。

![步骤 2](https://assets.leetcode.com/uploads/2020/08/19/illu_step2.jpg)

示例 2：

输入：n = 5, lamps = [[0,0],[4,4]], queries = [[1,1],[1,1]]
输出：[1,1]

示例 3：

输入：n = 5, lamps = [[0,0],[0,4]], queries = [[0,4],[0,1],[1,4]]
输出：[1,1,0]

__提示:__

1 <= n <= 10^9
0 <= lamps.length <= 20000
0 <= queries.length <= 20000
lamps[i].length == 2
0 <= rowi, coli < n
queries[j].length == 2
0 <= rowj, colj < n

__思路__:

哈希表
用一个集合记录 (i, j), 也可以转化成一维记录
记录开的灯泡的行列及主副对角线
只要跟开的灯泡在同一行同一列及同一个对角线上都算开
遍历灯泡, 给同一行同一列及对角线的哈希表记录
对角线可以用 i + j, i - j 代替
遍历查询, 一边查询一边关灯, 关灯需要将灯泡移出开灯集合
时间复杂度为 O(m + n), 空间复杂度为 O(n), n 为 lamps 数组的长度, m 为 queries 数组的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> gridIllumination(int n, vector<vector<int>>& lamps, vector<vector<int>>& queries) 
    {
        int m = queries.size();
        vector<int> result(m);
        unordered_map<int, int> row, col, main_diag, sub_diag;
        unordered_set<long> s;
        long N = n;
        for (const auto& l : lamps) 
        {
            int x = l.front(), y = l.back();
            if (s.find(x * N + y) != s.end()) continue;
            increment(row, x);
            increment(col, y);
            increment(main_diag, x + y);
            increment(sub_diag, x - y);
            s.insert(x * N + y);
        }
        for (int i = 0; i < m; i++) 
        {
            int x = queries[i][0], y = queries[i][1];
            if (!row[x] and !col[y] and !main_diag[x + y] and !sub_diag[x - y]) continue;
            for (const auto& d : dirs) 
            {
                int nx = x + d[0], ny = y + d[1];
                if (nx < 0 or nx >= n or ny < 0 or ny >= n) continue;
                if (s.find(nx * N + ny) != s.end()) 
                {
                    s.erase(nx * N + ny);
                    decrement(row, nx);
                    decrement(col, ny);
                    decrement(main_diag, nx + ny);
                    decrement(sub_diag, nx - ny);
                }
            }
            result[i] = 1;
        }
        return result;
    }
private:
    int dirs[9][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}, {1, -1}, {1, 1}, {-1, 1}, {-1, -1}, {0, 0}};
    
    void increment(unordered_map<int, int>& m, int key) 
    {
        ++m[key];
    }
    
    void decrement(unordered_map<int, int>& m, int key) 
    {
        if (--m[key] <= 0) m.erase(key);
    }
};
```

__Java__:

```Java
class Solution {
    private int[][] dirs = new int[][]{{0, 0}, {0, -1}, {0, 1}, {-1, 0}, {-1, -1}, {-1, 1}, {1, 0}, {1, -1}, {1, 1}};
    public int[] gridIllumination(int n, int[][] lamps, int[][] queries) {
        int m = queries.length, result[] = new int[m];
        Map<Integer, Integer> row = new HashMap<>(), col = new HashMap<>(), mainDiag = new HashMap<>(), subDiag = new HashMap<>();
        Set<Long> set = new HashSet<>();
        long N = n;
        for (int[] l : lamps) {
            int x = l[0], y = l[1];
            if (set.contains(x * N + y)) continue;
            increment(row, x);
            increment(col, y);
            increment(mainDiag, x + y);
            increment(subDiag, x - y);
            set.add(x * N + y);
        }
        for (int i = 0; i < m; i++) {
            int x = queries[i][0], y = queries[i][1];
            if (!row.containsKey(x) && !col.containsKey(y) && !mainDiag.containsKey(x + y) && !subDiag.containsKey(x - y)) continue;
            for (int[] d : dirs) {
                int nx = x + d[0], ny = y + d[1];
                if (nx < 0 || nx >= n || ny < 0 || ny >= n) continue;
                if (set.contains(nx * N + ny)) {
                    set.remove(nx * N + ny);
                    decrement(row, nx);
                    decrement(col, ny);
                    decrement(mainDiag, nx + ny);
                    decrement(subDiag, nx - ny);
                }
            }
            result[i] = 1;
        }
        return result;
    }
    
    private void increment(Map<Integer, Integer> map, int key) {
        map.put(key, map.getOrDefault(key, 0) + 1);
    }
    
    private void decrement(Map<Integer, Integer> map, int key) {
        if (map.get(key) == 1) map.remove(key);
        else map.put(key, map.get(key) - 1);
    }
}
```

__Python__:

```Python
class Solution:
    def gridIllumination(self, n: int, lamps: List[List[int]], queries: List[List[int]]) -> List[int]:
        result, opened_lamp, row, col, main_diag, sub_diag = [], set(), defaultdict(int), defaultdict(int), defaultdict(int), defaultdict(int)
        for i, j in lamps:
            if (i, j) not in opened_lamp:
                opened_lamp.add((i, j))
                row[i] += 1
                col[j] += 1
                main_diag[i + j] += 1
                sub_diag[i - j] += 1
        for i, j in queries:
            if not row[i] and not col[j] and not main_diag[i + j] and not sub_diag[i - j]:
                result.append(0)
                continue
            result.append(1)
            for x, y in [(i, j), (i - 1, j), (i - 1, j - 1), (i - 1, j + 1), (i + 1, j), (i + 1, j - 1), (i + 1, j + 1), (i, j + 1), (i, j - 1)]:
                if (x, y) in opened_lamp:
                    opened_lamp.remove((x, y))
                    row[x] -= 1
                    col[y] -= 1
                    main_diag[x + y] -= 1
                    sub_diag[x - y] -= 1
        return result
```
