# 2363 Merge Similar Items 合并相似的物品

__Description:__

You are given two 2D integer arrays, `items1` and `items2`, representing two sets of items. Each array `items` has the following properties:

- `items[i] = [value_i, weight_i]` where `value_i` represents the __value__ and `weight_i` represents the __weight__ of the `i ^ th` item.
- The value of each item in `items` is __unique__.

Return _a 2D integer array_ `ret` _where_ `ret[i] = [value_i, weight_i]`_,_ _with_ `weight_i` _being the __sum of weights__ of all items with value_ `value_i`.

__Note:__ `ret` should be returned in __ascending__ order by value.

__Example:__

Example 1:

```text
Input: items1 = [[1,1],[4,5],[3,8]], items2 = [[3,1],[1,5]]
Output: [[1,6],[3,9],[4,5]]
Explanation: 
The item with value = 1 occurs in items1 with weight = 1 and in items2 with weight = 5, total weight = 1 + 5 = 6.
The item with value = 3 occurs in items1 with weight = 8 and in items2 with weight = 1, total weight = 8 + 1 = 9.
The item with value = 4 occurs in items1 with weight = 5, total weight = 5.  
Therefore, we return [[1,6],[3,9],[4,5]].
```

Example 2:

```text
Input: items1 = [[1,1],[3,2],[2,3]], items2 = [[2,1],[3,2],[1,3]]
Output: [[1,4],[2,4],[3,4]]
Explanation: 
The item with value = 1 occurs in items1 with weight = 1 and in items2 with weight = 3, total weight = 1 + 3 = 4.
The item with value = 2 occurs in items1 with weight = 3 and in items2 with weight = 1, total weight = 3 + 1 = 4.
The item with value = 3 occurs in items1 with weight = 2 and in items2 with weight = 2, total weight = 2 + 2 = 4.
Therefore, we return [[1,4],[2,4],[3,4]].
```

Example 3:

```text
Input: items1 = [[1,3],[2,2]], items2 = [[7,1],[2,2],[1,4]]
Output: [[1,7],[2,4],[7,1]]
Explanation:
The item with value = 1 occurs in items1 with weight = 3 and in items2 with weight = 4, total weight = 3 + 4 = 7. 
The item with value = 2 occurs in items1 with weight = 2 and in items2 with weight = 2, total weight = 2 + 2 = 4. 
The item with value = 7 occurs in items2 with weight = 1, total weight = 1.
Therefore, we return [[1,7],[2,4],[7,1]].
```

__Constraints:__

- `1 <= items1.length, items2.length <= 1000`
- `items1[i].length == items2[i].length == 2`
- `1 <= value_i, weight_i <= 1000`
- Each `value_i` in `items1` is __unique__.
- Each `value_i` in `items2` is __unique__.

__题目描述:__

给你两个二维整数数组 `items1` 和 `items2` ，表示两个物品集合。每个数组 `items` 有以下特质:

- `items[i] = [value_i, weight_i]` 其中 `value_i` 表示第 `i` 件物品的 __价值__ ， `weight_i` 表示第 `i` 件物品的 __重量__ 。
- `items` 中每件物品的价值都是 __唯一的__ 。

请你返回一个二维数组 `ret`，其中 `ret[i] = [value_i, weight_i]`， `weight_i` 是所有价值为 `value_i` 物品的 __重量之和__ 。

__注意:__`ret` 应该按价值 __升序__ 排序后返回。

__示例:__

示例 1：

```text
输入：items1 = [[1,1],[4,5],[3,8]], items2 = [[3,1],[1,5]]
输出：[[1,6],[3,9],[4,5]]
解释：
value = 1 的物品在 items1 中 weight = 1 ，在 items2 中 weight = 5 ，总重量为 1 + 5 = 6 。
value = 3 的物品再 items1 中 weight = 8 ，在 items2 中 weight = 1 ，总重量为 8 + 1 = 9 。
value = 4 的物品在 items1 中 weight = 5 ，总重量为 5 。
所以，我们返回 [[1,6],[3,9],[4,5]] 。
```

示例 2：

```text
输入：items1 = [[1,1],[3,2],[2,3]], items2 = [[2,1],[3,2],[1,3]]
输出：[[1,4],[2,4],[3,4]]
解释：
value = 1 的物品在 items1 中 weight = 1 ，在 items2 中 weight = 3 ，总重量为 1 + 3 = 4 。
value = 2 的物品在 items1 中 weight = 3 ，在 items2 中 weight = 1 ，总重量为 3 + 1 = 4 。
value = 3 的物品在 items1 中 weight = 2 ，在 items2 中 weight = 2 ，总重量为 2 + 2 = 4 。
所以，我们返回 [[1,4],[2,4],[3,4]] 。
```

示例 3：

```text
输入：items1 = [[1,3],[2,2]], items2 = [[7,1],[2,2],[1,4]]
输出：[[1,7],[2,4],[7,1]]
解释：
value = 1 的物品在 items1 中 weight = 3 ，在 items2 中 weight = 4 ，总重量为 3 + 4 = 7 。
value = 2 的物品在 items1 中 weight = 2 ，在 items2 中 weight = 2 ，总重量为 2 + 2 = 4 。
value = 7 的物品在 items2 中 weight = 1 ，总重量为 1 。
所以，我们返回 [[1,7],[2,4],[7,1]] 。
```

__提示：__

- `1 <= items1.length, items2.length <= 1000`
- `items1[i].length == items2[i].length == 2`
- `1 <= value_i, weight_i <= 1000`
- `items1` 中每个 `value_i` 都是 _唯一的_ 。
- `items2` 中每个 `value_i` 都是 _唯一的_ 。

__思路:__

```text
哈希表
使用哈希表记录不同的 value 对应的 weight 之和
最后将哈希表中的元素转换为二维数组即可
再对二维数组按 value 升序排序
时间复杂度为 O((M + N)log(M + N)), 空间复杂度为 O(M + N), 其中 M 和 N 分别为 items1 和 items2 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> mergeSimilarItems(vector<vector<int>>& items1, vector<vector<int>>& items2) 
    {
        unordered_map<int, int> m;
        for (auto &p : items1) m[p[0]] += p[1];
        for (auto &p : items2) m[p[0]] += p[1];
        vector<vector<int>> result;
        for (auto &[k, v] : m) result.push_back({k, v});
        sort(result.begin(), result.end(), [](auto &a, auto &b) { return a.front() < b.front(); });
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> mergeSimilarItems(int[][] items1, int[][] items2) {
        var map = new HashMap<Integer, Integer>();
        for (var item : items1) map.merge(item[0], item[1], Integer::sum);
        for (var item : items2) map.merge(item[0], item[1], Integer::sum);
        var result = new ArrayList<List<Integer>>();
        for (var e : map.entrySet()) result.add(List.of(e.getKey(), e.getValue()));
        result.sort((a, b) -> (a.get(0) - b.get(0)));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def mergeSimilarItems(self, items1: List[List[int]], items2: List[List[int]]) -> List[List[int]]:
        return sorted((Counter(dict(items1)) + Counter(dict(items2))).items())
```
