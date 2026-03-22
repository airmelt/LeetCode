# 2500 Delete Greatest Value in Each Row 删除每行中的最大值

__Description:__

You are given an `m x n` matrix `grid` consisting of positive integers.

Perform the following operation until `grid` becomes empty:

- Delete the element with the greatest value from each row. If multiple such elements exist, delete any of them.
- Add the maximum of deleted elements to the answer.

Note that the number of columns decreases by one after each operation.

Return the answer after performing the operations described above.

__Example:__

Example 1:

![2500-1](https://assets.leetcode.com/uploads/2022/10/19/q1ex1.jpg)

```text
Input: grid = [[1,2,4],[3,3,1]]
Output: 8
Explanation: The diagram above shows the removed values in each step.
```

- In the first operation, we remove 4 from the first row and 3 from the second row (notice that, there are two cells with value 3 and we can remove any of them). We add 4 to the answer.
- In the second operation, we remove 2 from the first row and 3 from the second row. We add 3 to the answer.
- In the third operation, we remove 1 from the first row and 1 from the second row. We add 1 to the answer.

The final answer = 4 + 3 + 1 = 8.

Example 2:

![2500-2](https://assets.leetcode.com/uploads/2022/10/19/q1ex2.jpg)

```text
Input: grid = [[10]]
Output: 10
Explanation: The diagram above shows the removed values in each step.
```

- In the first operation, we remove 10 from the first row. We add 10 to the answer.

The final answer = 10.

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `1 <= grid[i][j] <= 100`

__题目描述:__

给你一个 `m x n` 大小的矩阵 `grid` ，由若干正整数组成。

执行下述操作，直到 `grid` 变为空矩阵:

- 从每一行删除值最大的元素。如果存在多个这样的值，删除其中任何一个。
- 将删除元素中的最大值与答案相加。

注意 每执行一次操作，矩阵中列的数据就会减 1 。

返回执行上述操作后的答案。

__示例:__

示例 1：

![2500-3](https://assets.leetcode.com/uploads/2022/10/19/q1ex1.jpg)

```text
输入：grid = [[1,2,4],[3,3,1]]
输出：8
解释：上图展示在每一步中需要移除的值。
```

- 在第一步操作中，从第一行删除 4 ，从第二行删除 3（注意，有两个单元格中的值为 3 ，我们可以删除任一）。在答案上加 4 。
- 在第二步操作中，从第一行删除 2 ，从第二行删除 3 。在答案上加 3 。
- 在第三步操作中，从第一行删除 1 ，从第二行删除 1 。在答案上加 1 。

最终，答案 = 4 + 3 + 1 = 8 。

示例 2：

![2500-4](https://assets.leetcode.com/uploads/2022/10/19/q1ex2.jpg)

```text
输入：grid = [[10]]
输出：10
解释：上图展示在每一步中需要移除的值。
```

- 在第一步操作中，从第一行删除 10 。在答案上加 10 。
最终，答案 = 10 。

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `1 <= grid[i][j] <= 100`

__思路:__

```text
贪心
对每一行进行排序
每次取出每一列的最大值
累加到结果中
时间复杂度为 O(MNlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int deleteGreatestValue(vector<vector<int>>& grid) 
    {
        for (auto& row : grid) sort(row.begin(), row.end());
        int result = 0;
        for (int j = 0, n = grid.front().size(), mx = 0; j < n; j++, mx = 0) 
        {
            for (const auto& row : grid) mx = max(mx, row[j]);
            result += mx;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int deleteGreatestValue(int[][] grid) {
        for (var row : grid) Arrays.sort(row);
        int result = 0;
        for (int j = 0, n = grid[0].length, mx = 0; j < n; j++, mx = 0) {
            for (var row : grid) mx = Math.max(mx, row[j]);
            result += mx;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def deleteGreatestValue(self, grid: List[List[int]]) -> int:
        return sum(map(max, zip(*[sorted(row) for row in grid])))
```
