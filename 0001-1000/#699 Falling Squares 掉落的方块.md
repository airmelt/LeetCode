# 699 Falling Squares 掉落的方块

__Description__:
There are several squares being dropped onto the X-axis of a 2D plane.

You are given a 2D integer array positions where positions[i] = [lefti, sideLengthi] represents the ith square with a side length of sideLengthi that is dropped with its left edge aligned with X-coordinate lefti.

Each square is dropped one at a time from a height above any landed squares. It then falls downward (negative Y direction) until it either lands on the top side of another square or on the X-axis. A square brushing the left/right side of another square does not count as landing on it. Once it lands, it freezes in place and cannot be moved.

After each square is dropped, you must record the height of the current tallest stack of squares.

Return an integer array ans where ans[i] represents the height described above after dropping the ith square.

__Example:__

Example 1:

![Falling Squares](https://assets.leetcode.com/uploads/2021/04/28/fallingsq1-plane.jpg)

Input: positions = [[1,2],[2,3],[6,1]]
Output: [2,5,5]
Explanation:
After the first drop, the tallest stack is square 1 with a height of 2.
After the second drop, the tallest stack is squares 1 and 2 with a height of 5.
After the third drop, the tallest stack is still squares 1 and 2 with a height of 5.
Thus, we return an answer of [2, 5, 5].

Example 2:

Input: positions = [[100,100],[200,100]]
Output: [100,100]
Explanation:
After the first drop, the tallest stack is square 1 with a height of 100.
After the second drop, the tallest stack is either square 1 or square 2, both with heights of 100.
Thus, we return an answer of [100, 100].
Note that square 2 only brushes the right side of square 1, which does not count as landing on it.

__Constraints:__

1 <= positions.length <= 1000
1 <= lefti <= 10^8
1 <= sideLengthi <= 10^6

__题目描述__:
在无限长的数轴（即 x 轴）上，我们根据给定的顺序放置对应的正方形方块。

第 i 个掉落的方块（positions[i] = (left, side_length)）是正方形，其中 left 表示该方块最左边的点位置(positions[i][0])，side_length 表示该方块的边长(positions[i][1])。

每个方块的底部边缘平行于数轴（即 x 轴），并且从一个比目前所有的落地方块更高的高度掉落而下。在上一个方块结束掉落，并保持静止后，才开始掉落新方块。

方块的底边具有非常大的粘性，并将保持固定在它们所接触的任何长度表面上（无论是数轴还是其他方块）。邻接掉落的边不会过早地粘合在一起，因为只有底边才具有粘性。

返回一个堆叠高度列表 ans 。每一个堆叠高度 ans[i] 表示在通过 positions[0], positions[1], ..., positions[i] 表示的方块掉落结束后，目前所有已经落稳的方块堆叠的最高高度。

__示例 :__

示例 1:

输入: [[1, 2], [2, 3], [6, 1]]
输出: [2, 5, 5]
解释:

第一个方块 positions[0] = [1, 2] 掉落：

```text
_aa
_aa
-------
```

方块最大高度为 2 。

第二个方块 positions[1] = [2, 3] 掉落：

```text
__aaa
__aaa
__aaa
_aa__
_aa__
--------------
```

方块最大高度为5。
大的方块保持在较小的方块的顶部，不论它的重心在哪里，因为方块的底部边缘有非常大的粘性。

第三个方块 positions[1] = [6, 1] 掉落：

```text
__aaa
__aaa
__aaa
_aa
_aa___a
-------------- 
```

方块最大高度为5。

因此，我们返回结果[2, 5, 5]。

示例 2:

输入: [[100, 100], [200, 100]]
输出: [100, 100]
解释: 相邻的方块不会过早地卡住，只有它们的底部边缘才能粘在表面上。

__注意:__

1 <= positions.length <= 1000.
1 <= positions[i][0] <= 10^8.
1 <= positions[i][1] <= 10^6.

__思路__:

1. 模拟
由于数据量不大
可以采用直接模拟
每次掉落一个方块, 更新它后面的所有方块的高度, 因为前面的掉落不会影响到后面的方块, 这样做是没问题的
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)
2. 线段树
这个题本质上还是区间更新及查询的问题
方块都是连续的, 不过不需要对区间中间的进行操作, 所以可以将坐标离散化
用 TreeMap 记录左右边界 [left, left + size - 1]
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    int a[10005], mark[10005];

    void build(int p, int l, int r) 
    {
        if (l == r) 
        {
            mark[p] = a[p] = 0;
            return;
        }
        int mid = l + (r - l >> 1);
        build(p << 1, l, mid);
        build(p << 1 | 1, mid + 1, r);
        a[p] = max(a[p << 1], a[p << 1 | 1]);
    }

    void push_down(int p) 
    {
        mark[p << 1] = max(mark[p], mark[p << 1]);
        mark[p << 1 | 1] = max(mark[p], mark[p << 1 | 1]);
        a[p << 1] = max(a[p << 1], mark[p << 1]);
        a[p << 1 | 1] = max(a[p << 1 | 1], mark[p << 1 | 1]);
        mark[p] = 0;
    }

    int query(int p, int l, int r, int ql, int qr) 
    {
        if (qr < l or ql > r) return 0;
        else if (ql <= l and qr >= r) return a[p];
        else 
        {
            if (mark[p]) push_down(p);
            int mid = l + (r - l >> 1);
            if (qr <= mid) return query(p << 1, l, mid, ql, qr);
            else if (ql > mid) return query(p << 1 | 1, mid + 1, r, ql, qr);
            else return max(query(p << 1, l, mid, ql, qr),query(p << 1 | 1, mid + 1, r, ql, qr));
        }
    }

    void update(int p, int l, int r, int ul, int ur, int val) 
    {
        if (ur < l or ul > r) return;
        else if (ul <= l and ur >= r) 
        {
            a[p] = max(a[p], val);
            mark[p] = max(mark[p], val);
        } 
        else 
        {
            if (mark[p]) push_down(p);
            int mid = l + (r - l >> 1);
            update(p << 1, l, mid, ul, ur, val);
            update(p << 1 | 1, mid + 1, r, ul, ur, val);
            a[p] = max(a[p << 1], a[p << 1 | 1]);
        }
    }

