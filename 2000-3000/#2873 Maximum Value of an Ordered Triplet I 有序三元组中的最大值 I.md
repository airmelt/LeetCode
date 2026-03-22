# 2873 Maximum Value of an Ordered Triplet I 有序三元组中的最大值 I

__Description:__

You are given a __0-indexed__ integer array `nums`.

Return ___the maximum value over all triplets of indices___ `(i, j, k)` _such that_ `i < j < k`. If all such triplets have a negative value, return `0`.

The __value of a triplet of indices__ `(i, j, k)` is equal to `(nums[i] - nums[j]) * nums[k]`.

__Example:__

Example 1:

```text
Input: nums = [12,6,1,2,7]
Output: 77
Explanation: The value of the triplet (0, 2, 4) is (nums[0] - nums[2]) * nums[4] = 77.
It can be shown that there are no ordered triplets of indices with a value greater than 77.
```

Example 2:

```text
Input: nums = [1,10,3,4,19]
Output: 133
Explanation: The value of the triplet (1, 2, 4) is (nums[1] - nums[2]) * nums[4] = 133.
It can be shown that there are no ordered triplets of indices with a value greater than 133.
```

Example 3:

```text
Input: nums = [1,2,3]
Output: 0
Explanation: The only ordered triplet of indices (0, 1, 2) has a negative value of (nums[0] - nums[1]) * nums[2] = -3. Hence, the answer would be 0.
```

__Constraints:__

- `3 <= nums.length <= 100`
- `1 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。

请你从所有满足 `i < j < k` 的下标三元组 `(i, j, k)` 中，找出并返回下标三元组的最大值。如果所有满足条件的三元组的值都是负数，则返回 `0` 。

__下标三元组__ `(i, j, k)` 的值等于 `(nums[i] - nums[j]) * nums[k]` 。

__示例:__

示例 1：

```text
输入：nums = [12,6,1,2,7]
输出：77
解释：下标三元组 (0, 2, 4) 的值是 (nums[0] - nums[2]) * nums[4] = 77 。
可以证明不存在值大于 77 的有序下标三元组。
```

示例 2：

```text
输入：nums = [1,10,3,4,19]
输出：133
解释：下标三元组 (1, 2, 4) 的值是 (nums[1] - nums[2]) * nums[4] = 133 。
可以证明不存在值大于 133 的有序下标三元组。
```

示例 3：

```text
输入：nums = [1,2,3]
输出：0
解释：唯一的下标三元组 (0, 1, 2) 的值是一个负数，(nums[0] - nums[1]) * nums[2] = -3 。因此，答案是 0 。
```

__提示：__

- `3 <= nums.length <= 100`
- `1 <= nums[i] <= 10 ^ 6`

__思路:__

```text
1. 暴力法
枚举 i, j, k
计算最大的 (nums[i] - nums[j]) * nums[k]
时间复杂度为 O(N ^ 3), 空间复杂度为 O(1)
2. 枚举中间值
设 i 左边最大的值为 left[i]
i 右边最大的值为 right[i]
则 left[i] = max(nums[:i]), 迭代可以用 max(left[i - 1], nums[i - 1])
right[i] 同理
则对 i 来说值取 (left[i] - nums[i]) * right[i] 时能取到最大值
遍历数组找到上式的最大值即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maximumTripletValue(vector<int>& nums) 
    {
        long long result = 0LL;
        int n = nums.size();
        vector<int> left(n), right(n);
        for (int i = 1; i < n; i++) 
        {
            left[i] = max(left[i - 1], nums[i - 1]);
            right[n - i - 1] = max(right[n - i], nums[n - i]);
        }
        for (int i = 1; i < n - 1; i++) result = max((long long)right[i] * (left[i] - nums[i]), result);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maximumTripletValue(int[] nums) {
        long result = 0L;
        int n = nums.length, left[] = new int[n], right[] = new int[n];
        for (int i = 1; i < n; i++) {
            left[i] = Math.max(left[i - 1], nums[i - 1]);
            right[n - i - 1] = Math.max(right[n - i], nums[n - i]);
        }
        for (int i = 1; i < n - 1; i++) result = Math.max((long)right[i] * (left[i] - nums[i]), result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumTripletValue(self, nums: List[int]) -> int:
        return max(max(((x - y) * z for i, x in enumerate(nums) for j, y in enumerate(nums[i + 1:], i + 1) for k, z in enumerate(nums[j + 1:], j)), default=0), 0)
```
