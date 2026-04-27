# 2101 Detonate the Maximum Bombs 引爆最多的炸弹

__Description:__

You are given a list of bombs. The range of a bomb is defined as the area where its effect can be felt. This area is in the shape of a circle with the center as the location of the bomb.

The bombs are represented by a __0-indexed__ 2D integer array `bombs` where `bombs[i] = [xi, yi, ri]`. `xi` and `yi` denote the X-coordinate and Y-coordinate of the location of the `i ^ th` bomb, whereas `ri` denotes the __radius__ of its range.

You may choose to detonate a single bomb. When a bomb is detonated, it will detonate all bombs that lie in its range. These bombs will further detonate the bombs that lie in their ranges.

Given the list of `bombs`, return _the __maximum__ number of bombs that can be detonated if you are allowed to detonate __only one__ bomb_.

__Example:__

Example 1:

![2101-1](https://assets.leetcode.com/uploads/2021/11/06/desmos-eg-3.png)

```text
Input: bombs = [[2,1,3],[6,1,4]]
Output: 2
Explanation:
The above figure shows the positions and ranges of the 2 bombs.
If we detonate the left bomb, the right bomb will not be affected.
But if we detonate the right bomb, both bombs will be detonated.
So the maximum bombs that can be detonated is max(1, 2) = 2.
```

Example 2:

![2101-2](https://assets.leetcode.com/uploads/2021/11/06/desmos-eg-2.png)

```text
Input: bombs = [[1,1,5],[10,10,5]]
Output: 1
Explanation:
Detonating either bomb will not detonate the other bomb, so the maximum number of bombs that can be detonated is 1.
```

Example 3:

![2101-3](https://assets.leetcode.com/uploads/2021/11/07/desmos-eg1.png)

```text
Input: bombs = [[1,2,3],[2,3,1],[3,4,2],[4,5,3],[5,6,4]]
Output: 5
Explanation:
The best bomb to detonate is bomb 0 because:
- Bomb 0 detonates bombs 1 and 2. The red circle denotes the range of bomb 0.
- Bomb 2 detonates bomb 3. The blue circle denotes the range of bomb 2.
- Bomb 3 detonates bomb 4. The green circle denotes the range of bomb 3.
Thus all 5 bombs are detonated.
```

__Constraints:__

- `1 <= bombs.length <= 100`
- `bombs[i].length == 3`
- `1 <= xi, yi, ri <= 10 ^ 5`

__题目描述:__

给你一个炸弹列表。一个炸弹的 爆炸范围 定义为以炸弹为圆心的一个圆。

炸弹用一个下标从 __0__ 开始的二维整数数组 `bombs` 表示，其中 `bombs[i] = [xi, yi, ri]` 。 `xi` 和 `yi` 表示第 `i` 个炸弹的 X 和 Y 坐标， `ri` 表示爆炸范围的 __半径__ 。

你需要选择引爆 一个 炸弹。当这个炸弹被引爆时，所有 在它爆炸范围内的炸弹都会被引爆，这些炸弹会进一步将它们爆炸范围内的其他炸弹引爆。

给你数组 `bombs` ，请你返回在引爆 __一个__ 炸弹的前提下，__最多__ 能引爆的炸弹数目。

__示例:__

示例 1：

![2101-4](https://assets.leetcode.com/uploads/2021/11/06/desmos-eg-3.png)

```text
输入：bombs = [[2,1,3],[6,1,4]]
输出：2
解释：
上图展示了 2 个炸弹的位置和爆炸范围。
如果我们引爆左边的炸弹，右边的炸弹不会被影响。
但如果我们引爆右边的炸弹，两个炸弹都会爆炸。
所以最多能引爆的炸弹数目是 max(1, 2) = 2 。
```

示例 2：

![2101-5](https://assets.leetcode.com/uploads/2021/11/06/desmos-eg-2.png)

```text
输入：bombs = [[1,1,5],[10,10,5]]
输出：1
解释：
引爆任意一个炸弹都不会引爆另一个炸弹。所以最多能引爆的炸弹数目为 1 。
```

示例 3：

![2101-6](https://assets.leetcode.com/uploads/2021/11/07/desmos-eg1.png)

```text
输入：bombs = [[1,2,3],[2,3,1],[3,4,2],[4,5,3],[5,6,4]]
输出：5
解释：
最佳引爆炸弹为炸弹 0 ，因为：
- 炸弹 0 引爆炸弹 1 和 2 。红色圆表示炸弹 0 的爆炸范围。
- 炸弹 2 引爆炸弹 3 。蓝色圆表示炸弹 2 的爆炸范围。
- 炸弹 3 引爆炸弹 4 。绿色圆表示炸弹 3 的爆炸范围。
所以总共有 5 个炸弹被引爆。
```

__提示：__

- `1 <= bombs.length <= 100`
- `bombs[i].length == 3`
- `1 <= xi, yi, ri <= 10 ^ 5`

__思路:__

```text
DFS
遍历列表, 将能够引爆的其他炸弹加入哈希表中的列表
以每个炸弹为起点引爆所有炸弹
统计每次能够引爆的炸弹并更新结果
时间复杂度为 O(N ^ 3), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumDetonation(vector<vector<int>>& bombs) 
    {
        int n = bombs.size(), result = 0;
        unordered_map<int, vector<int>> graph;
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) if (i != j and (long long)bombs[i][2] * bombs[i][2] >= (long long)(bombs[i][0] - bombs[j][0]) * (bombs[i][0] - bombs[j][0]) + (long long)(bombs[i][1] - bombs[j][1]) * (bombs[i][1] - bombs[j][1])) graph[i].emplace_back(j);
        for (int i = 0; i < n; i++) 
        {
            int c = 1;
            vector<int> visited(n);
            visited[i] = true;
            queue<int> q;
            q.push(i);
            while (!q.empty()) 
            {
                int cur = q.front();
                q.pop();
                for (int next : graph[cur])
                {
                    if (!visited[next]) 
                    {
                        visited[next] = true;
                        q.push(next);
                        ++c;
                    }
                }
            }
            result = max(result, c);
        }
        return result;
    }
};

```

__Java__:

```Java
class Solution {
    public int maximumDetonation(int[][] bombs) {
        int n = bombs.length, result = 0;
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for (int i = 0; i < n; i++) graph.put(i, new ArrayList<>());    
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) if (i != j && (long)bombs[i][2] * bombs[i][2] >= (long)(bombs[i][0] - bombs[j][0]) * (bombs[i][0] - bombs[j][0]) + (long)(bombs[i][1] - bombs[j][1]) * (bombs[i][1] - bombs[j][1])) graph.get(i).add(j);
        for (int i = 0; i < n; i++) {
            int count = 1;
            boolean[] visited = new boolean[n];
            visited[i] = true;
            Queue<Integer> queue = new LinkedList<>();
            queue.offer(i);
            while (!queue.isEmpty()) {
                for (int next : graph.get(queue.poll())) {
                    if (!visited[next]) {
                        visited[next] = true;
                        queue.offer(next);
                        ++count;
                    }
                }
            }
            result = Math.max(result, count);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumDetonation(self, bombs: List[List[int]]) -> int:
        n, graph, result = len(bombs), defaultdict(set), 0
        for i in range(n):
            for j in range(i + 1, n):
                if i != j and bombs[i][2] ** 2 >= (bombs[i][0] - bombs[j][0]) ** 2 + (bombs[i][1] - bombs[j][1]) ** 2:
                    graph[i].add(j)
        for i in range(n):
            visited, count, q = [False] * n, 1, deque([i])
            visited[i] = True
            while q:
                for next in graph[q.popleft()]:
                    if visited[next]:
                        continue
                    count += 1
                    q.append(next)
                    visited[next] = True
            result = max(result, count)
        return result
```
