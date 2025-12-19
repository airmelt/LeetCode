# 2811 Check if it is Possible to Split Array 判断是否能拆分数组

__Description:__

You are given an array `nums` of length `n` and an integer `m`. You need to determine if it is possible to split the array into `n` arrays of size 1 by performing a series of steps.

An array is called good if:

- The length of the array is __one__, or
- The sum of the elements of the array is __greater than or equal__ to `m`.

In each step, you can select an existing array (which may be the result of previous steps) with a length of at least two and split it into two arrays, if both resulting arrays are good.

Return true if you can split the given array into `n` arrays, otherwise return false.

__Example:__

Example 1:

```text
Input: nums = [2, 2, 1], m = 4

Output: true

Explanation:
```

- Split `[2, 2, 1]` to `[2, 2]` and `[1]`. The array `[1]` has a length of one, and the array `[2, 2]` has the sum of its elements equal to `4 >= m`, so both are good arrays.
- Split `[2, 2]` to `[2]` and `[2]`. both arrays have the length of one, so both are good arrays.

Example 2:

```text
Input: nums = [2, 1, 3], m = 5

Output: false

Explanation:

The first move has to be either of the following:
```

- Split `[2, 1, 3]` to `[2, 1]` and `[3]`. The array `[2, 1]` has neither length of one nor sum of elements greater than or equal to `m`.
- Split `[2, 1, 3]` to `[2]` and `[1, 3]`. The array `[1, 3]` has neither length of one nor sum of elements greater than or equal to `m`.

So as both moves are invalid (they do not divide the array into two good arrays), we are unable to split `nums` into `n` arrays of size 1.

Example 3:

```text
Input: nums = [2, 3, 3, 2, 3], m = 6

Output: true

Explanation:
```

- Split `[2, 3, 3, 2, 3]` to `[2]` and `[3, 3, 2, 3]`.
- Split `[3, 3, 2, 3]` to `[3, 3, 2]` and `[3]`.
- Split `[3, 3, 2]` to `[3, 3]` and `[2]`.
- Split `[3, 3]` to `[3]` and `[3]`.

__Constraints:__

- `1 <= n == nums.length <= 100`
- `1 <= nums[i] <= 100`
- `1 <= m <= 200`

__题目描述:__

给你一个长度为 `n` 的数组 `nums` 和一个整数 `m` 。请你判断能否执行一系列操作，将数组拆分成 `n` 个 __非空__ 数组。

一个数组被称为 好 的，如果：

- 子数组的长度为 1 ，或者
- 子数组元素之和 __大于或等于__  `m` 。

在每一步操作中，你可以选择一个 长度至少为 2 的现有数组（之前步骤的结果） 并将其拆分成 2 个子数组，而得到的 每个 子数组都需要是好的。

如果你可以将给定数组拆分成 `n` 个满足要求的数组，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：nums = [2, 2, 1], m = 4

输出：true

解释：
```

- 将 `[2, 2, 1]` 切分为 `[2, 2]` 和 `[1]`。数组 `[1]` 的长度为 1，数组 `[2, 2]` 的元素之和等于 `4 >= m`，所以两者都是好的数组。
- 将 `[2, 2]` 切分为 `[2]` 和 `[2]`。两个数组的长度都是 1，所以都是好的数组。

示例 2：

```text
输入：nums = [2, 1, 3], m = 5

输出：false

解释：

第一步必须是以下之一：
```

- 将 `[2, 1, 3]` 切分为 `[2, 1]` 和 `[3]`。数组 `[2, 1]` 既不是长度为 1，也没有大于或等于 `m` 的元素和。
- 将 `[2, 1, 3]` 切分为 `[2]` 和 `[1, 3]`。数组 `[1, 3]` 既不是长度为 1，也没有大于或等于 `m` 的元素和。

因此，由于这两个操作都无效（它们没有将数组分成两个好的数组），因此我们无法将 `nums` 分成 `n` 个大小为 1 的数组。

示例 3：

```text
输入：nums = [2, 3, 3, 2, 3], m = 6

输出：true

解释：
```

- 将 `[2, 3, 3, 2, 3]` 切分为 `[2]` 和 `[3, 3, 2, 3]`。
- 将 `[3, 3, 2, 3]` 切分为 `[3, 3, 2]` 和 `[3]`。
- 将 `[3, 3, 2]` 切分为 `[3, 3]` 和 `[2]`。
- 将 `[3, 3]` 切分为 `[3]` 和 `[3]`。

__提示：__

- `1 <= n == nums.length <= 100`
- `1 <= nums[i] <= 100`
- `1 <= m <= 200`

__思路:__

```text
脑筋急转弯
如果 nums 长度是 1 或 2 必定成功
否则只要找到任何一个相邻元素使得两个元素之和不比 m 小
以这两个数为基础不断进行操作即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canSplitArray(vector<int>& nums, int m) 
    {
        return nums.size() < 3 or std::ranges::any_of(nums | std::views::pairwise, [m](const auto& p) { return (get<0>(p) + get<1>(p)) >= m; });
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canSplitArray(List<Integer> nums, int m) {
        return nums.size() < 3 || IntStream.range(1, nums.size()).mapToObj(i -> nums.get(i) + nums.get(i - 1)).anyMatch(v -> v >= m);
    }
}
```

__Python__:

```Python
class Solution:
    def canSplitArray(self, nums: List[int], m: int) -> bool:
        return len(nums) < 3 or any(x + y >= m for x, y in pairwise(nums))
```
