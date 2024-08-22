# 624 Maximum Distance in Arrays 数组列表中的最大距离

__Description:__

You are given `m` `arrays`, where each array is sorted in __ascending order__.

You can pick up two integers from two different arrays (each array picks one) and calculate the distance. We define the distance between two integers `a` and `b` to be their absolute difference `|a - b|`.

Return the maximum distance.

__Example:__

Example 1:

```text
Input: arrays = [[1,2,3],[4,5],[1,2,3]]
Output: 4
Explanation: One way to reach the maximum distance 4 is to pick 1 in the first or third array and pick 5 in the second array.
```

Example 2:

```text
Input: arrays = [[1],[1]]
Output: 0
```

__Constraints:__

- `m == arrays.length`
- `2 <= m <= 10 ^ 5`
- `1 <= arrays[i].length <= 500`
- `-10 ^ 4 <= arrays[i][j] <= 10 ^ 4`
- `arrays[i]` is sorted in __ascending order__.
- There will be at most `10 ^ 5` integers in all the arrays.

__题目描述:__

给定 `m` 个数组，每个数组都已经按照升序排好序了。

现在你需要从两个不同的数组中选择两个整数（每个数组选一个）并且计算它们的距离。两个整数 `a` 和 `b` 之间的距离定义为它们差的绝对值 `|a-b|` 。

返回最大距离。

__示例:__

示例 1：

```text
输入：[[1,2,3],[4,5],[1,2,3]]
输出：4
解释：
一种得到答案 4 的方法是从第一个数组或者第三个数组中选择 1，同时从第二个数组中选择 5 。
```

示例 2：

```text
输入：arrays = [[1],[1]]
输出：0
```

__提示：__

- `m == arrays.length`
- `2 <= m <= 10 ^ 5`
- `1 <= arrays[i].length <= 500`
- `-10 ^ 4 <= arrays[i][j] <= 10 ^ 4`
- `arrays[i]` 以 __升序__ 排序。
- 所有数组中最多有 `10 ^ 5` 个整数。

__思路:__

```text
贪心
每个数组的最大值一定是最后一个元素, 最小值一定是第一个元素
所以我们只需要维护一个最大值和一个最小值即可
遍历数组, 每次更新最大值和最小值, 并计算当前数组的最大距离
由于是先更新结果再更新最大值和最小值, 所以不会出现同一个数组的最值比较的情况
两次计算的都是该组和其他组的最大距离
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxDistance(vector<vector<int>>& arrays) 
    {
        int result = 0, min_value = arrays.front().front(), max_value = arrays.front().back();
        for (int i = 1, m = arrays.size(); i < m; i++) 
        {
            result = max({result, abs(arrays[i].back() - min_value), abs(max_value - arrays[i].front())});
            min_value = min(min_value, arrays[i].front());
            max_value = max(max_value, arrays[i].back());
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxDistance(List<List<Integer>> arrays) {
        int result = 0, minValue = arrays.get(0).get(0), maxValue = arrays.get(0).get(arrays.get(0).size() - 1);
        for (int i = 1, m = arrays.size(); i < m; i++) {
            result = Math.max(result, Math.max(Math.abs(arrays.get(i).get(arrays.get(i).size() - 1) - minValue), Math.abs(maxValue - arrays.get(i).get(0))));
            minValue = Math.min(minValue, arrays.get(i).get(0));
            maxValue = Math.max(maxValue, arrays.get(i).get(arrays.get(i).size() - 1));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxDistance(self, arrays: List[List[int]]) -> int:
        result, min_value, max_value, m = 0, arrays[0][0], arrays[0][-1], len(arrays)
        for i in range(1, m):
            result, min_value, max_value = max(result, max(abs(arrays[i][-1] - min_value), abs(max_value - arrays[i][0]))), min(min_value, arrays[i][0]), max(max_value, arrays[i][-1])
        return result
```
