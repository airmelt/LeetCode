# 2366 Minimum Replacements to Sort the Array 将数组排序的最少替换次数

__Description:__

You are given a __0-indexed__ integer array `nums`. In one operation you can replace any element of the array with __any two__ elements that __sum__ to it.

- For example, consider `nums = [5,6,7]`. In one operation, we can replace `nums[1]` with `2` and `4` and convert `nums` to `[5,2,4,7]`.

Return the minimum number of operations to make an array that is sorted in non-decreasing order.

__Example:__

Example 1:

```text
Input: nums = [3,9,3]
Output: 2
Explanation: Here are the steps to sort the array in non-decreasing order:
```

- From [3,9,3], replace the 9 with 3 and 6 so the array becomes [3,3,6,3]
- From [3,3,6,3], replace the 6 with 3 and 3 so the array becomes [3,3,3,3,3]

There are 2 steps to sort the array in non-decreasing order. Therefore, we return 2.

Example 2:

```text
Input: nums = [1,2,3,4,5]
Output: 0
Explanation: The array is already in non-decreasing order. Therefore, we return 0.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。每次操作中，你可以将数组中任何一个元素替换为 __任意两个__ 和为该元素的数字。

- 比方说， `nums = [5,6,7]` 。一次操作中，我们可以将 `nums[1]` 替换成 `2` 和 `4` ，将 `nums` 转变成 `[5,2,4,7]` 。

请你执行上述操作，将数组变成元素按 非递减 顺序排列的数组，并返回所需的最少操作次数。

__示例:__

示例 1：

```text
输入：nums = [3,9,3]
输出：2
解释：以下是将数组变成非递减顺序的步骤：
```

- [3,9,3] ，将9 变成 3 和 6 ，得到数组 [3,3,6,3]
- [3,3,6,3] ，将 6 变成 3 和 3 ，得到数组 [3,3,3,3,3]

总共需要 2 步将数组变成非递减有序，所以我们返回 2 。

示例 2：

```text
输入：nums = [1,2,3,4,5]
输出：0
解释：数组已经是非递减顺序，所以我们返回 0 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
贪心
从后往前遍历数
每次将前一个元素替换为不大于当前元素的最大值
如果设前一个数最大为 m, 这时拆出了 x 个 m
即 nums[i] = mx
所以替换次数 k = x - 1 = nums[i] / m - 1, 向上取整
所以 k = (nums[i] - 1) / m
拆完之后最新的最小值应为 nums[i] / (k + 1), 即下一次拆分的最大值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minimumReplacement(vector<int>& nums) 
    {
        long long result = 0, m = nums.back();
        for (auto it = nums.rbegin(); it != nums.rend(); it++) 
        {
            result += (*it - 1LL) / m;
            m = *it / ((*it - 1LL) / m + 1LL);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long minimumReplacement(int[] nums) {
        long result = 0, m = nums[nums.length - 1];
        for (int n = nums.length, i = n - 1; i > -1; i--) {
            result += (nums[i] - 1L) / m;
            m = nums[i] / ((nums[i] - 1L) / m + 1L);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumReplacement(self, nums: List[int]) -> int:
        return reduce(lambda r, x: (r[0] + (t := (x - 1) // r[1]), x // (t + 1)), nums[::-1], (0, nums[-1]))[0]
```
