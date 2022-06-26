# 850 Rectangle Area II 矩形面积 II

__Description__:
You are given a 2D array of axis-aligned rectangles. Each rectangle[i] = [xi1, yi1, xi2, yi2] denotes the ith rectangle where (xi1, yi1) are the coordinates of the bottom-left corner, and (xi2, yi2) are the coordinates of the top-right corner.

Calculate the total area covered by all rectangles in the plane. Any area covered by two or more rectangles should only be counted once.

Return the total area. Since the answer may be too large, return it modulo 109 + 7.

__Example:__

Example 1:

[图片上传失败...(image-e15534-1655993271478)]

Input: rectangles = [[0,0,2,2],[1,0,2,3],[1,0,3,1]]
Output: 6
Explanation: A total area of 6 is covered by all three rectangales, as illustrated in the picture.
From (1,1) to (2,2), the green and red rectangles overlap.
From (1,0) to (2,3), all three rectangles overlap.

Example 2:

Input: rectangles = [[0,0,1000000000,1000000000]]
Output: 49
Explanation: The answer is 10^18 modulo (10^9 + 7), which is 49.

__Constraints:__

1 <= rectangles.length <= 200
rectanges[i].length == 4
0 <= xi1, yi1, xi2, yi2 <= 10^9

__题目描述__:
我们给出了一个（轴对齐的）二维矩形列表 rectangles 。 对于 rectangle[i] = [x1, y1, x2, y2]，其中（x1，y1）是矩形 i 左下角的坐标， (xi1, yi1) 是该矩形 左下角 的坐标， (xi2, yi2) 是该矩形 右上角 的坐标。

计算平面中所有 rectangles 所覆盖的 总面积 。任何被两个或多个矩形覆盖的区域应只计算 一次 。

返回 总面积 。因为答案可能太大，返回 109 + 7 的 模 。

__示例 :__

示例 1：

[图片上传失败...(image-4e5a14-1655993271478)]

输入：rectangles = [[0,0,2,2],[1,0,2,3],[1,0,3,1]]
输出：6
解释：如图所示，三个矩形覆盖了总面积为6的区域。
从(1,1)到(2,2)，绿色矩形和红色矩形重叠。
从(1,0)到(2,3)，三个矩形都重叠。

示例 2：

输入：rectangles = [[0,0,1000000000,1000000000]]
输出：49
解释：答案是 10^18 对 (10^9 + 7) 取模的结果， 即 49 。

__提示:__

1 <= rectangles.length <= 200
rectanges[i].length = 4
0 <= xi1, yi1, xi2, yi2 <= 10^9
矩形叠加覆盖后的总面积不会超越 2^63 - 1 ，这意味着可以用一个 64 位有符号整数来保存面积结果。

__思路__:

离散化 ➕ 扫描线 ➕ 线段树

1. 离散化
构建差分数组时, 可以操作区间 [start, end], 通过 arr[start] + k, arr[end + 1] - k 来给整段区间加上 k 值
这里由于矩形的范围非常宽, 可以用哈希表记录每个端点通过排序的方式得到对应的排名
rank = {e:i + 1 for i, e in enumerate(sorted(set(arr)))}
比如 [1, 22, 333, 4444, 55555] 就会被映射为 {1: 1, 22: 2, 333: 3, 4444: 4, 55555: 5} 而不需要所有的数组中的数字范围空间仅需要数组长度的哈希表空间
2. 扫描线
直接添加矩形难以记录及计算面积
可以想象有一条平行于 x 轴的扫描线从下(这里是从 y = 0 开始)竖直往上扫描, 记录所有的 x 的范围, 则矩形面积可以通过 (x_max - x_min) * (y - pre_y) 得到
比如示例 1 中扫描线从 y = 0 开始到 y = 1 结束, 此时 x_max = 3, x_min = 0, y - pre_y = 1, 这一部分面积为 3
第二段从 y = 1 开始到 y = 2 结束, 此时 x_max = 2, x_min = 0, y - pre_y = 1, 这一部分面积为 2
第三段从 y = 2 开始到 y = 3 结束, 此时 x_max = 2, x_min = 1, y - pre_y = 1, 这一部分面积为 1
总计面积为 6
3. 线段树
为了统计每一条扫描线的长度需要使用能够记录区间的线段树
在线段树的端点加入 count 用来记录端点代表的线段被覆盖的次数 len 代表端点对应的线段长度
当加入端点的时候在端点上 count 自增
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    const int N = 1e9 + 7;
    vector<int> vals;
    struct Node
    {
        int left, right, count, len;
    } tr[4000];
    struct segment
    {
        int x, y1, y2, d;
        bool operator <(const segment &t)const
        {
            return x < t.x;
        }
    } seg[400];

    void build(int u, int left, int right)
    {
        tr[u] = {left, right, 0, 0};
        if (left != right)
        {
            int mid = left + ((right - left) >> 1);
            build(u << 1, left, mid);
            build(u << 1 | 1, mid + 1, right);
        }
    }

    void pushup(int u)
    {
        if (tr[u].count) tr[u].len = vals[tr[u].right + 1] - vals[tr[u].left];
        else if (tr[u].left != tr[u].right ) tr[u].len = tr[u << 1].len + tr[u << 1 | 1].len;
        else tr[u].len = 0;
    }

    void modify(int u,int left,int right,int k)
    {
        if (tr[u].left >= left and tr[u].right <= right)
        {
            tr[u].count += k;
            pushup(u);
            return;
        }
        int mid = tr[u].left + tr[u].right >> 1;
        if (left <= mid) modify(u << 1, left, right, k);
        if (right > mid) modify(u << 1 | 1, left, right, k);
        pushup(u);
    }

    int find(int x)
    {
        return lower_bound(vals.begin(), vals.end(), x) - vals.begin();
    }
