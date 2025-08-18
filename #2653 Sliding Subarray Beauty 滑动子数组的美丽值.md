# 2653 Sliding Subarray Beauty 滑动子数组的美丽值

__Description:__

Given an integer array `nums` containing `n` integers, find the __beauty__ of each subarray of size `k`.

The __beauty__ of a subarray is the `x ^ th` __smallest integer__ in the subarray if it is __negative__, or `0` if there are fewer than `x` negative integers.

Return _an integer array containing_ `n - k + 1` _integers, which denote the_ __beauty__ _of the subarrays __in order__ from the first index in the array._

- A subarray is a contiguous __non-empty__ sequence of elements within an array.

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input:  nums = [1,-1,-3,-2,3], k = 3, x = 2
Output:  [-1,-2,-2]
Explanation:  There are 3 subarrays with size k = 3. 
The first subarray is [1, -1, -3] and the 2 ^ nd smallest negative integer is -1. 
The second subarray is [-1, -3, -2] and the 2 ^ nd smallest negative integer is -2. 
The third subarray is [-3, -2, 3] and the 2 ^ nd smallest negative integer is -2.
```

Example 2:

```text
Input:  nums = [-1,-2,-3,-4,-5], k = 2, x = 2
Output:  [-1,-2,-3,-4]
Explanation:  There are 4 subarrays with size k = 2.
For [-1, -2], the 2 ^ nd smallest negative integer is -1.
For [-2, -3], the 2 ^ nd smallest negative integer is -2.
For [-3, -4], the 2 ^ nd smallest negative integer is -3.
For [-4, -5], the 2 ^ nd smallest negative integer is -4.
```

Example 3:

```text
Input:  nums = [-3,1,2,-3,0,-3], k = 2, x = 1
Output:  [-3,0,-3,-3,-3]
Explanation:  There are 5 subarrays with size k = 2.
For [-3, 1], the 1 ^ st smallest negative integer is -3.
For [1, 2], there is no negative integer so the beauty is 0.
For [2, -3], the 1 ^ st smallest negative integer is -3.
For [-3, 0], the 1 ^ st smallest negative integer is -3.
For [0, -3], the 1 ^ st smallest negative integer is -3.
```

__Constraints:__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `1 <= k <= n`
- `1 <= x <= k`
- `-50 <= nums[i] <= 50`

__题目描述:__

给你一个长度为 `n` 的整数数组 `nums` ，请你求出每个长度为 `k` 的子数组的 _美丽值_ 。

一个子数组的 __美丽值__ 定义为:如果子数组中第 `x` __小整数__ 是 __负数__ ，那么美丽值为第 `x` 小的数，否则美丽值为 `0` 。

请你返回一个包含 `n - k + 1` 个整数的数组，__依次__ 表示数组中从第一个下标开始，每个长度为 `k` 的子数组的 __美丽值__ 。

- 子数组指的是数组中一段连续 __非空__ 的元素序列。

子数组指的是数组中一段连续 非空 的元素序列。

__示例:__

示例 1：

```text
输入: nums = [1,-1,-3,-2,3], k = 3, x = 2
输出: [-1,-2,-2]
解释: 总共有 3 个 k = 3 的子数组。
第一个子数组是 [1, -1, -3] ，第二小的数是负数 -1 。
第二个子数组是 [-1, -3, -2] ，第二小的数是负数 -2 。
第三个子数组是 [-3, -2, 3] ，第二小的数是负数 -2 。
```

示例 2：

```text
输入: nums = [-1,-2,-3,-4,-5], k = 2, x = 2
输出: [-1,-2,-3,-4]
解释: 总共有 4 个 k = 2 的子数组。
[-1, -2] 中第二小的数是负数 -1 。
[-2, -3] 中第二小的数是负数 -2 。
[-3, -4] 中第二小的数是负数 -3 。
[-4, -5] 中第二小的数是负数 -4 。
```

示例 3：

```text
输入: nums = [-3,1,2,-3,0,-3], k = 2, x = 1
输出: [-3,0,-3,-3,-3]
解释: 总共有 5 个 k = 2 的子数组。
[-3, 1] 中最小的数是负数 -3 。
[1, 2] 中最小的数不是负数，所以美丽值为 0 。
[2, -3] 中最小的数是负数 -3 。
[-3, 0] 中最小的数是负数 -3 。
[0, -3] 中最小的数是负数 -3 。
```

__提示：__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `1 <= k <= n`
- `1 <= x <= k`
- `-50 <= nums[i] <= 50`

__思路:__

```text
计数排序
注意到 nums[i] 的范围为 [-50, 50]
则 nums[i] + 50 的范围为 [0, 100]
用一个数组记录每个数出现的次数
遍历这个数组就能得到第 x 小的数
时间复杂度为 O(MN), 空间复杂度为 O(M), M 为数组的值域
```

__代码:__

__C++__:

```C++
class Solution {
public:
    vector<int> getSubarrayBeauty(vector<int>& nums, int k, int x) {
        int n = nums.size();
        vector<int> result(n - k + 1), c(101);
        for (int i = 0; i < k - 1; i++) ++c[nums[i] + 50];
        for (int i = k - 1; i < n; i++) 
        {
            ++c[nums[i] + 50];
            for (int j = 0, left = x; j < 50; j++) 
            {
                if ((left -= c[j]) <= 0) 
                {
                    result[i - k + 1] = j - 50;
                    break;
                }
            }
            --c[nums[i - k + 1] + 50];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
public:
    vector<int> getSubarrayBeauty(vector<int>& nums, int k, int x) {
        int n = nums.size();
        vector<int> result(n - k + 1), c(101);
        for (int i = 0; i < k - 1; i++) ++c[nums[i] + 50];
        for (int i = k - 1; i < n; i++) 
        {
            ++c[nums[i] + 50];
            for (int j = 0, left = x; j < 50; j++) 
            {
                if ((left -= c[j]) <= 0) 
                {
                    result[i - k + 1] = j - 50;
                    break;
                }
            }
            --c[nums[i - k + 1] + 50];
        }
        return result;
    }
};
```

__Python__:

```Python
class Solution {
public:
    vector<int> getSubarrayBeauty(vector<int>& nums, int k, int x) {
        int n = nums.size();
        vector<int> result(n - k + 1), c(101);
        for (int i = 0; i < k - 1; i++) ++c[nums[i] + 50];
        for (int i = k - 1; i < n; i++) 
        {
            ++c[nums[i] + 50];
            for (int j = 0, left = x; j < 50; j++) 
            {
                if ((left -= c[j]) <= 0) 
                {
                    result[i - k + 1] = j - 50;
                    break;
                }
            }
            --c[nums[i - k + 1] + 50];
        }
        return result;
    }
};
```
