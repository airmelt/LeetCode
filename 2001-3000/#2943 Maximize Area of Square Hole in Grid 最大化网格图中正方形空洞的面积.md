# 2943 Maximize Area of Square Hole in Grid 最大化网格图中正方形空洞的面积

__Description:__

You are given the two integers, `n` and `m` and two integer arrays, `hBars` and `vBars`. The grid has `n + 2` horizontal and `m + 2` vertical bars, creating 1 x 1 unit cells. The bars are indexed starting from `1`.

You can __remove__ some of the bars in `hBars` from horizontal bars and some of the bars in `vBars` from vertical bars. Note that other bars are fixed and cannot be removed.

Return an integer denoting the maximum area of a square-shaped hole in the grid, after removing some bars (possibly none).

__Example:__

Example 1:

![2943-1](https://assets.leetcode.com/uploads/2023/11/05/screenshot-from-2023-11-05-22-40-25.png)

```text
Input: n = 2, m = 1, hBars = [2,3], vBars = [2]

Output: 4

Explanation:

The left image shows the initial grid formed by the bars. The horizontal bars are [1,2,3,4], and the vertical bars are [1,2,3].

One way to get the maximum square-shaped hole is by removing horizontal bar 2 and vertical bar 2.
```

Example 2:

![2943-2](https://assets.leetcode.com/uploads/2023/11/04/screenshot-from-2023-11-04-17-01-02.png)

```text
Input: n = 1, m = 1, hBars = [2], vBars = [2]

Output: 4

Explanation:

To get the maximum square-shaped hole, we remove horizontal bar 2 and vertical bar 2.
```

Example 3:

![2943-3](https://assets.leetcode.com/uploads/2024/03/12/unsaved-image-2.png)

```text
Input: n = 2, m = 3, hBars = [2,3], vBars = [2,4]

Output: 4

Explanation:

One way to get the maximum square-shaped hole is by removing horizontal bar 3, and vertical bar 4.
```

__Constraints:__

- `1 <= n <= 10 ^ 9`
- `1 <= m <= 10 ^ 9`
- `1 <= hBars.length <= 100`
- `2 <= hBars[i] <= n + 1`
- `1 <= vBars.length <= 100`
- `2 <= vBars[i] <= m + 1`
- All values in `hBars` are distinct.
- All values in `vBars` are distinct.

__题目描述:__

给你两个整数 `n` 和 `m`，以及两个整数数组 `hBars` 和 `vBars`。网格由 `n + 2` 条水平线和 `m + 2` 条竖直线组成，形成 1x1 的单元格。网格中的线条从 `1` 开始编号。

你可以从 `hBars` 中 __删除__ 一些水平线条，并从 `vBars` 中删除一些竖直线条。注意，其他线条是固定的，无法删除。

返回一个整数表示移除一些线条（可以不移除任何线条）后，网格中 正方形空洞的最大面积 。

__示例:__

示例 1：

![2943-4](https://assets.leetcode.com/uploads/2023/11/05/screenshot-from-2023-11-05-22-40-25.png)

```text
输入: n = 2, m = 1, hBars = [2,3], vBars = [2]

输出: 4

解释:

左侧图片展示了网格的初始状态。水平线是 [1,2,3,4]，竖直线是 [1,2,3]。

构造最大正方形空洞的一种方法是移除水平线 2 和竖直线 2。
```

示例 2：

![2943-5](https://assets.leetcode.com/uploads/2023/11/04/screenshot-from-2023-11-04-17-01-02.png)

```text
输入: n = 1, m = 1, hBars = [2], vBars = [2]

输出: 4

解释:

移除水平线 2 和竖直线 2，可以得到最大正方形空洞。
```

示例 3：

![2943-6](https://assets.leetcode.com/uploads/2024/03/12/unsaved-image-2.png)

```text
输入: n = 2, m = 3, hBars = [2,3], vBars = [2,4]

输出: 4

解释:

构造最大正方形空洞的一种方法是移除水平线 3 和竖直线 4。
```

__提示：__

- `1 <= n <= 10 ^ 9`
- `1 <= m <= 10 ^ 9`
- `1 <= hBars.length <= 100`
- `2 <= hBars[i] <= n + 1`
- `1 <= vBars.length <= 100`
- `2 <= vBars[i] <= m + 1`
- `hBars` 中所有值互不相同。
- `vBars` 中所有值互不相同。

__思路:__

```text
1. 排序
排序之后
尽可能移除连续的子数组
最长的连续子数组长度即为正方形的边长
时间复杂度为 O(HlogH + VlogV), 空间复杂度为 O(logH + logV), H 和 V 分别为两个数组的长度
2. 哈希表
参考 128 最长连续序列
将数组转化为哈希表
如果一个数是一段数组的开头
即 num - 1 不在哈希表中
从 num + 1 开始检查直到 num + x 不在哈希表中
这段长度就是可以移除的最长长度
因为是正方形
所以只要留下两边较短的边即可
时间复杂度为 O(H + V), 空间复杂度为 O(H + V), H 和 V 分别为两个数组的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximizeSquareHoleArea(int n, int m, vector<int>& hBars, vector<int>& vBars) 
    {
        auto helper = [](const auto& nums)
        {
            unordered_set<int> s(nums.begin(), nums.end());
            int result = 0;
            for (const auto& num : s) 
            {
                if (!s.count(num - 1)) 
                {
                    int next_num = num + 1;
                    while (s.count(next_num)) ++next_num;
                    result = max(result, next_num - num);
                }
            }
            return result;
        };
        return (n = min(helper(hBars), helper(vBars)) + 1) * n;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximizeSquareHoleArea(int n, int m, int[] hBars, int[] vBars) {
        return (n = Math.min(helper(hBars), helper(vBars)) + 1) * n;
    }

    private int helper(int[] nums) {
        var set = Arrays.stream(nums).boxed().collect(Collectors.toSet());
        int result = 0;
        for (int num : set) {
            if (!set.contains(num - 1)) {
                int nextNum = num + 1;
                while (set.contains(nextNum)) ++nextNum;
                result = Math.max(result, nextNum - num);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximizeSquareHoleArea(self, n: int, m: int, hBars: List[int], vBars: List[int]) -> int:
        def helper(nums: Set[int]) -> int:
            result = 0
            for num in nums:
                if num - 1 not in nums:
                    next_num = num + 1
                    while next_num in nums:
                        next_num += 1
                    result = max(result, next_num - num)
            return result
        return (min(helper(set(hBars)), helper(set(vBars))) + 1) ** 2
```
