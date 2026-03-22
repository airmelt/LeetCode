# 864 Shortest Path to Get All Keys 获取所有钥匙的最短路径

__Description__:
You are given an m x n grid grid where:

'.' is an empty cell.
'#' is a wall.
'@' is the starting point.
Lowercase letters represent keys.
Uppercase letters represent locks.
You start at the starting point and one move consists of walking one space in one of the four cardinal directions. You cannot walk outside the grid, or walk into a wall.

If you walk over a key, you can pick it up and you cannot walk over a lock unless you have its corresponding key.

For some 1 <= k <= 6, there is exactly one lowercase and one uppercase letter of the first k letters of the English alphabet in the grid. This means that there is exactly one key for each lock, and one lock for each key; and also that the letters used to represent the keys and locks were chosen in the same order as the English alphabet.

Return the lowest number of moves to acquire all keys. If it is impossible, return -1.

__Example:__

Example 1:

![keys1](https://assets.leetcode.com/uploads/2021/07/23/lc-keys2.jpg)

Input: grid = ["@.a.#","###.#","b.A.B"]
Output: 8
Explanation: Note that the goal is to obtain all the keys not to open all the locks.

Example 2:

![keys2](https://assets.leetcode.com/uploads/2021/07/23/lc-key2.jpg)

Input: grid = ["@..aA","..B#.","....b"]
Output: 6

Example 3:

![keys3](https://assets.leetcode.com/uploads/2021/07/23/lc-keys3.jpg)

Input: grid = ["@Aa"]
Output: -1

__Constraints:__

m == grid.length
n == grid[i].length
1 <= m, n <= 30
grid[i][j] is either an English letter, '.', '#', or '@'.
The number of keys in the grid is in the range [1, 6].
Each key in the grid is unique.
Each key in the grid has a matching lock.

__题目描述__:
给定一个二维网格 grid。 "." 代表一个空房间， "#" 代表一堵墙， "@" 是起点，（"a", "b", ...）代表钥匙，（"A", "B", ...）代表锁。

我们从起点开始出发，一次移动是指向四个基本方向之一行走一个单位空间。我们不能在网格外面行走，也无法穿过一堵墙。如果途经一个钥匙，我们就把它捡起来。除非我们手里有对应的钥匙，否则无法通过锁。

假设 K 为钥匙/锁的个数，且满足 1 <= K <= 6，字母表中的前 K 个字母在网格中都有自己对应的一个小写和一个大写字母。换言之，每个锁有唯一对应的钥匙，每个钥匙也有唯一对应的锁。另外，代表钥匙和锁的字母互为大小写并按字母顺序排列。

返回获取所有钥匙所需要的移动的最少次数。如果无法获取所有钥匙，返回 -1 。

__示例 :__

示例 1：

输入：["@.a.#","###.#","b.A.B"]
输出：8

示例 2：

输入：["@..aA","..B#.","....b"]
输出：6

__提示:__

1 <= grid.length <= 30
1 <= grid[0].length <= 30
grid[i][j] 只含有 '.', '#', '@', 'a'-'f' 以及 'A'-'F'
钥匙的数目范围是 [1, 6]，每个钥匙都对应一个不同的字母，正好打开一个对应的锁。

__思路__:

Dijkstra ➕ 状态压缩
总共只有 6 把钥匙用一个 int 数字可以存下各状态
只需要每个关键结点, 即钥匙的位置
用一个优先队列记录各结点的相对位置, 从起点出发依次遍历各关键结点直到找到所有钥匙
每次查询的时候注意没有钥匙是不能开门的
时间复杂度为 O(mn), 空间复杂度为 O(mn)

__代码__:
__C++__:

```C++
struct Node 
{
    int dist;
    char cur;
    int state;

    Node(int dist, char cur, int state) : dist(dist), cur(cur), state(state) {}
};

bool operator<(const Node& a, const Node& b)
{
    return a.dist > b.dist;
}

class Solution 
{
private:
    int m, n;

    unordered_map<char, int> next_distance(char source,  unordered_map<char, int>& char_loc, vector<string>& grid)
    {
        int r = char_loc[source] / 100, c = char_loc[source] % 100, directions[5] = { 1, 0, -1, 0, 1 }, steps = 0;
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        unordered_map<char, int> distances;
        queue<pair<int, int>> q;
        q.push({ r, c });
        visited[r][c] = true;
        while (!q.empty())
        {
            for (int i = q.size(); i > 0; i--)
            {
                r = q.front().first;
                c = q.front().second;
                q.pop();
                if (grid[r][c] != source and grid[r][c] != '.')
                {
                    distances[grid[r][c]] = steps;
                    continue;
                }
                for (int j = 0; j < 4; j++)
                {
                    int next_r = r + directions[j], next_c = c + directions[j + 1];
                    if (next_r > -1 and next_r < m and next_c > -1 and next_c < n and !visited[next_r][next_c] and grid[next_r][next_c] != '#')
                    {
                        visited[next_r][next_c] = true;
                        q.push({next_r, next_c});
                    }
                }
            }
            ++steps;
        }
        return distances;
    }

public:
    int shortestPathAllKeys(vector<string>& grid) 
    {
        m = grid.size();
        n = grid[0].size();
        unordered_map<char, int> char_loc;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j] != '.' and grid[i][j] != '#') char_loc[grid[i][j]] = i * 100 + j;
        unordered_map<char, unordered_map<char, int>> distances;
        for (const auto& [k, v] : char_loc) distances[k] = next_distance(k, char_loc, grid);
        int target = (1 << (char_loc.size() >> 1)) - 1;
        unordered_map<int, int> source;
        source['@'*256 + 0] = 0;
        priority_queue<Node> q;
        q.emplace(Node(0, '@', 0));
        while (!q.empty())
        {
            Node node = q.top();
            q.pop();
            int node_dist = node.dist;
            if (node.state == target) return node_dist;
            auto& nexts = distances[node.cur];
            for (const auto& [k, v] : nexts)
            {
                int s2 = node.state;
                if (k >= 'a' and k <= 'z') s2 |= (1 << (k - 'a'));
                else if (k >= 'A' and k <= 'Z') if (!(s2 & (1 << (k - 'A')))) continue;
                int cur_state = k * 256 + s2;
                if ((source.find(cur_state) == source.end()) or (node_dist + v < source[cur_state]))
                {
                    source[cur_state] = node_dist + v;
                    q.emplace(Node{ node_dist + v, k, s2 });
                }
            }
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int shortestPathAllKeys(String[] grid) {
        int x = 0, y = 0, m = grid.length, n = grid[0].length(), target = 0, directions[][] = { { -1, 0 }, { 1, 0 }, { 0, 1 }, { 0, -1 } }, step = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                char c = grid[i].charAt(j);
                if (c >= 'a' && c <= 'f') {
                    target |= 1 << (c - 'a');
                }
                if (c == '@') {
                    x = i;
                    y = j;
                }
            }
        }
        boolean[][][] visited = new boolean[m][n][64];
        Queue<int[]> queue = new ArrayDeque<>();
        queue.offer(new int[]{x, y, 0});
        visited[x][y][0] = true;
        while (!queue.isEmpty()) {
            ++step;
            int size = queue.size();
            while (size-- > 0) {
                int[] cur = queue.poll();
                for (int[] dir : directions) {
                    int xx = cur[0] + dir[0], yy = cur[1] + dir[1], newKey = cur[2];
                    if (xx < 0 || xx >= m || yy < 0 || yy >= n || visited[xx][yy][newKey] || grid[xx].charAt(yy) == '#') continue;
                    char c = grid[xx].charAt(yy);
                    if (c >= 'a' && c <= 'f') {
                        if (!hasKey(newKey, c - 'a')) {
                            newKey += (1 << (c - 'a'));
                            if (newKey == target) return step;
                        }
                    } else if (c >= 'A' && c <= 'F') if (!hasKey(newKey, c - 'A')) continue;
                    visited[xx][yy][newKey] = true;
                    queue.offer(new int[]{ xx, yy, newKey });
                }
            }
        }
        return -1;
    }
    
    private boolean hasKey(int n, int i) {
        return (n & (1 << i)) != 0;
    }
}
```

__Python__:

```Python
class Solution:
    def shortestPathAllKeys(self, grid: List[str]) -> int:
        n, m, direction, s, q, dist = len(grid), len(grid[0]), [(-1, 0), (1, 0), (0, -1), (0, 1)], 0, deque(), defaultdict(lambda:float('inf'))
        for i in range(n):
            for j in range(m):
                if grid[i][j] == '@':
                    dist[(i, j, 0)] = 0
                    q.append((i, j, 0))
                elif grid[i][j].isupper():
                    s += 1
        target = (1 << s) - 1
        while q:
            d = dist[(t := q.popleft())]
            i, j, s = t
            for dx, dy in direction:
                x, y, new_s = i + dx, j + dy, s
                if -1 < x < n and -1 < y < m and grid[x][y] != '#':
                    if (c := grid[x][y]).islower():
                        new_s |= (1 << (ord(c) - ord('a')))
                        if dist[(x, y, new_s)] > d + 1:
                            dist[(x, y, new_s)] = d + 1
                            if new_s == target:
                                return dist[(x, y, new_s)]
                            q.append((x, y, new_s))
                    elif c.isupper():
                        if new_s & (1 << (ord(c) - ord('A'))):
                            if dist[(x, y, new_s)] > d + 1:
                                dist[(x, y, new_s)] = d + 1
                                q.append((x, y, new_s))
                    else:
                        if dist[(x, y, new_s)] > d + 1:
                            dist[(x, y, new_s)] = d + 1
                            q.append((x, y, new_s))
        return -1
```
