# 587 Erect the Fence  安装栅栏

__Description__:
You are given an array trees where trees[i] = [xi, yi] represents the location of a tree in the garden.

You are asked to fence the entire garden using the minimum length of rope as it is expensive. The garden is well fenced only if all the trees are enclosed.

Return the coordinates of trees that are exactly located on the fence perimeter.

__Example:__

Example 1:

![erect-plane-1](https://assets.leetcode.com/uploads/2021/04/24/erect2-plane.jpg)

Input: points = [[1,1],[2,2],[2,0],[2,4],[3,3],[4,2]]
Output: [[1,1],[2,0],[3,3],[2,4],[4,2]]

Example 2:

![erect-plane-2](https://assets.leetcode.com/uploads/2021/04/24/erect1-plane.jpg)

Input: points = [[1,2],[2,2],[4,2]]
Output: [[4,2],[2,2],[1,2]]

__Constraints:__

1 <= points.length <= 3000
points[i].length == 2
0 <= xi, yi <= 100
All the given points are unique.

__题目描述__:
在一个二维的花园中，有一些用 (x, y) 坐标表示的树。由于安装费用十分昂贵，你的任务是先用最短的绳子围起所有的树。只有当所有的树都被绳子包围时，花园才能围好栅栏。你需要找到正好位于栅栏边界上的树的坐标。

__示例 :__

示例 1:
输入: [[1,1],[2,2],[2,0],[2,4],[3,3],[4,2]]
输出: [[1,1],[2,0],[4,2],[3,3],[2,4]]
解释:

![栅栏-1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/erect_the_fence_1.png)

示例 2:

输入: [[1,2],[2,2],[4,2]]
输出: [[1,2],[2,2],[4,2]]
解释:

![栅栏-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/erect_the_fence_2.png)

即使树都在一条直线上，你也需要先用绳子包围它们。

__注意:__

所有的树应当被围在一起。你不能剪断绳子来包围树或者把树分成一组以上。
输入的整数在 0 到 100 之间。
花园至少有一棵树。
所有树的坐标都是不同的。
输入的点没有顺序。输出顺序也没有要求。

__思路__:

凸包
先按照 x 坐标排序, 然后再按照 y 坐标排序
先将初始两个点加到栈中, 遍历数组, 对于每个新的点, 用叉积检查是否在最后两个点的逆时针方向, 是的话加入凸包中, 否则继续弹出元素, 直到 x 最大为止, 这样可以求出凸包的下半部分
然后按顺时针弹出元素构造凸包的上半部分
时间复杂度 O(nlgn), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
inline int cross(vector<int> &p, vector<int> &q, vector<int> &r) { return (q.back() - p.back()) * (r.front() - q.front()) - (q.front() - p.front()) * (r.back() - q.back()); }
class Solution 
{
public:
    vector<vector<int>> outerTrees(vector<vector<int>>& trees) 
    {
        vector<vector<int>> result;
        sort(trees.begin(), trees.end());
        for (int i = 0; i < trees.size(); i++)
        {
            while (result.size() >= 2 and cross(result[result.size() - 2], result[result.size() - 1], trees[i]) > 0) result.pop_back();
            result.push_back(trees[i]);
        }
        result.pop_back();
        for (int i = trees.size() - 1; i > -1; i--)
        {
            while (result.size() >= 2 and cross(result[result.size() - 2], result[result.size() - 1], trees[i]) > 0) result.pop_back();
            result.push_back(trees[i]);
        }
        set<vector<int>> s(result.begin(), result.end());
        result.assign(s.begin(), s.end());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int cross(int[] p, int[] q, int[] r) {
        return (q[1] - p[1]) * (r[0] - q[0]) - (q[0] - p[0]) * (r[1] - q[1]);
    }
    
    public int[][] outerTrees(int[][] trees) {
        Arrays.sort(trees, new Comparator<int[]>() {
            public int compare(int[] p, int[] q) {
                return q[0] - p[0] == 0 ? q[1] - p[1] : q[0] - p[0];
            }
        });
        Stack<int[]> hull = new Stack<>();
        for (int i = 0; i < trees.length; i++) {
            while (hull.size() >= 2 && cross(hull.get(hull.size() - 2), hull.get(hull.size() - 1), trees[i]) > 0) hull.pop();
            hull.push(trees[i]);
        }
        hull.pop();
        for (int i = trees.length - 1; i >= 0; i--) {
            while (hull.size() >= 2 && cross(hull.get(hull.size() - 2), hull.get(hull.size() - 1), trees[i]) > 0) hull.pop();
            hull.push(trees[i]);
        }
        List<int[]> list = new ArrayList<>(new HashSet<>(hull));
        int result[][] = new int[list.size()][];
        for (int i = 0; i < list.size(); i++) result[i] = new int[]{ list.get(i)[0], list.get(i)[1] };
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def outerTrees(self, trees: List[List[int]]) -> List[List[int]]:
        if n := len(trees) < 3:
            return trees
        trees.sort(key=lambda x: (x[0], x[1]))
        cross, low, up = lambda a, b, c: (b[0] - a[0]) * (c[1] - b[1]) - (b[1] - a[1]) * (c[0] - b[0]), [], []
        for p in trees:
            while len(low) > 1 and cross(low[-2], low[-1], p) < 0:
                low.pop()
            low.append((p[0], p[1]))
        for p in reversed(trees):
            while len(up) > 1 and cross(up[-2], up[-1], p) < 0:
                up.pop()
            up.append((p[0], p[1]))
        return list(set(low[:-1] + up[:-1]))
```
