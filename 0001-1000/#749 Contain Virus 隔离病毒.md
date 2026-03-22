# 749 Contain Virus 隔离病毒

__Description__:
A virus is spreading rapidly, and your task is to quarantine the infected area by installing walls.

The world is modeled as an m x n binary grid isInfected, where isInfected[i][j] == 0 represents uninfected cells, and isInfected[i][j] == 1 represents cells contaminated with the virus. A wall (and only one wall) can be installed between any two 4-directionally adjacent cells, on the shared boundary.

Every night, the virus spreads to all neighboring cells in all four directions unless blocked by a wall. Resources are limited. Each day, you can install walls around only one region (i.e., the affected area (continuous block of infected cells) that threatens the most uninfected cells the following night). There will never be a tie.

Return the number of walls used to quarantine all the infected regions. If the world will become fully infected, return the number of walls used.

__Example:__

Example 1:

![virus 11](https://assets.leetcode.com/uploads/2021/06/01/virus11-grid.jpg)

Input: isInfected = [[0,1,0,0,0,0,0,1],[0,1,0,0,0,0,0,1],[0,0,0,0,0,0,0,1],[0,0,0,0,0,0,0,0]]
Output: 10
Explanation: There are 2 contaminated regions.
On the first day, add 5 walls to quarantine the viral region on the left. The board after the virus spreads is:

![virus 12](https://assets.leetcode.com/uploads/2021/06/01/virus12edited-grid.jpg)

On the second day, add 5 walls to quarantine the viral region on the right. The virus is fully contained.

![virus 13](https://assets.leetcode.com/uploads/2021/06/01/virus13edited-grid.jpg)

Example 2:

![virus 2](https://assets.leetcode.com/uploads/2021/06/01/virus2-grid.jpg)

Input: isInfected = [[1,1,1],[1,0,1],[1,1,1]]
Output: 4
Explanation: Even though there is only one cell saved, there are 4 walls built.
Notice that walls are only built on the shared boundary of two different cells.

Example 3:

Input: isInfected = [[1,1,1,0,0,0,0,0,0],[1,0,1,0,1,1,1,1,1],[1,1,1,0,0,0,0,0,0]]
Output: 13
Explanation: The region on the left only builds two new walls.

__Constraints:__

m == isInfected.length
n == isInfected[i].length
1 <= m, n <= 50
isInfected[i][j] is either 0 or 1.
There is always a contiguous viral region throughout the described process that will infect strictly more uncontaminated squares in the next round.

__题目描述__:
病毒扩散得很快，现在你的任务是尽可能地通过安装防火墙来隔离病毒。

假设世界由二维矩阵组成，0 表示该区域未感染病毒，而 1 表示该区域已感染病毒。可以在任意 2 个四方向相邻单元之间的共享边界上安装一个防火墙（并且只有一个防火墙）。

每天晚上，病毒会从被感染区域向相邻未感染区域扩散，除非被防火墙隔离。现由于资源有限，每天你只能安装一系列防火墙来隔离其中一个被病毒感染的区域（一个区域或连续的一片区域），且该感染区域对未感染区域的威胁最大且保证唯一。

你需要努力使得最后有部分区域不被病毒感染，如果可以成功，那么返回需要使用的防火墙个数; 如果无法实现，则返回在世界被病毒全部感染时已安装的防火墙个数。

__示例 :__

示例 1：

输入: grid =
[[0,1,0,0,0,0,0,1],
 [0,1,0,0,0,0,0,1],
 [0,0,0,0,0,0,0,1],
 [0,0,0,0,0,0,0,0]]
输出: 10
说明:
一共有两块被病毒感染的区域: 从左往右第一块需要 5 个防火墙，同时若该区域不隔离，晚上将感染 5 个未感染区域（即被威胁的未感染区域个数为 5）;
第二块需要 4 个防火墙，同理被威胁的未感染区域个数是 4。因此，第一天先隔离左边的感染区域，经过一晚后，病毒传播后世界如下:
[[0,1,0,0,0,0,1,1],
 [0,1,0,0,0,0,1,1],
 [0,0,0,0,0,0,1,1],
 [0,0,0,0,0,0,0,1]]
第二天，只剩下一块未隔离的被感染的连续区域，此时需要安装 5 个防火墙，且安装完毕后病毒隔离任务完成。

示例 2：

输入: grid =
[[1,1,1],
 [1,0,1],
 [1,1,1]]
输出: 4
说明:
此时只需要安装 4 面防火墙，就有一小区域可以幸存，不被病毒感染。
注意不需要在世界边界建立防火墙。

示例 3:

输入: grid =
[[1,1,1,0,0,0,0,0,0],
 [1,0,1,0,1,1,1,1,1],
 [1,1,1,0,0,0,0,0,0]]
输出: 13
说明:
在隔离右边感染区域后，隔离左边病毒区域只需要 2 个防火墙了。

__说明:__

grid 的行数和列数范围是 [1, 50]。
 grid[i][j] 只包含 0 或 1 。
题目保证每次选取感染区域进行隔离时，一定存在唯一一个对未感染区域的威胁最大的区域。

__思路__:

模拟
每次找到周长最长的病毒区域, 放下防火墙, 将病毒区域内的都改为 -1, 表示已经访问过
将剩下的边界全部感染
时间复杂度为 O((mn) ^ 4/3), 空间复杂度为 O(mn)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    unordered_set<int> visited;
    vector<unordered_set<int>> regions;
    vector<unordered_set<int>> frontiers;
    vector<int> perimeters, dx{ -1, 1, 0, 0 }, dy{ 0, 0, -1, 1 };
    int r, c;
    
    void dfs(vector<vector<int>>& grid, int x, int y) 
    {
        if (!visited.count(x * c + y)) 
        {
            visited.insert(x * c + y);
            regions.back().insert(x * c + y);
            for (int k = 0; k < 4; ++k) 
            {
                int nx = x + dx[k], ny = y + dy[k];
                if (nx > -1 and nx < r and ny > -1 and ny < c) 
                {
                    if (grid[nx][ny] == 1) dfs(grid, nx, ny);
                    else if (!grid[nx][ny])
                    {
                        frontiers.back().insert(nx * c + ny);
                        ++perimeters.back();
                    }
                }
            }
        }
    }
public:
    int containVirus(vector<vector<int>>& isInfected) 
    {
        r = isInfected.size();
        c = isInfected.front().size();
        int result = 0;
        while (true)
        {
            visited.clear();
            regions.clear();
            frontiers.clear();
            perimeters.clear();
            for (int x = 0; x < r; x++) 
            {
                for (int y = 0; y < c; y++) 
                {
                    if (isInfected[x][y] == 1 and !visited.count(x * c + y))
                    {
                        unordered_set<int> s, t;
                        regions.emplace_back(s);
                        frontiers.emplace_back(t);
                        perimeters.emplace_back(0);
                        dfs(isInfected, x, y);
                    }
                }
            }
            if (regions.empty()) break;
            int triage_index = 0;
            for (int i = 0; i < frontiers.size(); i++) if (frontiers[triage_index].size() < frontiers[i].size()) triage_index = i;
            result += perimeters[triage_index];
            for (int i = 0; i < regions.size(); i++) 
            {
                if (i == triage_index) for (const auto code : regions[i]) isInfected[code / c][code % c] = -1;
                else 
                {
                    for (const auto code : regions[i]) 
                    {
                        int x = code / c, y = code % c;
                        for (int k = 0; k < 4; k++) 
                        {
                            int nx = x + dx[k], ny = y + dy[k];
                            if (nx > -1 and nx < r and ny > -1 and ny < c and !isInfected[nx][ny]) isInfected[nx][ny] = 1;
                        }
                    }
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private Set<Integer> visited;
    private List<Set<Integer>> regions;
    private List<Set<Integer>> frontiers;
    private List<Integer> perimeters;
    private int dx[] = new int[]{ -1, 1, 0, 0 }, dy[] = new int[]{ 0, 0, -1, 1 }, r, c, grid[][];

    public int containVirus(int[][] isInfected) {
        grid = isInfected;
        r = grid.length;
        c = grid[0].length;
        int result = 0;
        while (true) {
            visited = new HashSet();
            regions = new ArrayList();
            frontiers = new ArrayList();
            perimeters = new ArrayList();

            for (int x = 0; x < r; x++) {
                for (int y = 0; y < c; y++) {
                    if (grid[x][y] == 1 && !visited.contains(x * c + y)) {
                        regions.add(new HashSet());
                        frontiers.add(new HashSet());
                        perimeters.add(0);
                        dfs(x, y);
                    }
                }
            }
            if (regions.isEmpty()) break;
            int triageIndex = 0;
            for (int i = 0; i < frontiers.size(); i++) if (frontiers.get(triageIndex).size() < frontiers.get(i).size()) triageIndex = i;
            result += perimeters.get(triageIndex);
            for (int i = 0; i < regions.size(); ++i) {
                if (i == triageIndex) for (int code : regions.get(i)) grid[code / c][code % c] = -1;
                else {
                    for (int code : regions.get(i)) {
                        int x = code / c, y = code % c;
                        for (int k = 0; k < 4; k++) {
                            int nx = x + dx[k], ny = y + dy[k];
                            if (nx > -1 && nx < r && ny > -1 && ny < c && grid[nx][ny] == 0) grid[nx][ny] = 1;
                        }
                    }
                }
            }
        }
        return result;
    }

    public void dfs(int x, int y) {
        if (!visited.contains(x * c + y)) {
            visited.add(x * c + y);
            int n = regions.size();
            regions.get(n - 1).add(x * c + y);
            for (int k = 0; k < 4; k++) {
                int nx = x + dx[k], ny = y + dy[k];
                if (nx > -1 && nx < r && ny > -1 && ny < c) {
                    if (grid[nx][ny] == 1) dfs(nx, ny);
                    else if (grid[nx][ny] == 0){
                        frontiers.get(n - 1).add(nx * c + ny);
                        perimeters.set(n - 1, perimeters.get(n - 1) + 1);
                    }
                }
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def containVirus(self, isInfected: List[List[int]]) -> int:
        r, c, d = len(isInfected), len(isInfected[0]), [(0, 1), (0, -1), (1, 0), (-1, 0)]

        def dfs(x: int, y: int) -> None:
            if (x, y) not in visited:
                visited.add((x, y))
                regions[-1].add((x, y))
                for dx, dy in d:
                    next_x, next_y = x + dx, y + dy
                    if (not -1 < next_x < r) or (not -1 < next_y < c):
                        continue
                    isInfected[next_x][next_y] == 1 and dfs(next_x, next_y)
                    if not isInfected[next_x][next_y]:
                        frontiers[-1].add((next_x, next_y)) 
                        perimeters[-1] += 1

        result = 0
        while True:
            visited, regions, frontiers, perimeters = set(), [], [], []
            for i, row in enumerate(isInfected):
                for j, val in enumerate(row):
                    if val == 1 and (i, j) not in visited:
                        regions.append(set())
                        frontiers.append(set())
                        perimeters.append(0)
                        dfs(i, j)
            if not regions: 
                break
            triage_index = frontiers.index(max(frontiers, key=len))
            result += perimeters[triage_index]
            for i, reg in enumerate(regions):
                if i == triage_index:
                    for x, y in reg:
                        isInfected[x][y] = -1
                else:
                    for x, y in reg:
                        for dx, dy in d:
                            next_x, next_y = x + dx, y + dy
                            if not (-1 < next_x < r) or not (-1 < next_y < c):
                                continue
                            isInfected[next_x][next_y] = 1 if not isInfected[next_x][next_y] else isInfected[next_x][next_y]
        return result
```