public:
    int rectangleArea(vector<vector<int>>& rectangles) 
    {
        long long result = 0;
        int j = 0;
        for (const auto r : rectangles)
        {
            seg[j++] = {r[0], r[1], r[3], 1};
            seg[j++] = {r[2], r[1], r[3], -1};
            vals.push_back(r[1]);
            vals.push_back(r[3]);
        }
        sort(seg, seg + j);
        sort(vals.begin(), vals.end());
        vals.erase(unique(vals.begin(), vals.end()), vals.end());
        build(1, 0, vals.size() - 2);
        for (int i = 0; i < j; i++)
        {
            if (i) result += ((long long)(seg[i].x - seg[i - 1].x) * tr[1].len);
            modify(1, find(seg[i].y1), find(seg[i].y2) - 1, seg[i].d);
        }
        return (int)(result % N);
    }
};
```

__Java__:

```Java
class Solution {
    public int rectangleArea(int[][] rectangles) {
        int n = rectangles.length, m = 0, events[][] = new int[n << 1][4], mod = 1_000_000_007;
        Set<Integer> xVals = new HashSet<>();
        for (int[] r : rectangles) {
            xVals.add(r[0]);
            xVals.add(r[2]);
            events[m++] = new int[]{r[1], 1, r[0], r[2]};
            events[m++] = new int[]{r[3], -1, r[0], r[2]};
        }
        Arrays.sort(events, new Comparator<>() {
            public int compare(int[] a,int[] b) {
                return a[0] - b[0];
            }  
        });
        Integer[] x = xVals.toArray(new Integer[0]);
        Arrays.sort(x);
        Map<Integer,Integer> map = new HashMap<>();
        for (int i = 0; i < x.length; i++) map.put(x[i], i);
        long result = 0, total = 0;
        int prev = events[0][0];
        SegmentTree st = new SegmentTree(x);
        for (int[] e : events) {
            int y = e[0], val = e[1], x1 = e[2], x2 = e[3];
            result += total * (y - prev);
            st.update(st.root, map.get(x1), map.get(x2), val);
            total = st.query(st.root, 0, x.length - 1);
            prev = y;
        }
        return (int)(result % mod);
    }
}

class SegmentTree {
    Node root = null;
    Integer[] x;
    int n;
    public SegmentTree(Integer[] x) {
        this.n = x.length;
        this.x = x;
        root = new Node(0, n - 1);
    }
    
    public void update(Node root, int start, int end, int val) {
        if (start <= root.start && end >= root.end) {
            root.count += val;
            return;
        }
        int mid = root.start + ((root.end - root.start) >>> 1);
        if (start < mid) {
            Node left = root.left;
            if (root.left == null) {
                left = new Node(root.start,mid);
                root.left = left;
            }
            update(left,start,end,val);
        }
        if (mid < end) {
            Node right = root.right;
            if (root.right == null) {
                right = new Node(mid, root.end);
                root.right = right;
            }
            update(right, start, end, val);
        }
    }
    
    public long query(Node root, int start, int end) {
        if (start == root.start && end == root.end) if(root.count > 0) return x[root.end] - x[root.start];
        long result = 0;
        int mid = root.start + ((root.end - root.start) >>> 1);
        if (root.left != null) result += query(root.left, start, mid);
        if (root.right != null) result += query(root.right, mid, end);
        return  result;
    }
}

class Node {
    int start;
    int end;
    Node left;
    Node right;
    int count = 0;
    public Node(int start,int end) {
        this.start = start;
        this.end = end;
    }
}
```

__Python__:

```Python
class Solution:
    def rectangleArea(self, rectangles: List[List[int]]) -> int:
        mod, result, xs = 10 ** 9 + 7, 0, sorted(set([x1 for x1, y1, x2, y2 in rectangles] + [x2 for x1, y1, x2, y2 in rectangles]))
        for i in range(1, len(xs)):
            d, px, x, count = defaultdict(int), xs[i - 1], xs[i], 0
            for x1, y1, x2, y2 in rectangles:
                if x1 <= px <= x <= x2:
                    d[y1] += 1
                    d[y2] -= 1
            for y in sorted(d):
                if not count:
                    py = y
                count += d[y]
                if not count:
                    result += (x - px) * (y - py)
        return result % mod
```
