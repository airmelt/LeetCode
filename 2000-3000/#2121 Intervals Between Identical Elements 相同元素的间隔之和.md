# 2121 Intervals Between Identical Elements 相同元素的间隔之和

__Description:__

You are given a __0-indexed__ array of `n` integers `arr`.

The __interval__ between two elements in `arr` is defined as the __absolute difference__ between their indices. More formally, the __interval__ between `arr[i]` and `arr[j]` is `|i - j|`.

Return _an array_ `intervals` _of length_ `n` _where_ `intervals[i]` _is __the sum of intervals__ between_ `arr[i]` _and each element in_ `arr` _with the same value as_ `arr[i]`_._

__Note:__ `|x|` is the absolute value of `x`.

__Example:__

Example 1:

```text
Input: arr = [2,1,3,1,2,3,3]
Output: [4,2,7,2,4,4,5]
Explanation:
```

- Index 0: Another 2 is found at index 4. |0 - 4| = 4
- Index 1: Another 1 is found at index 3. |1 - 3| = 2
- Index 2: Two more 3s are found at indices 5 and 6. |2 - 5| + |2 - 6| = 7
- Index 3: Another 1 is found at index 1. |3 - 1| = 2
- Index 4: Another 2 is found at index 0. |4 - 0| = 4
- Index 5: Two more 3s are found at indices 2 and 6. |5 - 2| + |5 - 6| = 4
- Index 6: Two more 3s are found at indices 2 and 5. |6 - 2| + |6 - 5| = 5

Example 2:

```text
Input: arr = [10,5,10,10]
Output: [5,0,3,4]
Explanation:
```

- Index 0: Two more 10s are found at indices 2 and 3. |0 - 2| + |0 - 3| = 5
- Index 1: There is only one 5 in the array, so its sum of intervals to identical elements is 0.
- Index 2: Two more 10s are found at indices 0 and 3. |2 - 0| + |2 - 3| = 3
- Index 3: Two more 10s are found at indices 0 and 2. |3 - 0| + |3 - 2| = 4

__Constraints:__

- `n == arr.length`
- `1 <= n <= 10 ^ 5`
- `1 <= arr[i] <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始、由 `n` 个整数组成的数组 `arr` 。

`arr` 中两个元素的 __间隔__ 定义为它们下标之间的 __绝对差__ 。更正式地， `arr[i]` 和 `arr[j]` 之间的间隔是 `|i - j|` 。

返回一个长度为 `n` 的数组 `intervals` ，其中 `intervals[i]` 是 `arr[i]` 和 `arr` 中每个相同元素（与 `arr[i]` 的值相同）的 __间隔之和__ _。_

__注意:__`|x|` 是 `x` 的绝对值。

__示例:__

示例 1：

```text
输入：arr = [2,1,3,1,2,3,3]
输出：[4,2,7,2,4,4,5]
解释：
```

- 下标 0 ：另一个 2 在下标 4 ，|0 - 4| = 4
- 下标 1 ：另一个 1 在下标 3 ，|1 - 3| = 2
- 下标 2 ：另两个 3 在下标 5 和 6 ，|2 - 5| + |2 - 6| = 7
- 下标 3 ：另一个 1 在下标 1 ，|3 - 1| = 2
- 下标 4 ：另一个 2 在下标 0 ，|4 - 0| = 4
- 下标 5 ：另两个 3 在下标 2 和 6 ，|5 - 2| + |5 - 6| = 4
- 下标 6 ：另两个 3 在下标 2 和 5 ，|6 - 2| + |6 - 5| = 5

示例 2：

```text
输入：arr = [10,5,10,10]
输出：[5,0,3,4]
解释：
```

- 下标 0 ：另两个 10 在下标 2 和 3 ，|0 - 2| + |0 - 3| = 5
- 下标 1 ：只有这一个 5 在数组中，所以到相同元素的间隔之和是 0
- 下标 2 ：另两个 10 在下标 0 和 3 ，|2 - 0| + |2 - 3| = 3
- 下标 3 ：另两个 10 在下标 0 和 2 ，|3 - 0| + |3 - 2| = 4

__提示：__

- `n == arr.length`
- `1 <= n <= 10 ^ 5`
- `1 <= arr[i] <= 10 ^ 5`

__思路:__

```text
数学
先按值记录每一个位置
比如 [2,1,3,1,2,3,3] 中 3 的位置为 v = [2, 5, 6]
对于第一个位置为 sum = sum(v) - v[0] * len(v)
若 m 为 v 的长度
对于第 i 个位置 (i > 0), 前 i 个位置增加了 v[i] - v[i - 1], 后 m - i 个位置减少了 v[i] - v[i - 1]
i * (v[i] - v[i - 1]) - (m - i) * (v[i] - v[i - 1]) = (2 * i - m) * (v[i] - v[i - 1])
所以 sum += ((i << 1) - m) * (v[i] - v[i - 1])
并且 result[v[i]] = sum
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<long long> getDistances(vector<int>& arr) 
    {
        unordered_map<int, vector<int>> graph;
        int n = arr.size();
        for (int i = 0; i < n; i++) graph[arr[i]].emplace_back(i);
        vector<long long> result(n);
        for (auto &[_, v] : graph) 
        {
            long sum = accumulate(v.begin(), v.end(), 0L) - v.size() * v.front();
            result[v.front()] = sum;
            for (int i = 1, m = v.size(); i < m; ++i) result[v[i]] = sum += ((i << 1) - m) * (v[i] - v[i - 1]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long[] getDistances(int[] arr) {
        var graph = new HashMap<Integer, List<Integer>>();
        var n = arr.length;
        for (var i = 0; i < n; i++) graph.computeIfAbsent(arr[i], k -> new ArrayList<>()).add(i);
        var result = new long[n];
        for (var v : graph.values()) {
            var sum = 0L;
            for (var i : v) sum += i - v.get(0);
            result[v.get(0)] = sum;
            for (int i = 1, m = v.size(); i < m; i++) result[v.get(i)] = sum += ((i << 1) - m) * (v.get(i) - v.get(i - 1));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getDistances(self, arr: List[int]) -> List[int]:
        graph, result = defaultdict(list), [0] * len(arr)
        for i, num in enumerate(arr):
            graph[num].append(i)
        for v in graph.values():
            n = len(v)
            result[v[0]] = s = sum(v) - n * v[0]
            for i in range(1, n):
                s += ((i << 1) - n) * (v[i] - v[i - 1])
                result[v[i]] = s
        return result
```
