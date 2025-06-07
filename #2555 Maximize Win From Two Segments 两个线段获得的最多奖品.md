# 2555 Maximize Win From Two Segments 两个线段获得的最多奖品

__Description:__

There are some prizes on the __X-axis__. You are given an integer array `prizePositions` that is __sorted in non-decreasing order__, where `prizePositions[i]` is the position of the `i ^ th` prize. There could be different prizes at the same position on the line. You are also given an integer `k`.

You are allowed to select two segments with integer endpoints. The length of each segment must be `k`. You will collect all prizes whose position falls within at least one of the two selected segments (including the endpoints of the segments). The two selected segments may intersect.

- For example if `k = 2`, you can choose segments `[1, 3]` and `[2, 4]`, and you will win any prize i that satisfies `1 <= prizePositions[i] <= 3` or `2 <= prizePositions[i] <= 4`.

Return the maximum number of prizes you can win if you choose the two segments optimally.

__Example:__

Example 1:

```text
Input: prizePositions = [1,1,2,2,3,3,5], k = 2
Output: 7
Explanation: In this example, you can win all 7 prizes by selecting two segments [1, 3] and [3, 5].
```

Example 2:

```text
Input:  prizePositions = [1,2,3,4], k = 0
Output:  2
Explanation:  For this example, one choice for the segments is `[3, 3]` and `[4, 4],` and you will be able to get `2` prizes.
```

__Constraints:__

- `1 <= prizePositions.length <= 10 ^ 5`
- `1 <= prizePositions[i] <= 10 ^ 9`
- `0 <= k <= 10 ^ 9`
- `prizePositions` is sorted in non-decreasing order.

__题目描述:__

在 __X轴__ 上有一些奖品。给你一个整数数组 `prizePositions` ，它按照 __非递减__ 顺序排列，其中 `prizePositions[i]` 是第 `i` 件奖品的位置。数轴上一个位置可能会有多件奖品。再给你一个整数 `k` 。

你可以同时选择两个端点为整数的线段。每个线段的长度都必须是 `k` 。你可以获得位置在任一线段上的所有奖品（包括线段的两个端点）。注意，两个线段可能会有相交。

- 比方说 `k = 2` ，你可以选择线段 `[1, 3]` 和 `[2, 4]` ，你可以获得满足 `1 <= prizePositions[i] <= 3` 或者 `2 <= prizePositions[i] <= 4` 的所有奖品 i 。

请你返回在选择两个最优线段的前提下，可以获得的 最多 奖品数目。

__示例:__

示例 1：

```text
输入：prizePositions = [1,1,2,2,3,3,5], k = 2
输出：7
解释：这个例子中，你可以选择线段 [1, 3] 和 [3, 5] ，获得 7 个奖品。
```

示例 2：

```text
输入: prizePositions = [1,2,3,4], k = 0
输出: 2
解释: 这个例子中，一个选择是选择线段 `[3, 3]` 和 `[4, 4]` ，获得 2 个奖品。
```

__提示：__

- `1 <= prizePositions.length <= 10 ^ 5`
- `1 <= prizePositions[i] <= 10 ^ 9`
- `0 <= k <= 10 ^ 9`
- `prizePositions` 有序非递减。

__思路:__

```text
滑动窗口
使用两段滑动窗口
设第二段右边界为 right, 第一段左边界为 left, 中间的窗口为 mid
可以将数组分为 [left, mid] 和 [mid, right] 两段
每次移动 mid, 右边界 right 向右扩展, 直到不满足 prizePositions[right] - prizePositions[mid] <= k, 此时 right 在第一个不满足的位置上, 即第二段实际上应该为 [mid, right - 1]
长度为 right - mid
然后计算当前的最大值 result = max(result, mx + right - mid)
同时需要维护第一段的最大长度 mx, 即 [left, mid] 的长度
当 prizePositions[mid] - prizePositions[left] > k 时, 需要将 left 向右移动, 直到满足条件
mx 更新为 max(mx, mid - left + 1)
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int maximizeWin(vector<int>& prizePositions, int k) {
        
    }
};
```

__Java__:

```Java
class Solution {
    public int maximizeWin(int[] prizePositions, int k) {
        int n = prizePositions.length, result = 0, mx = 0, left = 0, right = 0;
        if ((k << 1) >= prizePositions[n - 1] - prizePositions[0]) return n;
        for (int mid = 0; mid < n; mid++) {
            while (right < n && prizePositions[right] - prizePositions[mid] <= k) ++right;
            result = Math.max(result, mx + right - mid);
            while (prizePositions[mid] - prizePositions[left] > k) ++left;
            mx = Math.max(mx, mid - left + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximizeWin(self, prizePositions: List[int], k: int) -> int:
        n = len(prizePositions)
        if k << 1 >= prizePositions[-1] - prizePositions[0]:
            return n
        result = mx = left = right = 0
        for mid, p in enumerate(prizePositions):
            while right < n and prizePositions[right] - p <= k:
                right += 1
            result = max(result, mx + right - mid)
            while p - prizePositions[left] > k:
                left += 1
            mx = max(mx, mid - left + 1)
        return result
```
