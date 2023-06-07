# 1654 Minimum Jumps to Reach Home 到家的最少跳跃次数

__Description:__

A certain bug's home is on the x-axis at position `x`. Help them get there from position `0`.

The bug jumps according to the following rules:

- It can jump exactly `a` positions __forward__ (to the right).
- It can jump exactly `b` positions __backward__ (to the left).
- It cannot jump backward twice in a row.
- It cannot jump to any `forbidden` positions.

The bug may jump forward beyond its home, but it cannot jump to positions numbered with negative integers.

Given an array of integers `forbidden`, where `forbidden[i]` means that the bug cannot jump to the position `forbidden[i]`, and integers `a`, `b`, and `x`, return _the minimum number of jumps needed for the bug to reach its home_. If there is no possible sequence of jumps that lands the bug on position `x`, return `-1.`

__Example:__

Example 1:

```text
Input: forbidden = [14,4,18,1,15], a = 3, b = 15, x = 9
Output: 3
Explanation: 3 jumps forward (0 -> 3 -> 6 -> 9) will get the bug home.
```

Example 2:

```text
Input: forbidden = [8,3,16,6,12,20], a = 15, b = 13, x = 11
Output: -1
```

Example 3:

```text
Input: forbidden = [1,6,2,14,5,17,4], a = 16, b = 9, x = 7
Output: 2
Explanation: One jump forward (0 -> 16) then one jump backward (16 -> 7) will get the bug home.
```

__Constraints:__

- `1 <= forbidden.length <= 1000`
- `1 <= a, b, forbidden[i] <= 2000`
- `0 <= x <= 2000`
- All the elements in `forbidden` are distinct.
- Position `x` is not forbidden.

__题目描述:__

有一只跳蚤的家在数轴上的位置 `x` 处。请你帮助它从位置 `0` 出发，到达它的家。

跳蚤跳跃的规则如下：

- 它可以 __往前__ 跳恰好 `a` 个位置（即往右跳）。
- 它可以 __往后__ 跳恰好 `b` 个位置（即往左跳）。
- 它不能 __连续__ 往后跳 `2` 次。
- 它不能跳到任何 `forbidden` 数组中的位置。

跳蚤可以往前跳 超过 它的家的位置，但是它 不能跳到负整数 的位置。

给你一个整数数组 `forbidden` ，其中 `forbidden[i]` 是跳蚤不能跳到的位置，同时给你整数 `a`， `b` 和 `x` ，请你返回跳蚤到家的最少跳跃次数。如果没有恰好到达 `x` 的可行方案，请你返回 `-1` 。

__示例:__

示例 1：

```text
输入：forbidden = [14,4,18,1,15], a = 3, b = 15, x = 9
输出：3
解释：往前跳 3 次（0 -> 3 -> 6 -> 9），跳蚤就到家了。
```

示例 2：

```text
输入：forbidden = [8,3,16,6,12,20], a = 15, b = 13, x = 11
输出：-1
```

示例 3：

```text
输入：forbidden = [1,6,2,14,5,17,4], a = 16, b = 9, x = 7
输出：2
解释：往前跳一次（0 -> 16），然后往回跳一次（16 -> 7），跳蚤就到家了。
```

__提示：__

- `1 <= forbidden.length <= 1000`
- `1 <= a, b, forbidden[i] <= 2000`
- `0 <= x <= 2000`
- `forbidden` 中所有位置互不相同。
- 位置 `x` 不在 `forbidden` 中。

__思路:__

```text
BFS
首先猜想一个上界 bound = max(forbidden) + a + b
实际上 bound = max(max(forbidden) + a + b, x + b)
当 a >= b 时, 由于每次最多只能往回跳 1 次, 超过 x + b 之后将无法返回 x
当 a < b 时, 为了保证不跳入 forbidden 中, 可以在 max(forbidden) 意外事件进行跳跃, 每次跳跃如果超过 a + b, 则可以通过该点的坐标对 a + b 取模映射回 [0, a + b) 的区间内
由于 b > a, 所以 a + b < 2b, 所以只要跳出 a + b 总可以用 1 次往回跳跃回到 a + b 以内
这样将 forbidden 数组的所有元素加入哈希表中, 每次向前跳的值也加入哈希表中, 由于只用向后跳一次, 所以不需要记录向后跳的值
时间复杂度为 O(N), 空间复杂度为 O(N), 其中 N 为 max(max(forbidden) + a + b, x + b), 即最远能到达的距离, 或搜索区间
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumJumps(vector<int>& forbidden, int a, int b, int x) 
    {
        unordered_set<int> visited(forbidden.begin(), forbidden.end());
        int bound = max(*max_element(forbidden.begin(), forbidden.end()) + a + b, x + b);
        queue<tuple<int, int, int>> q;
        q.push(make_tuple(0, 0, 0));
        while (!q.empty()) 
        {
            auto [cur, step, direction] = q.front();
            q.pop();
            if (cur == x) return step;
            if (cur < 0 or cur > bound or visited.count(cur)) continue;
            if (!direction) 
            {
                visited.insert(cur);
                q.push(make_tuple(cur - b, step + 1, 1));
            }
            q.push(make_tuple(cur + a, step + 1, 0));
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumJumps(int[] forbidden, int a, int b, int x) {
        int m = 0;
        Set<Integer> visited = new HashSet<>();
        for (int f : forbidden) {
            visited.add(f);
            m = Math.max(f, m);
        }
        int bound = Math.max(m + a + b, x + b);
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(0);
        while (!queue.isEmpty()) {
            int front = queue.poll(), direction = front >= 0 ? 1 : -1, cur = Math.abs(front) % 10000, step = Math.abs(front) / 10000;
            if (cur == x) return step;
            if (cur > bound || visited.contains(cur)) continue;
            if (direction == 1) {
                visited.add(cur);
                if (cur > b) queue.offer(((step + 1) * 10000 + (cur - b)) * -1);
            }
            queue.offer((step + 1) * 10000 + cur + a);
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumJumps(self, forbidden: List[int], a: int, b: int, x: int) -> int:
        visited, bound, q = set(forbidden), max(max(forbidden) + a + b, x + b), deque([(0, 0, 0)])
        while q:
            cur, step, direction = q.popleft()
            if cur == x:
                return step
            cur not in visited and ((not direction and (visited.add(cur) or (cur > b and q.append((cur - b, step + 1, 1))))) or (cur + a <= bound and q.append((cur + a, step + 1, 0))))
        return -1
```
