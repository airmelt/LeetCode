# 2013 Detect Squares 检测正方形

__Description:__

You are given a stream of points on the X-Y plane. Design an algorithm that:

- __Adds__ new points from the stream into a data structure. __Duplicate__ points are allowed and should be treated as different points.
- Given a query point, __counts__ the number of ways to choose three points from the data structure such that the three points and the query point form an __axis-aligned square__ with __positive area__.

An axis-aligned square is a square whose edges are all the same length and are either parallel or perpendicular to the x-axis and y-axis.

Implement the `DetectSquares` class:

- `DetectSquares()` Initializes the object with an empty data structure.
- `void add(int[] point)` Adds a new point `point = [x, y]` to the data structure.
- `int count(int[] point)` Counts the number of ways to form __axis-aligned squares__ with point `point = [x, y]` as described above.

__Example:__

Example 1:

![2013-1](https://assets.leetcode.com/uploads/2021/09/01/image.png)

```text
Input
["DetectSquares", "add", "add", "add", "count", "count", "add", "count"]
[[], [[3, 10]], [[11, 2]], [[3, 2]], [[11, 10]], [[14, 8]], [[11, 2]], [[11, 10]]]
Output
[null, null, null, null, 1, 0, null, 2]

Explanation
```

```Java
DetectSquares detectSquares = new DetectSquares();
detectSquares.add([3, 10]);
detectSquares.add([11, 2]);
detectSquares.add([3, 2]);
detectSquares.count([11, 10]); // return 1. You can choose:
                               //   - The first, second, and third points
detectSquares.count([14, 8]);  // return 0. The query point cannot form a square with any points in the data structure.
detectSquares.add([11, 2]);    // Adding duplicate points is allowed.
detectSquares.count([11, 10]); // return 2. You can choose:
                               //   - The first, second, and third points
                               //   - The first, third, and fourth points
```

__Constraints:__

- `point.length == 2`
- `0 <= x, y <= 1000`
- At most `3000` calls __in total__ will be made to `add` and `count`.

__题目描述:__

给你一个在 X-Y 平面上的点构成的数据流。设计一个满足下述要求的算法：

- __添加__ 一个在数据流中的新点到某个数据结构中__。__可以添加 __重复__ 的点，并会视作不同的点进行处理。
- 给你一个查询点，请你从数据结构中选出三个点，使这三个点和查询点一同构成一个 __面积为正__ 的 __轴对齐正方形__ ，__统计__ 满足该要求的方案数目__。__

轴对齐正方形 是一个正方形，除四条边长度相同外，还满足每条边都与 x-轴 或 y-轴 平行或垂直。

实现 `DetectSquares` 类:

- `DetectSquares()` 使用空数据结构初始化对象
- `void add(int[] point)` 向数据结构添加一个新的点 `point = [x, y]`
- `int count(int[] point)` 统计按上述方式与点 `point = [x, y]` 共同构造 __轴对齐正方形__ 的方案数。

__示例:__

示例：

![2013-2](https://assets.leetcode.com/uploads/2021/09/01/image.png)

```text
输入：
["DetectSquares", "add", "add", "add", "count", "count", "add", "count"]
[[], [[3, 10]], [[11, 2]], [[3, 2]], [[11, 10]], [[14, 8]], [[11, 2]], [[11, 10]]]
输出：
[null, null, null, null, 1, 0, null, 2]

解释：
```

```Java
DetectSquares detectSquares = new DetectSquares();
detectSquares.add([3, 10]);
detectSquares.add([11, 2]);
detectSquares.add([3, 2]);
detectSquares.count([11, 10]); // 返回 1 。你可以选择：
                               //   - 第一个，第二个，和第三个点
detectSquares.count([14, 8]);  // 返回 0 。查询点无法与数据结构中的这些点构成正方形。
detectSquares.add([11, 2]);    // 允许添加重复的点。
detectSquares.count([11, 10]); // 返回 2 。你可以选择：
                               //   - 第一个，第二个，和第三个点
                               //   - 第一个，第三个，和第四个点
```

__提示：__

- `point.length == 2`
- `0 <= x, y <= 1000`
- 调用 `add` 和 `count` 的 __总次数__ 最多为 `5000`

__思路:__

```text
哈希表
用哈希表记录成对的坐标
data[x][y] 记录以 (x, y) 为坐标的点对的数量
在计算的时候取两个点 (x1, y1) 和 (x2, y2) 以及查询点 (x, y), 其中 x1 == x2, x != x1
然后根据乘法原理通过计算对称点以统计 (x, y) 为顶点的正方形的数量
初始化时间复杂度为 O(1), 空间复杂度为 O(1)
add 时间复杂度为 O(1), 空间复杂度为 O(1)
count 时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class DetectSquares 
{
public:
    DetectSquares() {}
    
    void add(vector<int> point) 
    {
        ++data[point.front()][point.back()];
    }
    
    int count(vector<int> point) 
    {
        int result = 0, x = point.front(), y = point.back();
        for (auto& [k, v] : data) if (k != x) result += v[y] * data[x][y + k - x] * v[y + k - x] + v[y] * data[x][y - k + x] * v[y - k + x];
        return result;
    }
private:
    unordered_map<int, unordered_map<int, int>> data;
};

/**
 * Your DetectSquares object will be instantiated and called as such:
 * DetectSquares* obj = new DetectSquares();
 * obj->add(point);
 * int param_2 = obj->count(point);
 */
```

__Java__:

```Java
class DetectSquares {
    Map<Integer, Map<Integer, Integer>> data;

    public DetectSquares() {
        data = new HashMap<>();
    }
    
    public void add(int[] point) {
        data.computeIfAbsent(point[0], k -> new HashMap<>());
        data.get(point[0]).merge(point[1], 1, Integer::sum);
    }
    
    public int count(int[] point) {
        int result = 0, x = point[0], y = point[1];
        for (Map.Entry<Integer, Map<Integer, Integer>> entry : data.entrySet()) if (entry.getKey() != point[0]) result += entry.getValue().getOrDefault(point[1], 0) * data.getOrDefault(point[0], new HashMap<>()).getOrDefault(point[1] + entry.getKey() - point[0], 0) * entry.getValue().getOrDefault(point[1] + entry.getKey() - point[0], 0) + entry.getValue().getOrDefault(point[1], 0) * data.getOrDefault(point[0], new HashMap<>()).getOrDefault(point[1] - entry.getKey() + point[0], 0) * entry.getValue().getOrDefault(point[1] - entry.getKey() + point[0], 0);
        return result;
    }
}

/**
 * Your DetectSquares object will be instantiated and called as such:
 * DetectSquares obj = new DetectSquares();
 * obj.add(point);
 * int param_2 = obj.count(point);
 */
```

__Python__:

```Python
class DetectSquares:

    def __init__(self):
        self.data = defaultdict(lambda: defaultdict(int))


    def add(self, point: List[int]) -> None:
        self.data[point[0]][point[1]] += 1


    def count(self, point: List[int]) -> int:
        return sum(v[point[1]] * self.data[point[0]][point[1] + k - point[0]] * v[point[1] + k - point[0]] + v[point[1]] * self.data[point[0]][point[1] - k + point[0]] * v[point[1] - k + point[0]] for k, v in copy.copy(self.data).items() if k != point[0])
        



# Your DetectSquares object will be instantiated and called as such:
# obj = DetectSquares()
# obj.add(point)
# param_2 = obj.count(point)
```
