# 2106 Maximum Fruits Harvested After at Most K Steps 摘水果

__Description:__

Fruits are available at some positions on an infinite x-axis. You are given a 2D integer array `fruits` where `fruits[i] = [positioni, amounti]` depicts `amounti` fruits at the position `positioni`. `fruits` is already __sorted__ by `positioni` in __ascending order__, and each `positioni` is __unique__.

You are also given an integer `startPos` and an integer `k`. Initially, you are at the position `startPos`. From any position, you can either walk to the __left or right__. It takes __one step__ to move __one unit__ on the x-axis, and you can walk __at most__ `k` steps in total. For every position you reach, you harvest all the fruits at that position, and the fruits will disappear from that position.

Return the maximum total number of fruits you can harvest.

__Example:__

Example 1:

![2106-1](https://assets.leetcode.com/uploads/2021/11/21/1.png)

```text
Input: fruits = [[2,8],[6,3],[8,6]], startPos = 5, k = 4
Output: 9
Explanation: 
The optimal way is to:
```

- Move right to position 6 and harvest 3 fruits
- Move right to position 8 and harvest 6 fruits

You moved 3 steps and harvested 3 + 6 = 9 fruits in total.

Example 2:

![2106-2](https://assets.leetcode.com/uploads/2021/11/21/2.png)

```text
Input: fruits = [[0,9],[4,1],[5,7],[6,2],[7,4],[10,9]], startPos = 5, k = 4
Output: 14
Explanation: 
You can move at most k = 4 steps, so you cannot reach position 0 nor 10.
The optimal way is to:
```

- Harvest the 7 fruits at the starting position 5
- Move left to position 4 and harvest 1 fruit
- Move right to position 6 and harvest 2 fruits
- Move right to position 7 and harvest 4 fruits

You moved 1 + 3 = 4 steps and harvested 7 + 1 + 2 + 4 = 14 fruits in total.

Example 3:

![2106-3](https://assets.leetcode.com/uploads/2021/11/21/3.png)

```text
Input: fruits = [[0,3],[6,4],[8,5]], startPos = 3, k = 2
Output: 0
Explanation:
You can move at most k = 2 steps and cannot reach any position with fruits.
```

__Constraints:__

- `1 <= fruits.length <= 10 ^ 5`
- `fruits[i].length == 2`
- `0 <= startPos, positioni <= 2 * 10 ^ 5`
- `positioni-1 < positioni` for any `i > 0` (__0-indexed__)
- `1 <= amounti <= 10 ^ 4`
- `0 <= k <= 2 * 10 ^ 5`

__题目描述:__

在一个无限的 x 坐标轴上，有许多水果分布在其中某些位置。给你一个二维整数数组 `fruits` ，其中 `fruits[i] = [positioni, amounti]` 表示共有 `amounti` 个水果放置在 `positioni` 上。 `fruits` 已经按 `positioni` __升序排列__ ，每个 `positioni` __互不相同__ 。

另给你两个整数 `startPos` 和 `k` 。最初，你位于 `startPos` 。从任何位置，你可以选择 __向左或者向右__ 走。在 x 轴上每移动 __一个单位__ ，就记作 __一步__ 。你总共可以走 __最多__ `k` 步。你每达到一个位置，都会摘掉全部的水果，水果也将从该位置消失（不会再生）。

返回你可以摘到水果的 最大总数 。

__示例:__

示例 1：

![2106-4](https://assets.leetcode.com/uploads/2021/11/21/1.png)

```text
输入：fruits = [[2,8],[6,3],[8,6]], startPos = 5, k = 4
输出：9
解释：
最佳路线为：
```

- 向右移动到位置 6 ，摘到 3 个水果
- 向右移动到位置 8 ，摘到 6 个水果

移动 3 步，共摘到 3 + 6 = 9 个水果

示例 2：

![2106-5](https://assets.leetcode.com/uploads/2021/11/21/2.png)

```text
输入：fruits = [[0,9],[4,1],[5,7],[6,2],[7,4],[10,9]], startPos = 5, k = 4
输出：14
解释：
可以移动最多 k = 4 步，所以无法到达位置 0 和位置 10 。
最佳路线为：
```

- 在初始位置 5 ，摘到 7 个水果
- 向左移动到位置 4 ，摘到 1 个水果
- 向右移动到位置 6 ，摘到 2 个水果
- 向右移动到位置 7 ，摘到 4 个水果

移动 1 + 3 = 4 步，共摘到 7 + 1 + 2 + 4 = 14 个水果

示例 3：

![2106-6](https://assets.leetcode.com/uploads/2021/11/21/3.png)

```text
输入：fruits = [[0,3],[6,4],[8,5]], startPos = 3, k = 2
输出：0
解释：
最多可以移动 k = 2 步，无法到达任一有水果的地方
```

__提示：__

- `1 <= fruits.length <= 10 ^ 5`
- `fruits[i].length == 2`
- `0 <= startPos, positioni <= 2 * 10 ^ 5`
- 对于任意 `i > 0` ， `positioni-1 < positioni` 均成立（下标从 __0__ 开始计数）
- `1 <= amounti <= 10 ^ 4`
- `0 <= k <= 2 * 10 ^ 5`

__思路:__

```text
二分查找 ➕ 滑动窗口
先找到 startPos - k 的位置, 作为左边界, 此时右边界为 startPos
统计 startPos - k 到 startPos 之间的水果数量, 作为初始值
然后从 startPos + 1 开始向右遍历, 直到最后一个水果的位置或者 startPos + k 的位置
右边界不断向右移动, 统计 startPos 到 startPos + k 之间的水果数量
并移除 right - left > k 的水果, 保证窗口大小不超过 k
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxTotalFruits(vector<vector<int>> &fruits, int startPos, int k) 
    {
        int left = lower_bound(fruits.begin(), fruits.end(), startPos - k, [](const auto &a, int b) { return a[0] < b; }) - fruits.begin(), right = left, s = 0, n = fruits.size(), result = 0;
        for (; right < n and fruits[right].front() <= startPos; ++right) s += fruits[right].back();
        for (result = s; right < n and fruits[right].front() <= startPos + k; ++right)
        {
            s += fruits[right].back();
            while ((fruits[right].front() << 1) - fruits[left].front() - startPos > k and fruits[right].front() - (fruits[left].front() << 1) + startPos > k) s -= fruits[left++].back();
            result = max(result, s);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxTotalFruits(int[][] fruits, int startPos, int k) {
        int left = helper(fruits, startPos - k), right = left, s = 0, result = 0, n = fruits.length;
        for (; right < n && fruits[right][0] <= startPos; right++) s += fruits[right][1];
        for (result = s; right < n && fruits[right][0] <= startPos + k; right++) {
            s += fruits[right][1];
            while ((fruits[right][0] << 1) - fruits[left][0] - startPos > k && fruits[right][0] - (fruits[left][0] << 1) + startPos > k) s -= fruits[left++][1];
            result = Math.max(result, s);
        } 
        return result;
    }

    private int helper(int[][] fruits, int target) {
        int left = 0, right = fruits.length;
        while (left < right) {
            int mid = left + ((right - left) >>> 1);
            if (fruits[mid][0] < target) left = mid + 1;
            else right = mid;
        }
        return left;
    }
}
```

__Python__:

```Python
class Solution:
    def maxTotalFruits(self, fruits: List[List[int]], startPos: int, k: int) -> int:
        left, result, s = bisect_left(fruits, [startPos - k]), 0, 0
        for pos, fruit in fruits[left:]:
            if pos > startPos + k:
                break
            s += fruit
            while (pos << 1) - fruits[left][0] - startPos > k and pos - (fruits[left][0] << 1) + startPos > k:
                s -= fruits[left][1]
                left += 1
            result = max(result, s)
        return result
```
