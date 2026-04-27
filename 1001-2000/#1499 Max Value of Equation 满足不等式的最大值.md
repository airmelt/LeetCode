# 1499 Max Value of Equation 满足不等式的最大值

__Description:__

You are given an array `points` containing the coordinates of points on a 2D plane, sorted by the x-values, where `points[i] = [xi, yi]` such that `xi < xj` for all `1 <= i < j <= points.length`. You are also given an integer `k`.

Return _the maximum value of the equation_ `yi + yj + |xi - xj|` where `|xi - xj| <= k` and `1 <= i < j <= points.length`.

It is guaranteed that there exists at least one pair of points that satisfy the constraint `|xi - xj| <= k`.

__Example:__

Example 1:

```text
Input: points = [[1,3],[2,0],[5,10],[6,-10]], k = 1
Output: 4
Explanation: The first two points satisfy the condition |xi - xj| <= 1 and if we calculate the equation we get 3 + 0 + |1 - 2| = 4. Third and fourth points also satisfy the condition and give a value of 10 + -10 + |5 - 6| = 1.
No other pairs satisfy the condition, so we return the max of 4 and 1.
```

Example 2:

```text
Input: points = [[0,0],[3,0],[9,2]], k = 3
Output: 3
Explanation: Only the first two points have an absolute difference of 3 or less in the x-values, and give the value of 0 + 0 + |0 - 3| = 3.
```

__Constraints:__

- `2 <= points.length <= 10 ^ 5`
- `points[i].length == 2`
- `-10 ^ 8 <= xi, yi <= 10 ^ 8`
- `0 <= k <= 2 * 10 ^ 8`
- `xi < xj` for all `1 <= i < j <= points.length`
- `xi` form a strictly increasing sequence.

__题目描述:__

给你一个数组 `points` 和一个整数 `k` 。数组中每个元素都表示二维平面上的点的坐标，并按照横坐标 x 的值从小到大排序。也就是说 `points[i] = [xi, yi]` ，并且在 `1 <= i < j <= points.length` 的前提下， `xi < xj` 总成立。

请你找出 `yi + yj + |xi - xj|` 的 __最大值__，其中 `|xi - xj| <= k` 且 `1 <= i < j <= points.length`。

题目测试数据保证至少存在一对能够满足 `|xi - xj| <= k` 的点。

__示例:__

示例 1：

```text
输入：points = [[1,3],[2,0],[5,10],[6,-10]], k = 1
输出：4
解释：前两个点满足 |xi - xj| <= 1 ，代入方程计算，则得到值 3 + 0 + |1 - 2| = 4 。第三个和第四个点也满足条件，得到值 10 + -10 + |5 - 6| = 1 。
没有其他满足条件的点，所以返回 4 和 1 中最大的那个。
```

示例 2：

```text
输入：points = [[0,0],[3,0],[9,2]], k = 3
输出：3
解释：只有前两个点满足 |xi - xj| <= 3 ，代入方程后得到值 0 + 0 + |0 - 3| = 3 。
```

__提示：__

- `2 <= points.length <= 10 ^ 5`
- `points[i].length == 2`
- `-10 ^ 8 <= points[i][0], points[i][1] <= 10 ^ 8`
- `0 <= k <= 2 * 10 ^ 8`
- 对于所有的 `1 <= i < j <= points.length` ， `points[i][0] < points[j][0]` 都成立。也就是说， `xi` 是严格递增的。

__思路:__

```text
单调队列
注意到 x 已经排序, 必然有 xj >= xi
将 max(yi + yj + xj - xi) 转化为 max(xj + yj) - min(xi - yi)
队列中按递减顺序保留 xi - yi 
每次遍历时将不符合 k 的弹出
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findMaxValueOfEquation(vector<vector<int>>& points, int k) 
    {
        deque<vector<int>> dq;
        int result = INT_MIN;
        for (const auto& p: points) 
        {
            while (!dq.empty() and p.front() - dq.front().front() > k) dq.pop_front();
            if (!dq.empty()) result = max(result, p.front() + p.back() - dq.front().front() + dq.front().back());
            while (!dq.empty() and dq.back().back() - dq.back().front() < p.back() - p.front()) dq.pop_back();
            dq.push_back(p);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findMaxValueOfEquation(int[][] points, int k) {
        Deque<int[]> deque = new LinkedList<>();
        int result = Integer.MIN_VALUE;
        for (int[] p: points) {
            while (!deque.isEmpty() && p[0] - deque.peekFirst()[0] > k) deque.pollFirst();
            if (!deque.isEmpty()) result = Math.max(result, p[0] + p[1] - deque.peekFirst()[0] + deque.peekFirst()[1]);
            while (!deque.isEmpty() && deque.peekLast()[1] - deque.peekLast()[0] < p[1] - p[0]) deque.pollLast();
            deque.offerLast(p);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findMaxValueOfEquation(self, points: List[List[int]], k: int) -> int:
        queue, result = deque(), -float('inf')
        for p in points:
            while queue and p[0] - queue[0][0] > k:
                queue.popleft()
            if queue:
                result = max(result, sum(p) + queue[0][1] - queue[0][0])
            while queue and p[1] - p[0] > queue[-1][1] - queue[-1][0]:
                queue.pop()
            queue.append(p)
        return result
```
