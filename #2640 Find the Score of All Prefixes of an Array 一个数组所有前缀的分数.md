# 2640 Find the Score of All Prefixes of an Array 一个数组所有前缀的分数

__Description:__

We define the __conversion array__ `conver` of an array `arr` as follows:

- `conver[i] = arr[i] + max(arr[0..i])` where `max(arr[0..i])` is the maximum value of `arr[j]` over `0 <= j <= i`.

We also define the __score__ of an array `arr` as the sum of the values of the conversion array of `arr`.

Given a __0-indexed__ integer array `nums` of length `n`, return _an array_ `ans` _of length_ `n` _where_ `ans[i]` _is the score of the prefix_ `nums[0..i]`.

__Example:__

Example 1:

```text
Input: nums = [2,3,7,5,10]
Output: [4,10,24,36,56]
Explanation: 
For the prefix [2], the conversion array is [4] hence the score is 4
For the prefix [2, 3], the conversion array is [4, 6] hence the score is 10
For the prefix [2, 3, 7], the conversion array is [4, 6, 14] hence the score is 24
For the prefix [2, 3, 7, 5], the conversion array is [4, 6, 14, 12] hence the score is 36
For the prefix [2, 3, 7, 5, 10], the conversion array is [4, 6, 14, 12, 20] hence the score is 56
```

Example 2:

```text
Input: nums = [1,1,2,4,8,16]
Output: [2,4,8,16,32,64]
Explanation: 
For the prefix [1], the conversion array is [2] hence the score is 2
For the prefix [1, 1], the conversion array is [2, 2] hence the score is 4
For the prefix [1, 1, 2], the conversion array is [2, 2, 4] hence the score is 8
For the prefix [1, 1, 2, 4], the conversion array is [2, 2, 4, 8] hence the score is 16
For the prefix [1, 1, 2, 4, 8], the conversion array is [2, 2, 4, 8, 16] hence the score is 32
For the prefix [1, 1, 2, 4, 8, 16], the conversion array is [2, 2, 4, 8, 16, 32] hence the score is 64
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

定义一个数组 `arr` 的 __转换数组__ `conver` 为:

- `conver[i] = arr[i] + max(arr[0..i])`，其中 `max(arr[0..i])` 是满足 `0 <= j <= i` 的所有 `arr[j]` 中的最大值。

定义一个数组 `arr` 的 __分数__ 为 `arr` 转换数组中所有元素的和。

给你一个下标从 __0__ 开始长度为 `n` 的整数数组 `nums` ，请你返回一个长度为 `n` 的数组 `ans` ，其中 `ans[i]`是前缀 `nums[0..i]` 的分数。

__示例:__

示例 1：

```text
输入：nums = [2,3,7,5,10]
输出：[4,10,24,36,56]
解释：
对于前缀 [2] ，转换数组为 [4] ，所以分数为 4 。
对于前缀 [2, 3] ，转换数组为 [4, 6] ，所以分数为 10 。
对于前缀 [2, 3, 7] ，转换数组为 [4, 6, 14] ，所以分数为 24 。
对于前缀 [2, 3, 7, 5] ，转换数组为 [4, 6, 14, 12] ，所以分数为 36 。
对于前缀 [2, 3, 7, 5, 10] ，转换数组为 [4, 6, 14, 12, 20] ，所以分数为 56 。
```

示例 2：

```text
输入：nums = [1,1,2,4,8,16]
输出：[2,4,8,16,32,64]
解释：
对于前缀 [1] ，转换数组为 [2] ，所以分数为 2 。
对于前缀 [1, 1]，转换数组为 [2, 2] ，所以分数为 4 。
对于前缀 [1, 1, 2]，转换数组为 [2, 2, 4] ，所以分数为 8 。
对于前缀 [1, 1, 2, 4]，转换数组为 [2, 2, 4, 8] ，所以分数为 16 。
对于前缀 [1, 1, 2, 4, 8]，转换数组为 [2, 2, 4, 8, 16] ，所以分数为 32 。
对于前缀 [1, 1, 2, 4, 8, 16]，转换数组为 [2, 2, 4, 8, 16, 32] ，所以分数为 64 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
模拟
一边遍历, 一边计算当前的最大值
结果用类似前缀和的方式累加
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<long long> findPrefixScore(vector<int>& nums) 
    {
        int mx = -1, n = nums.size();
        vector<long long> result(n);
        for (int i = 0; i < n; i++) result[i] = nums[i] + (mx = max(mx, nums[i])) + (i ? result[i - 1] : 0LL);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long[] findPrefixScore(int[] nums) {
        int mx = -1, n = nums.length;
        long[] result = new long[n];
        for (int i = 0; i < n; i++) result[i] = nums[i] + (mx = Math.max(mx, nums[i])) + (i == 0 ? 0L : result[i - 1]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findPrefixScore(self, nums: List[int]) -> List[int]:
        return list(accumulate(x + y for x, y in zip(nums, accumulate(nums, max))))
```
