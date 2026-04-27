# 1840 Maximum Building Height 最高建筑高度

__Description:__

You want to build `n` new buildings in a city. The new buildings will be built in a line and are labeled from `1` to `n`.

However, there are city restrictions on the heights of the new buildings:

- The height of each building must be a non-negative integer.
- The height of the first building __must__ be `0`.
- The height difference between any two adjacent buildings __cannot exceed__ `1`.

Additionally, there are city restrictions on the maximum height of specific buildings. These restrictions are given as a 2D integer array `restrictions` where `restrictions[i] = [idi, maxHeighti]` indicates that building `idi` must have a height __less than or equal to__ `maxHeighti`.

It is guaranteed that each building will appear __at most once__ in `restrictions`, and building `1` will __not__ be in `restrictions`.

Return the maximum possible height of the tallest building.

__Example:__

Example 1:

![1840-1](https://assets.leetcode.com/uploads/2021/04/08/ic236-q4-ex1-1.png)

```text
Input: n = 5, restrictions = [[2,1],[4,1]]
Output: 2
Explanation: The green area in the image indicates the maximum allowed height for each building.
We can build the buildings with heights [0,1,2,1,2], and the tallest building has a height of 2.
```

Example 2:

![1840-2](https://assets.leetcode.com/uploads/2021/04/08/ic236-q4-ex2.png)

```text
Input: n = 6, restrictions = []
Output: 5
Explanation: The green area in the image indicates the maximum allowed height for each building.
We can build the buildings with heights [0,1,2,3,4,5], and the tallest building has a height of 5.
```

Example 3:

![1840-3](https://assets.leetcode.com/uploads/2021/04/08/ic236-q4-ex3.png)

```text
Input: n = 10, restrictions = [[5,3],[2,5],[7,4],[10,3]]
Output: 5
Explanation: The green area in the image indicates the maximum allowed height for each building.
We can build the buildings with heights [0,1,2,3,3,4,4,5,4,3], and the tallest building has a height of 5.
```

__Constraints:__

- `2 <= n <= 10 ^ 9`
- `0 <= restrictions.length <= min(n - 1, 10 ^ 5)`
- `2 <= idi <= n`
- `idi` is __unique__.
- `0 <= maxHeighti <= 10 ^ 9`

__题目描述:__

在一座城市里，你需要建 `n` 栋新的建筑。这些新的建筑会从 `1` 到 `n` 编号排成一列。

这座城市对这些新建筑有一些规定：

- 每栋建筑的高度必须是一个非负整数。
- 第一栋建筑的高度 __必须__ 是 `0` 。
- 任意两栋相邻建筑的高度差 __不能超过__  `1` 。

除此以外，某些建筑还有额外的最高高度限制。这些限制会以二维整数数组 `restrictions` 的形式给出，其中 `restrictions[i] = [idi, maxHeighti]` ，表示建筑 `idi` 的高度 __不能超过__ `maxHeighti` 。

题目保证每栋建筑在 `restrictions` 中 __至多出现一次__ ，同时建筑 `1` __不会__ 出现在 `restrictions` 中。

请你返回 最高 建筑能达到的 最高高度 。

__示例:__

示例 1：

![1840-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/04/25/ic236-q4-ex1-1.png)

```text
输入：n = 5, restrictions = [[2,1],[4,1]]
输出：2
解释：上图中的绿色区域为每栋建筑被允许的最高高度。
我们可以使建筑高度分别为 [0,1,2,1,2] ，最高建筑的高度为 2 。
```

示例 2：

![1840-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/04/25/ic236-q4-ex2.png)

```text
输入：n = 6, restrictions = []
输出：5
解释：上图中的绿色区域为每栋建筑被允许的最高高度。
我们可以使建筑高度分别为 [0,1,2,3,4,5] ，最高建筑的高度为 5 。
```

示例 3：

![1840-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/04/25/ic236-q4-ex3.png)

```text
输入：n = 10, restrictions = [[5,3],[2,5],[7,4],[10,3]]
输出：5
解释：上图中的绿色区域为每栋建筑被允许的最高高度。
我们可以使建筑高度分别为 [0,1,2,3,3,4,4,5,4,3] ，最高建筑的高度为 5 。
```

__提示：__

- `2 <= n <= 10 ^ 9`
- `0 <= restrictions.length <= min(n - 1, 10 ^ 5)`
- `2 <= idi <= n`
- `idi` 是 __唯一的__ 。
- `0 <= maxHeighti <= 10 ^ 9`

__思路:__

```text
数学
对任意限制, 其实限制了整个建筑
因为限制为 [i, h] 实际上也限制了 i + 1 和 i - 1 不超过 h + 1 即 j 不超过 h + abs(i - j)
只考虑在限制数组中出现的建筑
因为在限制数组中出现的建筑中间会按照山脉数组的方式形成建筑群
设两个建筑分别为 [i, hi], [j, hj] 且 i < j
最理想的情况是从 hi 开始先递增到最大值 hmax, 然后递减到 j 的高度 hj
即 hmax - hi + hmax - hj = (j - i)
所以 hmax = (j - i + hi + hj) / 2
先将限制数组按照位置排序
为了方便计算我们加入 [1, 0] 和 [n, n - 1] 两个哨兵
分别从左到右和从右到左遍历限制数组, 将限制高度减少到不超过前一个限制的高度加上两个建筑之间的距离
最后遍历一次数组更新限制高度即可
时间复杂度为 O(MlogM), 空间复杂度为 O(logM), M 为限制数组的长度, 主要是排序的开销
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxBuilding(int n, vector<vector<int>>& restrictions) 
    {
        restrictions.push_back({1, 0});
        sort(restrictions.begin(), restrictions.end());
        if (restrictions.back().front() != n) restrictions.push_back({n, n - 1});
        int m = restrictions.size(), result = 0;
        for (int i = 1; i < m; i++) restrictions[i].back() = min(restrictions[i].back(), restrictions[i - 1].back() + (restrictions[i].front() - restrictions[i - 1].front()));
        for (int i = m - 1; i > 0; i--)  restrictions[i - 1].back() = min(restrictions[i - 1].back(), restrictions[i].back() + (restrictions[i].front() - restrictions[i - 1].front()));
        for (int i = 1; i < m; i++) result = max(result, ((restrictions[i].front() - restrictions[i - 1].front()) + restrictions[i].back() + restrictions[i - 1].back()) >> 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxBuilding(int n, int[][] restrictions) {
        Arrays.sort(restrictions, (a, b) -> a[0] - b[0]);
        int m = restrictions.length;
        if (m > 0) restrictions[0][1] = Math.min(restrictions[0][1], restrictions[0][0] - 1);
        for (int i = 1; i < m; i++) restrictions[i][1] = Math.min(restrictions[i][1], restrictions[i - 1][1] + (restrictions[i][0] - restrictions[i - 1][0]));
        for(int i=restrictions.length-2;i>=0;i--){restrictions[i][1]=Math.min(restrictions[i][1],restrictions[i+1][1]+(restrictions[i+1][0]-restrictions[i][0]));}
        int result = (m == 0) ? n - 1 : ((restrictions[0][0] + restrictions[0][1] - 1) >> 1);
        for (int i = 1; i < m; i++) result = Math.max(result, ((restrictions[i][0] + restrictions[i][1] + restrictions[i - 1][1] - restrictions[i - 1][0]) >> 1));
        return m == 0 ? result : Math.max(result, restrictions[m - 1][1] + n - restrictions[m - 1][0]);
    }
}
```

__Python__:

```Python
class Solution:
    def maxBuilding(self, n: int, restrictions: List[List[int]]) -> int:
        restrictions.append([1, 0])
        restrictions.sort()
        if restrictions[-1][0] != n:
            restrictions.append([n, n - 1])
        m = len(restrictions)
        for i in range(1, m):
            restrictions[i][1] = min(restrictions[i][1], restrictions[i - 1][1] + (restrictions[i][0] - restrictions[i - 1][0]))
        for i in range(m - 1, 0, -1):
            restrictions[i - 1][1] = min(restrictions[i - 1][1], restrictions[i][1] + (restrictions[i][0] - restrictions[i - 1][0]))
        return max((restrictions[i][0] - restrictions[i - 1][0] + restrictions[i][1] + restrictions[i - 1][1]) >> 1 for i in range(1, m))
```
