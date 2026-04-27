# 1703 Minimum Adjacent Swaps for K Consecutive Ones 得到连续K个1的最少相邻交换次数

__Description:__

You are given an integer array, `nums`, and an integer `k`. `nums` comprises of only `0`'s and `1`'s. In one move, you can choose two __adjacent__ indices and swap their values.

Return _the __minimum__ number of moves required so that_ `nums` _has_ `k` ___consecutive___ `1`_'s_.

__Example:__

Example 1:

```text
Input: nums = [1,0,0,1,0,1], k = 2
Output: 1
Explanation: In 1 move, nums could be [1,0,0,0,1,1] and have 2 consecutive 1's.
```

Example 2:

```text
Input: nums = [1,0,0,0,0,0,1,1], k = 3
Output: 5
Explanation: In 5 moves, the leftmost 1 can be shifted right until nums = [0,0,0,0,0,1,1,1].
```

Example 3:

```text
Input: nums = [1,1,0,1], k = 2
Output: 0
Explanation: nums already has 2 consecutive 1's.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `nums[i]` is `0` or `1`.
- `1 <= k <= sum(nums)`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` 。 `nums` 仅包含 `0` 和 `1` 。每一次移动，你可以选择 __相邻__ 两个数字并将它们交换。

请你返回使 `nums` 中包含 `k` 个 __连续__ `1` 的 __最少__ 交换次数。

__示例:__

示例 1：

```text
输入：nums = [1,0,0,1,0,1], k = 2
输出：1
解释：在第一次操作时，nums 可以变成 [1,0,0,0,1,1] 得到连续两个 1 。
```

示例 2：

```text
输入：nums = [1,0,0,0,0,0,1,1], k = 3
输出：5
解释：通过 5 次操作，最左边的 1 可以移到右边直到 nums 变为 [0,0,0,0,0,1,1,1] 。
```

示例 3：

```text
输入：nums = [1,1,0,1], k = 2
输出：0
解释：nums 已经有连续 2 个 1 了。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `nums[i]` 要么是 `0` ，要么是 `1` 。
- `1 <= k <= sum(nums)`

__思路:__

```text
滑动窗口
记录所有 1 出现的位置
为了求出连续 k 个 1, 需要找到第一次连续 k 个 1 的窗口
用 t 记录 1 的出现次数, 统计 sum(pos[t] - pos[t - 1] - 1), 即连续 1 之间出现的 0 的长度, 得到第一次连续 k 个 1 的总长度, t 初始化为 1
从上一个 1 移动到下一个 1, 首先需要加上下一个 1 的长度 pos[left] - pos[left + mid], 再减去上一个 1 的长度 pos[left + k - mid] - pos[left + k], 这里用 mid = k << 1 是因为两端的 1 可以向中间移动, 移动的长度就是一半
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minMoves(vector<int>& nums, int k) 
    {
        int n = nums.size(), left = 0, one = 0, result = INT_MAX, first = 0, mid = k >> 1, pos[n + 1];
        memset(pos, 0, sizeof pos);
        for (int i = 0; i < n; i++) if (nums[i]) pos[one++] = i;
        for (int t = 1; t < k; t++) first += (pos[t] - pos[t - 1] - 1) * min(t, k - t);
        while (left + k <= one) 
        {
            result = min(result, first);
            first += pos[left] - pos[left + mid] + pos[left + k] - pos[left++ + k - mid];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minMoves(int[] nums, int k) {
        int n = nums.length, left = 0, one = 0, result = Integer.MAX_VALUE, first = 0, mid = k >> 1, pos[] = new int[n + 1];
        for (int i = 0; i < n; i++) if (nums[i] == 1) pos[one++] = i;
        for (int t = 1; t < k; t++) first += (pos[t] - pos[t - 1] - 1) * Math.min(t, k - t);
        while (left + k <= one) {
            result = Math.min(result, first);
            first += pos[left] - pos[left + mid] + pos[left + k] - pos[left++ + k - mid];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minMoves(self, nums: List[int], k: int) -> int:
        n, left, one, result, mid, pos = len(nums), 0, sum(nums), float('inf'), k >> 1, [i for i in range(len(nums)) if nums[i]] + [0] * (len(nums) - sum(nums) + 1)
        first = sum((pos[t] - pos[t - 1] - 1) * min(k - t, t) for t in range(1, k))
        while left + k <= one:
            result = min(result, first)
            first += pos[left] - pos[left + mid] + pos[left + k] - pos[left + k - mid]
            left += 1
        return result
```
