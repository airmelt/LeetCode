# 2813 Maximum Elegance of a K-Length Subsequence 子序列最大优雅度

__Description:__

You are given a __0-indexed__ 2D integer array `items` of length `n` and an integer `k`.

`items[i] = [profit_i, category_i]`, where `profit_i` and `category_i` denote the profit and category of the `i ^ th` item respectively.

Let's define the __elegance__ of a __subsequence__ of `items` as `total_profit + distinct_categories ^ 2`, where `total_profit` is the sum of all profits in the subsequence, and `distinct_categories` is the number of __distinct__ categories from all the categories in the selected subsequence.

Your task is to find the __maximum elegance__ from all subsequences of size `k` in `items`.

Return _an integer denoting the maximum elegance of a subsequence of_ `items` _with size exactly_ `k`.

Note: A subsequence of an array is a new array generated from the original array by deleting some elements (possibly none) without changing the remaining elements' relative order.

__Example:__

Example 1:

```text
Input: items = [[3,2],[5,1],[10,1]], k = 2
Output: 17
Explanation: In this example, we have to select a subsequence of size 2.
We can select items[0] = [3,2] and items[2] = [10,1].
The total profit in this subsequence is 3 + 10 = 13, and the subsequence contains 2 distinct categories [2,1].
Hence, the elegance is 13 + 22 = 17, and we can show that it is the maximum achievable elegance.
```

Example 2:

```text
Input: items = [[3,1],[3,1],[2,2],[5,3]], k = 3
Output: 19
Explanation: In this example, we have to select a subsequence of size 3. 
We can select items[0] = [3,1], items[2] = [2,2], and items[3] = [5,3]. 
The total profit in this subsequence is 3 + 2 + 5 = 10, and the subsequence contains 3 distinct categories [1,2,3]. 
Hence, the elegance is 10 + 32 = 19, and we can show that it is the maximum achievable elegance.
```

Example 3:

```text
Input: items = [[1,1],[2,1],[3,1]], k = 3
Output: 7
Explanation: In this example, we have to select a subsequence of size 3. 
We should select all the items. 
The total profit will be 1 + 2 + 3 = 6, and the subsequence contains 1 distinct category [1]. 
Hence, the maximum elegance is 6 + 12 = 7.
```

__Constraints:__

- `1 <= items.length == n <= 10 ^ 5`
- `items[i].length == 2`
- `items[i][0] == profit_i`
- `items[i][1] == category_i`
- `1 <= profit_i <= 10 ^ 9`
- `1 <= category_i <= n`
- `1 <= k <= n`

__题目描述:__

给你一个长度为 `n` 的二维整数数组 `items` 和一个整数 `k` 。

`items[i] = [profit_i, category_i]`，其中 `profit_i` 和 `category_i` 分别表示第 `i` 个项目的利润和类别。

现定义 `items` 的 __子序列__ 的 __优雅度__ 可以用 `total_profit + distinct_categories ^ 2` 计算，其中 `total_profit` 是子序列中所有项目的利润总和， `distinct_categories` 是所选子序列所含的所有类别中不同类别的数量。

你的任务是从 `items` 所有长度为 `k` 的子序列中，找出 __最大优雅度__ 。

用整数形式表示并返回 `items` 中所有长度恰好为 `k` 的子序列的最大优雅度。

注意：数组的子序列是经由原数组删除一些元素（可能不删除）而产生的新数组，且删除不改变其余元素相对顺序。

__示例:__

示例 1：

```text
输入：items = [[3,2],[5,1],[10,1]], k = 2
输出：17
解释：
在这个例子中，我们需要选出长度为 2 的子序列。
其中一种方案是 items[0] = [3,2] 和 items[2] = [10,1] 。
子序列的总利润为 3 + 10 = 13 ，子序列包含 2 种不同类别 [2,1] 。
因此，优雅度为 13 + 22 = 17 ，可以证明 17 是可以获得的最大优雅度。
```

示例 2：

```text
输入：items = [[3,1],[3,1],[2,2],[5,3]], k = 3
输出：19
解释：
在这个例子中，我们需要选出长度为 3 的子序列。 
其中一种方案是 items[0] = [3,1] ，items[2] = [2,2] 和 items[3] = [5,3] 。
子序列的总利润为 3 + 2 + 5 = 10 ，子序列包含 3 种不同类别 [1, 2, 3] 。 
因此，优雅度为 10 + 32 = 19 ，可以证明 19 是可以获得的最大优雅度。
```

示例 3：

```text
输入：items = [[1,1],[2,1],[3,1]], k = 3
输出：7
解释：
在这个例子中，我们需要选出长度为 3 的子序列。
我们需要选中所有项目。
子序列的总利润为 1 + 2 + 3 = 6，子序列包含 1 种不同类别 [1] 。
因此，最大优雅度为 6 + 12 = 7 。
```

__提示：__

- `1 <= items.length == n <= 10 ^ 5`
- `items[i].length == 2`
- `items[i][0] == profit_i`
- `items[i][1] == category_i`
- `1 <= profit_i <= 10 ^ 9`
- `1 <= category_i <= n`
- `1 <= k <= n`

__思路:__

```text
贪心
按照利润从大到小排序
用一个哈希集合 visited 记录出现过的种类
先将所有小于 k 的 item 全部加入结果中
如果出现的是重复的种类那么把这个重复种类的利润存放在一个栈中
这样这个栈中的利润会是一个从大到小排好序的
然后对于不小于 k 的 item
如果有重复的种类的利润, 考虑增加种类替换
最终利润为 max(total_profit + len(visited) ** 2)
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 主要时间瓶颈在排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long findMaximumElegance(vector<vector<int>>& items, int k) 
    {
        long long result = 0LL, totalProfit = 0LL;
        unordered_set<int> visited;
        stack<int> duplicate;
        sort(items.begin(), items.end(), [](auto a, auto b) { return a.front() > b.front(); });
        for (int i = 0, n = items.size(); i < n; i++) 
        {
            if (i < k) 
            {
                totalProfit += items[i][0];
                if (!visited.insert(items[i][1]).second) duplicate.push(items[i][0]);
            } 
            else if (!duplicate.empty() and visited.insert(items[i][1]).second) 
            {
                totalProfit += items[i][0] - duplicate.top();
                duplicate.pop();
            }
            result = max(result, totalProfit + (long long)visited.size() * (long long)visited.size());
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long findMaximumElegance(int[][] items, int k) {
        long result = 0L, totalProfit = 0L;
        var visited = new HashSet<Integer>();
        var duplicate = new Stack<Integer>();
        Arrays.sort(items, (a, b) -> b[0] - a[0]);
        for (int i = 0, n = items.length; i < n; i++) {
            if (i < k) {
                totalProfit += items[i][0];
                if (!visited.add(items[i][1])) duplicate.push(items[i][0]);
            } else if (!duplicate.isEmpty() && visited.add(items[i][1])) totalProfit += items[i][0] - duplicate.pop();
            result = Math.max(result, totalProfit + (long)visited.size() * visited.size());
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findMaximumElegance(self, items: List[List[int]], k: int) -> int:
        items.sort(key=lambda i: -i[0])
        result, total_profit, visited, duplicate = 0, 0, set(), []
        for i, (profit, category) in enumerate(items):
            if i < k:
                total_profit += profit
                if category not in visited:
                    visited.add(category)
                else:
                    duplicate.append(profit)
            elif duplicate and category not in visited:
                visited.add(category)
                total_profit += profit - duplicate.pop()
            result = max(result, total_profit + len(visited) ** 2)
        return result
```
