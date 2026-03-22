# 973 K Closest Points to Origin 最接近原点的 K 个点

__Description__:
Given an array of points where points[i] = [xi, yi] represents a point on the X-Y plane and an integer k, return the k closest points to the origin (0, 0).

The distance between two points on the X-Y plane is the Euclidean distance (i.e., √(x1 - x2)2 + (y1 - y2)2).

You may return the answer in any order. The answer is guaranteed to be unique (except for the order that it is in).

__Example:__

Example 1:

![Points](https://assets.leetcode.com/uploads/2021/03/03/closestplane1.jpg)

Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].

Example 2:

Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]
Explanation: The answer [[-2,4],[3,3]] would also be accepted.

__Constraints:__

1 <= k <= points.length <= 10^4
-104 < xi, yi < 10^4

__题目描述__:
我们有一个由平面上的点组成的列表 points。需要从中找出 K 个距离原点 (0, 0) 最近的点。

（这里，平面上两点之间的距离是欧几里德距离。）

你可以按任何顺序返回答案。除了点坐标的顺序之外，答案确保是唯一的。

__示例 :__

示例 1：

输入：points = [[1,3],[-2,2]], K = 1
输出：[[-2,2]]
解释：
(1, 3) 和原点之间的距离为 sqrt(10)，
(-2, 2) 和原点之间的距离为 sqrt(8)，
由于 sqrt(8) < sqrt(10)，(-2, 2) 离原点更近。
我们只需要距离原点最近的 K = 1 个点，所以答案就是 [[-2,2]]。

示例 2：

输入：points = [[3,3],[5,-1],[-2,4]], K = 2
输出：[[3,3],[-2,4]]
（答案 [[-2,4],[3,3]] 也会被接受。）

__提示:__

1 <= K <= points.length <= 10000
-10000 < points[i][0] < 10000
-10000 < points[i][1] < 10000

__思路__:

1. 排序
直接按照欧几里得距离的平方进行排序
时间复杂度为 O(nlgn), 空间复杂度为 O(lgn)
2. 堆
准备一个大小为 k 的大顶堆
当堆空间足够时直接插入, 当空间超过 k 时弹出堆顶元素
时间复杂度为 O(nlgk), 空间复杂度为 O(k)
3. 快速排序
即选择第 k 大的元素
时间复杂度为 O(n), 空间复杂度为 O(lgn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) 
    {
        nth_element(points.begin(), points.begin() + k, points.end(), [](auto&& a, auto&& b) { return a.front() * a.front() + a.back() * a.back() < b.front() * b.front() + b.back() * b.back(); });
        points.resize(k);
        return points;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        // return Arrays.stream(points).sorted(Comparator.comparingInt((p) -> p[0] * p[0] + p[1] * p[1])).limit(k).toArray(int[][]::new);
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>(new Comparator<int[]>() {
            public int compare(int[] array1, int[] array2) {
                return array2[0] - array1[0];
            }
        });
        for (int i = 0; i < k; ++i) pq.offer(new int[]{points[i][0] * points[i][0] + points[i][1] * points[i][1], i});
        for (int i = k, n = points.length; i < n; ++i) {
            int dist = points[i][0] * points[i][0] + points[i][1] * points[i][1];
            if (dist < pq.peek()[0]) {
                pq.poll();
                pq.offer(new int[]{dist, i});
            }
        }
        int[][] result = new int[k][2];
        for (int i = 0; i < k; ++i) result[i] = points[pq.poll()[1]];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        return sorted(points, key=lambda point: point[0] ** 2 + point[1] ** 2)[:k]
```
