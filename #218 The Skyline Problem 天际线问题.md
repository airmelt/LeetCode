# 218 The Skyline Problem 天际线问题

__Description__:
A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Now suppose you are given the locations and height of all the buildings as shown on a cityscape photo (Figure A), write a program to output the skyline formed by these buildings collectively (Figure B).

![Figure](https://assets.leetcode.com/uploads/2020/12/01/merged.jpg)

The geometric information of each building is represented by a triplet of integers [Li, Ri, Hi], where Li and Ri are the x coordinates of the left and right edge of the ith building, respectively, and Hi is its height. It is guaranteed that 0 ≤ Li, Ri ≤ INT_MAX, 0 < Hi ≤ INT_MAX, and Ri - Li > 0. You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.

__Example:__

For instance, the dimensions of all buildings in Figure A are recorded as: [ [2 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ] .

The output is a list of "key points" (red dots in Figure B) in the format of [ [x1,y1], [x2, y2], [x3, y3], ... ] that uniquely defines a skyline. A key point is the left endpoint of a horizontal line segment. Note that the last key point, where the rightmost building ends, is merely used to mark the termination of the skyline, and always has zero height. Also, the ground in between any two adjacent buildings should be considered part of the skyline contour.

For instance, the skyline in Figure B should be represented as:[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0] ].

__Notes:__

The number of buildings in any input list is guaranteed to be in the range [0, 10000].
The input list is already sorted in ascending order by the left x position Li.
The output list must be sorted by the x position.
There must be no consecutive horizontal lines of equal height in the output skyline. For instance, [...[2 3], [4 5], [7 5], [11 5], [12 7]...] is not acceptable; the three lines of height 5 should be merged into one in the final output as such: [...[2 3], [4 5], [12 7], ...]

__题目描述__:
城市的天际线是从远处观看该城市中所有建筑物形成的轮廓的外部轮廓。现在，假设您获得了城市风光照片（图A）上显示的所有建筑物的位置和高度，请编写一个程序以输出由这些建筑物形成的天际线（图B）。

![图](https://assets.leetcode.com/uploads/2020/12/01/merged.jpg)

每个建筑物的几何信息用三元组 [Li，Ri，Hi] 表示，其中 Li 和 Ri 分别是第 i 座建筑物左右边缘的 x 坐标，Hi 是其高度。可以保证 0 ≤ Li, Ri ≤ INT_MAX, 0 < Hi ≤ INT_MAX 和 Ri - Li > 0。您可以假设所有建筑物都是在绝对平坦且高度为 0 的表面上的完美矩形。

__示例 :__

例如，图A中所有建筑物的尺寸记录为：[ [2 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ] 。

输出是以 [ [x1,y1], [x2, y2], [x3, y3], ... ] 格式的“关键点”（图B中的红点）的列表，它们唯一地定义了天际线。关键点是水平线段的左端点。请注意，最右侧建筑物的最后一个关键点仅用于标记天际线的终点，并始终为零高度。此外，任何两个相邻建筑物之间的地面都应被视为天际线轮廓的一部分。

例如，图B中的天际线应该表示为：[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0] ]。

__说明：__

任何输入列表中的建筑物数量保证在 [0, 10000] 范围内。
输入列表已经按左 x 坐标 Li  进行升序排列。
输出列表必须按 x 位排序。
输出天际线中不得有连续的相同高度的水平线。例如 [...[2 3], [4 5], [7 5], [11 5], [12 7]...] 是不正确的答案；三条高度为 5 的线应该在最终输出中合并为一个：[...[2 3], [4 5], [12 7], ...]

__思路__:
从左到右扫描
先将所有建筑加入一个列表, 由于之后需要进行排序, 且为了区分左右端点, 可以将左端点对应的高度设为负数, 这样排序出来的高度越小(实际上是高度的绝对值越大)排在前面
然后对列表进行排序
设置一个堆存放高度, 每次遇到左端点就加入高度, 遇到右端点就删除对应的高度, 如果高度发生变化, 记录到结果集
时间复杂度O(nlgn), 空间复杂度O(n), 需要对高度进行排序

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> getSkyline(vector<vector<int>>& buildings) 
    {
        vector<pair<int, int>> all;
        for (const auto& building : buildings) 
        {
            all.push_back(make_pair(building[0], -building[2]));
            all.push_back(make_pair(building[1], building[2]));
        }
        sort(all.begin(), all.end());
        vector<vector<int>> result;
        multiset<int> height{0};
        int prev = 0;
        for (const auto& building : all) 
        {
            if (building.second < 0) height.insert(-building.second);
            else height.erase(height.find(building.second));
            if (prev != *height.rbegin()) result.push_back({building.first, *height.rbegin()});
            prev = *height.rbegin();
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> getSkyline(int[][] buildings) {
        List<List<Integer>> result = new ArrayList<>();
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]);
        for (int[] building : buildings) {
            pq.offer(new int[] { building[0], -building[2] });
            pq.offer(new int[] { building[1], building[2] });
        }
        TreeMap<Integer, Integer> heights = new TreeMap<>((a, b) -> b - a);
        heights.put(0, 1);
        int left = 0, height = 0;
        while (!pq.isEmpty()) {
            int[] arr = pq.poll();
            if (arr[1] < 0) heights.put(-arr[1], heights.getOrDefault(-arr[1], 0) + 1);
            else {
                heights.put(arr[1], heights.get(arr[1]) - 1);
                if (heights.get(arr[1]) == 0) heights.remove(arr[1]);
            }
            int maxHeight = heights.keySet().iterator().next();
            if (maxHeight != height) {
                left = arr[0];
                height = maxHeight;
                result.add(Arrays.asList(left, height));
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getSkyline(self, buildings: List[List[int]]) -> List[List[int]]:
        buildings, result, heap = sorted([(L, -H, R) for L, R, H in buildings] + list({(R, 0, 0) for _, R, _ in buildings } )), [[0, 0]], [[0, float("inf")]]
        for x, H, R in buildings:
            while x >= heap[0][1]:
                heapq.heappop(heap)
            if H:
                heapq.heappush(heap, [H, R])
            if result[-1][1] != -heap[0][0]:
                result.append([x, -heap[0][0]])
        return result[1:]
```
