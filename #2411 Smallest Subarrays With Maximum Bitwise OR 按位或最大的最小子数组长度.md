# 2411 Smallest Subarrays With Maximum Bitwise OR 按位或最大的最小子数组长度

__Description:__

You are given a __0-indexed__ array `nums` of length `n`, consisting of non-negative integers. For each index `i` from `0` to `n - 1`, you must determine the size of the __minimum sized__ non-empty subarray of `nums` starting at `i` (__inclusive__) that has the __maximum__ possible __bitwise OR__.

- In other words, let `Bij` be the bitwise OR of the subarray `nums[i...j]`. You need to find the smallest subarray starting at `i`, such that bitwise OR of this subarray is equal to `max(Bik)` where `i <= k <= n - 1`.

The bitwise OR of an array is the bitwise OR of all the numbers in it.

Return _an integer array_ `answer` _of size_ `n` _where_ `answer[i]` _is the length of the __minimum__ sized subarray starting at_ `i` _with __maximum__ bitwise OR._

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [1,0,2,1,3]
Output: [3,3,2,2,1]
Explanation:
The maximum possible bitwise OR starting at any index is 3. 
```

- Starting at index 0, the shortest subarray that yields it is [1,0,2].
- Starting at index 1, the shortest subarray that yields the maximum bitwise OR is [0,2,1].
- Starting at index 2, the shortest subarray that yields the maximum bitwise OR is [2,1].
- Starting at index 3, the shortest subarray that yields the maximum bitwise OR is [1,3].
- Starting at index 4, the shortest subarray that yields the maximum bitwise OR is [3].

Therefore, we return [3,3,2,2,1].

Example 2:

```text
Input: nums = [1,2]
Output: [2,1]
Explanation:
Starting at index 0, the shortest subarray that yields the maximum bitwise OR is of length 2.
Starting at index 1, the shortest subarray that yields the maximum bitwise OR is of length 1.
Therefore, we return [2,1].
```

__Constraints:__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个长度为 `n` 下标从 __0__ 开始的数组 `nums` ，数组中所有数字均为非负整数。对于 `0` 到 `n - 1` 之间的每一个下标 `i` ，你需要找出 `nums` 中一个 __最小__ 非空子数组，它的起始位置为 `i` （包含这个位置），同时有 __最大__ 的 __按位或__ _运算值_ 。

- 换言之，令 `Bij` 表示子数组 `nums[i...j]` 的按位或运算的结果，你需要找到一个起始位置为 `i` 的最小子数组，这个子数组的按位或运算的结果等于 `max(Bik)` ，其中 `i <= k <= n - 1` 。

一个数组的按位或运算值是这个数组里所有数字按位或运算的结果。

请你返回一个大小为 `n` 的整数数组 `answer`，其中 `answer[i]`是开始位置为 `i` ，按位或运算结果最大，且 __最短__ 子数组的长度。

子数组 是数组里一段连续非空元素组成的序列。

__示例:__

示例 1：

```text
输入：nums = [1,0,2,1,3]
输出：[3,3,2,2,1]
解释：
任何位置开始，最大按位或运算的结果都是 3 。
```

- 下标 0 处，能得到结果 3 的最短子数组是 [1,0,2] 。
- 下标 1 处，能得到结果 3 的最短子数组是 [0,2,1] 。
- 下标 2 处，能得到结果 3 的最短子数组是 [2,1] 。
- 下标 3 处，能得到结果 3 的最短子数组是 [1,3] 。
- 下标 4 处，能得到结果 3 的最短子数组是 [3] 。

所以我们返回 [3,3,2,2,1] 。

示例 2：

```text
输入：nums = [1,2]
输出：[2,1]
解释：
下标 0 处，能得到最大按位或运算值的最短子数组长度为 2 。
下标 1 处，能得到最大按位或运算值的最短子数组长度为 1 。
所以我们返回 [2,1] 。
```

__提示：__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`

__思路:__

```text
模拟
从 i 开始倒推
如果 nums[j] | nums[i] == nums[j] 则停止, 说明 nums[j] 已经无法再增大
令 nums[j] |= nums[i]
更新 result[j] = i - j + 1
时间复杂度为 O(NlogM), 空间复杂度为 O(1), 其中 M 为 nums 中的最大值, 因为 nums[j] 最多只会被更新 logM 次
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> smallestSubarrays(vector<int>& nums) 
    {
        vector<int> result(nums.size(), 1);
        for (int i = 0, n = nums.size(); i < n; i++) 
        {
            for (int j = i - 1; j > -1; j--) 
            {
                if ((nums[j] | nums[i]) == nums[j]) break;
                nums[j] |= nums[i];
                result[j] = i - j + 1;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] smallestSubarrays(int[] nums) {
        int n = nums.length, result[] = new int[n];
        Arrays.fill(result, 1);
        for (int i = 0; i < n; i++) {
            for (int j = i - 1; j > -1; j--) {
                if ((nums[j] | nums[i]) == nums[j]) break;
                nums[j] |= nums[i];
                result[j] = i - j + 1;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def smallestSubarrays(self, nums: List[int]) -> List[int]:
        result = [1] * len(nums)
        for i, num in enumerate(nums):
            for j in range(i - 1, -1, -1):
                if (nums[j] | num) == nums[j]:
                    break
                nums[j] |= num
                result[j] = i - j + 1
        return result
```
