# 3011 Find if Array Can Be Sorted 判断一个数组是否可以变为有序

__Description:__

You are given a __0-indexed__ array of __positive__ integers `nums`.

In one operation, you can swap any two adjacent elements if they have the same number of set bits. You are allowed to do this operation any number of times (including zero).

Return `true` _if you can sort the array in ascending order, else return_ `false`.

__Example:__

Example 1:

```text
Input: nums = [8,4,2,30,15]
Output: true
Explanation: Let's look at the binary representation of every element. The numbers 2, 4, and 8 have one set bit each with binary representation "10", "100", and "1000" respectively. The numbers 15 and 30 have four set bits each with binary representation "1111" and "11110".
We can sort the array using 4 operations:
```

- Swap nums[0] with nums[1]. This operation is valid because 8 and 4 have one set bit each. The array becomes [4,8,2,30,15].
- Swap nums[1] with nums[2]. This operation is valid because 8 and 2 have one set bit each. The array becomes [4,2,8,30,15].
- Swap nums[0] with nums[1]. This operation is valid because 4 and 2 have one set bit each. The array becomes [2,4,8,30,15].
- Swap nums[3] with nums[4]. This operation is valid because 30 and 15 have four set bits each. The array becomes [2,4,8,15,30].

The array has become sorted, hence we return true.

Note that there may be other sequences of operations which also sort the array.

Example 2:

```text
Input: nums = [1,2,3,4,5]
Output: true
Explanation: The array is already sorted, hence we return true.
```

Example 3:

```text
Input: nums = [3,16,8,4,2]
Output: false
Explanation: It can be shown that it is not possible to sort the input array using any number of operations.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 2 ^ 8`

__题目描述:__

给你一个下标从 __0__ 开始且全是 __正__ 整数的数组 `nums` 。

一次 操作 中，如果两个 相邻 元素在二进制下 设置位 的数目 相同 ，那么你可以将这两个元素交换。你可以执行这个操作 任意次 （也可以 0 次）。

如果你可以使数组变为非降序，请你返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

```text
输入：nums = [8,4,2,30,15]
输出：true
解释：我们先观察每个元素的二进制表示。 2 ，4 和 8 分别都只有一个数位为 1 ，分别为 "10" ，"100" 和 "1000" 。15 和 30 分别有 4 个数位为 1 ："1111" 和 "11110" 。
我们可以通过 4 个操作使数组非降序：
```

- 交换 nums[0] 和 nums[1] 。8 和 4 分别只有 1 个数位为 1 。数组变为 [4,8,2,30,15] 。
- 交换 nums[1] 和 nums[2] 。8 和 2 分别只有 1 个数位为 1 。数组变为 [4,2,8,30,15] 。
- 交换 nums[0] 和 nums[1] 。4 和 2 分别只有 1 个数位为 1 。数组变为 [2,4,8,30,15] 。
- 交换 nums[3] 和 nums[4] 。30 和 15 分别有 4 个数位为 1 ，数组变为 [2,4,8,15,30] 。

数组变成有序的，所以我们返回 true 。

注意我们还可以通过其他的操作序列使数组变得有序。

示例 2：

```text
输入：nums = [1,2,3,4,5]
输出：true
解释：数组已经是非降序的，所以我们返回 true 。
```

示例 3：

```text
输入：nums = [3,16,8,4,2]
输出：false
解释：无法通过操作使数组变为非降序。
```

__提示：__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 2 ^ 8`

__思路:__

```text
模拟
由于只能相邻元素进行交换
记录每一段的最大值 mx
如果当前元素属于同一组, 即对应二进制中的 1 的个数相等
就更新当前组的最大值
如果下一组任意元素小于之前的最大值就不能排序
否则更新最大值为当前组的最大值继续遍历
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canSortArray(vector<int>& nums) 
    {
        for (int pre = 0, n = nums.size(), i = 0, mx = 0; i < n; mx = 0) {
            for (int cur = __builtin_popcount(nums[i]); i < n and __builtin_popcount(nums[i]) == cur; i++) 
            {
                if (nums[i] < pre) return false;
                mx = max(mx, nums[i]);
            }
            pre = mx;
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canSortArray(int[] nums) {
        for (int i = 0, mx = 0, pre = 0, n = nums.length; i < n; mx = 0) {
            for (int cur = Integer.bitCount(nums[i]); i < n && Integer.bitCount(nums[i]) == cur; i++) {
                if (nums[i] < pre) return false;
                mx = Math.max(mx, nums[i]);
            }
            pre = mx;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def canSortArray(self, nums: List[int]) -> bool:
        n, i, pre = len(nums), 0, 0
        while i < n:
            mx, cur = 0, nums[i].bit_count()
            while i < n and nums[i].bit_count() == cur:
                if (x := nums[i]) < pre:
                    return False
                mx = max(mx, x)
                i += 1
            pre = mx
        return True
```
