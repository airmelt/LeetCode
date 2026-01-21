# 2857 Count Pairs of Points With Distance k 统计距离为 k 的点对

__Description:__

You are given a __2D__ integer array `coordinates` and an integer `k`, where `coordinates[i] = [xi, yi]` are the coordinates of the `i ^ th` point in a 2D plane.

We define the __distance__ between two points `(x1, y1)` and `(x2, y2)` as `(x1 XOR x2) + (y1 XOR y2)` where `XOR` is the bitwise `XOR` operation.

Return _the number of pairs_ `(i, j)` _such that_ `i < j` _and the distance between points_ `i` _and_ `j` _is equal to_ `k`.

__Example:__

Example 1:

```text
Input: coordinates = [[1,2],[4,2],[1,3],[5,2]], k = 5
Output: 2
Explanation: We can choose the following pairs:
```

- (0,1): Because we have (1 XOR 4) + (2 XOR 2) = 5.
- (2,3): Because we have (1 XOR 5) + (3 XOR 2) = 5.

Example 2:

```text
Input: coordinates = [[1,3],[1,3],[1,3],[1,3],[1,3]], k = 0
Output: 10
Explanation: Any two chosen pairs will have a distance of 0. There are 10 ways to choose two pairs.
```

__Constraints:__

- `2 <= coordinates.length <= 50000`
- `0 <= xi, yi <= 10 ^ 6`
- `0 <= k <= 100`

__题目描述:__

给你一个 __二维__ 整数数组 `coordinates` 和一个整数 `k` ，其中 `coordinates[i] = [xi, yi]` 是第 `i` 个点在二维平面里的坐标。

我们定义两个点 `(x1, y1)` 和 `(x2, y2)` 的 __距离__ 为 `(x1 XOR x2) + (y1 XOR y2)` ， `XOR` 指的是按位异或运算。

请你返回满足 `i < j` 且点 `i` 和点 `j`之间距离为 `k` 的点对数目。

__示例:__

示例 1：

```text
输入：coordinates = [[1,2],[4,2],[1,3],[5,2]], k = 5
输出：2
解释：以下点对距离为 k ：
```

- (0, 1)：(1 XOR 4) + (2 XOR 2) = 5 。
- (2, 3)：(1 XOR 5) + (3 XOR 2) = 5 。

示例 2：

```text
输入：coordinates = [[1,3],[1,3],[1,3],[1,3],[1,3]], k = 0
输出：10
解释：任何两个点之间的距离都为 0 ，所以总共有 10 组点对。
```

__提示：__

- `2 <= coordinates.length <= 50000`
- `0 <= xi, yi <= 10 ^ 6`
- `0 <= k <= 100`

__思路:__

```text
哈希表
用哈希表记录已经出现过的数对
由于数组元素都为非负数
x1 ^ x2 以及 y1 ^ y2 都会在 [0, k] 范围内
如果 x1 ^ x2 = i, 那么 y1 ^ y2 = k - i, 也都在 [0, k] 范围内
所以有 (x1 ^ i, y1 ^ (k - i)) = (x2, y2)
所以枚举 (x2, y2) 在哈希表中的数量累加到结果中即可
时间复杂度为 O(NK), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countPairs(vector<vector<int>>& coordinates, int k) 
    {
        unordered_map<long long, int> c;
        int result = 0;
        for (const auto& point : coordinates)
        {
            for (int i = 0; i <= k; i++) if (c.find((point.front() ^ i) * 123456LL + (point.back() ^ (k - i))) != c.end()) result += c[(point.front() ^ i) * 123456LL + (point.back() ^ (k - i))];
            ++c[point.front() * 123456LL + point.back()];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countPairs(List<List<Integer>> coordinates, int k) {
        var c = new HashMap<Long, Integer>();
        int result = 0;
        for (var point : coordinates) {
            for (int i = 0; i <= k; i++) result += c.getOrDefault((point.get(0) ^ i) * 123456L + (point.get(1) ^ (k - i)), 0);
            c.merge(point.get(0) * 123456L + point.get(1), 1, Integer::sum);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countPairs(self, coordinates: List[List[int]], k: int) -> int:
        c, result = Counter(), 0
        for x, y in coordinates:
            for i in range(k + 1):
                result += c[x ^ i, y ^ (k - i)]
            c[(x, y)] += 1
        return result

```
