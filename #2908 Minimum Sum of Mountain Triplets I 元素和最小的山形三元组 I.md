# 2908 Minimum Sum of Mountain Triplets I 元素和最小的山形三元组 I

__Description:__

You are given a __0-indexed__ array `nums` of integers.

A triplet of indices `(i, j, k)` is a __mountain__ if:

- `i < j < k`
- `nums[i] < nums[j]` and `nums[k] < nums[j]`

Return _the __minimum possible sum__ of a mountain triplet of_ `nums`. _If no such triplet exists, return_ `-1`.

__Example:__

Example 1:

```text
Input: nums = [8,6,1,5,3]
Output: 9
Explanation: Triplet (2, 3, 4) is a mountain triplet of sum 9 since: 
```

- 2 < 3 < 4
- nums[2] < nums[3] and nums[4] < nums[3]

And the sum of this triplet is nums[2] + nums[3] + nums[4] = 9. It can be shown that there are no mountain triplets with a sum of less than 9.

Example 2:

```text
Input: nums = [5,4,8,7,10,2]
Output: 13
Explanation: Triplet (1, 3, 5) is a mountain triplet of sum 13 since: 
```

- 1 < 3 < 5
- nums[1] < nums[3] and nums[5] < nums[3]

And the sum of this triplet is nums[1] + nums[3] + nums[5] = 13. It can be shown that there are no mountain triplets with a sum of less than 13.

Example 3:

```text
Input: nums = [6,5,4,3,4,5]
Output: -1
Explanation: It can be shown that there are no mountain triplets in nums.
```

__Constraints:__

- `3 <= nums.length <= 50`
- `1 <= nums[i] <= 50`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。

如果下标三元组 `(i, j, k)` 满足下述全部条件，则认为它是一个 __山形三元组__ :

- `i < j < k`
- `nums[i] < nums[j]` 且 `nums[k] < nums[j]`

请你找出 `nums` 中 __元素和最小__ 的山形三元组，并返回其 __元素和__ 。如果不存在满足条件的三元组，返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [8,6,1,5,3]
输出：9
解释：三元组 (2, 3, 4) 是一个元素和等于 9 的山形三元组，因为： 
```

- 2 < 3 < 4
- nums[2] < nums[3] 且 nums[4] < nums[3]

这个三元组的元素和等于 nums[2] + nums[3] + nums[4] = 9 。可以证明不存在元素和小于 9 的山形三元组。

示例 2：

```text
输入：nums = [5,4,8,7,10,2]
输出：13
解释：三元组 (1, 3, 5) 是一个元素和等于 13 的山形三元组，因为： 
```

- 1 < 3 < 5
- nums[1] < nums[3] 且 nums[5] < nums[3]

这个三元组的元素和等于 nums[1] + nums[3] + nums[5] = 13 。可以证明不存在元素和小于 13 的山形三元组。

示例 3：

```text
输入：nums = [6,5,4,3,4,5]
输出：-1
解释：可以证明 nums 中不存在山形三元组。
```

__提示：__

- `3 <= nums.length <= 50`
- `1 <= nums[i] <= 50`

__思路:__

```text
1. 暴力法
检查每一个三元组
如果是山形数组
那么更新最小和
时间复杂度为 O(N ^ 3), 空间复杂度为 O(1)
2. 前后缀分解
设 suf[i] 表示 i 及之后的最小值
那么 suf[n - 1] = nums[n - 1]
更新 suf[i] = min(suf[i + 1], nums[i])
记录 pre 为 nums[:i] 的最小值
则如果 pre < nums[j] and pre > suf[j + 1]
则满足山形数组
更新最小和
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumSum(vector<int>& nums) 
    {
        int n = nums.size(), result = INT_MAX, pre = nums.front();
        vector<int> suf(n, nums.back());
        for (int i = n - 2; i > 1; i--) suf[i] = min(suf[i + 1], nums[i]);
        for (int j = 1; j < n - 1; pre = min(pre, nums[j++])) if (pre < nums[j] and nums[j] > suf[j + 1]) result = min(result, pre + nums[j] + suf[j + 1]); 
        return result < INT_MAX ? result : -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumSum(int[] nums) {
        int n = nums.length, suf[] = new int[n], result = Integer.MAX_VALUE, pre = nums[0];
        suf[n - 1] = nums[n - 1];
        for (int i = n - 2; i > 1; i--) suf[i] = Math.min(suf[i + 1], nums[i]);
        for (int j = 1; j < n - 1; pre = Math.min(pre, nums[j++])) if (pre < nums[j] && nums[j] > suf[j + 1]) result = Math.min(result, pre + nums[j] + suf[j + 1]); 
        return result < Integer.MAX_VALUE ? result : -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumSum(self, nums: List[int]) -> int:
        return min((nums[i] + nums[j] + nums[k] for i in range(len(nums)) for j in range(i + 1, len(nums)) for k in range(j + 1, len(nums)) if nums[i] < nums[j] > nums[k]), default=-1)
```