public:
    vector<int> fallingSquares(vector<vector<int>>& positions) 
    {
        vector<int> pos;
        for (auto &p : positions) 
        {
            pos.push_back(p.front());
            pos.push_back(p.front() + p.back() - 1);
        }
        sort(pos.begin(), pos.end());
        pos.erase(unique(pos.begin(), pos.end()), pos.end());
        int n = pos.size(), height = 0;
        unordered_map<int, int> m;
        for (int i = 0; i < n; ++i) m[pos[i]] = i;
        build(1, 0, n - 1);
        vector<int> result;
        for (auto &p : positions) 
        {
            int ql = m[p.front()], qr = m[p.front() + p.back() - 1];
            int h = query(1, 0, n - 1, ql, qr);
            height = max(height, h + p.back());
            result.push_back(height);
            update(1, 0, n - 1, ql, qr, h + p.back());
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> fallingSquares(int[][] positions) {
        List<Integer> result = new ArrayList<>();
        int n = positions.length;
        int[] squares = new int[n];
        for (int i = 0; i < n; i++) {
            int left1 = positions[i][0], height1 = positions[i][1], right1 = positions[i][0] + positions[i][1];
            squares[i] += height1;
            for (int j = i + 1; j < n; j++) {
                int left2 = positions[j][0], height2 = positions[j][1], right2 = positions[j][0] + positions[j][1];
                if (left2 < right1 && left1 < right2) squares[j] = Math.max(squares[i], squares[j]);
            }
        }
        for (int s : squares) result.add(result.isEmpty() ? s : Math.max(result.get(result.size() - 1), s));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def fallingSquares(self, positions: List[List[int]]) -> List[int]:
        squares, result = [0] * (n := len(positions)), []
        for i, (left, size) in enumerate(positions):
            right = left + size
            squares[i] += size
            for j in range(i + 1, n):
                left2, size2 = positions[j]
                right2 = left2 + size2
                if left2 < right and left < right2:
                    squares[j] = max(squares[j], squares[i])
        for s in squares:
            result.append(max(result[-1], s) if result else s)
        return result
```
