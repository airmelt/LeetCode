# 2090 K Radius Subarray Averages 半径为 k 的子数组平均值

__Description:__

You are given a __0-indexed__ array `nums` of `n` integers, and an integer `k`.

The __k-radius average__ for a subarray of `nums` __centered__ at some index `i` with the __radius__ `k` is the average of __all__ elements in `nums` between the indices `i - k` and `i + k` (__inclusive__). If there are less than `k` elements before __or__ after the index `i`, then the __k-radius average__ is `-1`.

Build and return _an array_ `avgs` _of length_ `n` _where_ `avgs[i]` _is the __k-radius average__ for the subarray centered at index_ `i`.

The __average__ of `x` elements is the sum of the `x` elements divided by `x`, using __integer division__. The integer division truncates toward zero, which means losing its fractional part.

- For example, the average of four elements `2`, `3`, `1`, and `5` is `(2 + 3 + 1 + 5) / 4 = 11 / 4 = 2.75`, which truncates to `2`.

__Example:__

Example 1:

![2090-1](https://assets.leetcode.com/uploads/2021/11/07/eg1.png)

```text
Input: nums = [7,4,3,9,1,8,5,2,6], k = 3
Output: [-1,-1,-1,5,4,4,-1,-1,-1]
Explanation:
```

- avg[0], avg[1], and avg[2] are -1 because there are less than k elements before each index.
- The sum of the subarray centered at index 3 with radius 3 is: 7 + 4 + 3 + 9 + 1 + 8 + 5 = 37.
  Using integer division, avg[3] = 37 / 7 = 5.
- For the subarray centered at index 4, avg[4] = (4 + 3 + 9 + 1 + 8 + 5 + 2) / 7 = 4.
- For the subarray centered at index 5, avg[5] = (3 + 9 + 1 + 8 + 5 + 2 + 6) / 7 = 4.
- avg[6], avg[7], and avg[8] are -1 because there are less than k elements after each index.

Example 2:

```text
Input: nums = [100000], k = 0
Output: [100000]
Explanation:
```

- The sum of the subarray centered at index 0 with radius 0 is: 100000.
  avg[0] = 100000 / 1 = 100000.

Example 3:

```text
Input: nums = [8], k = 100000
Output: [-1]
Explanation: 
```

- avg[0] is -1 because there are less than k elements before and after index 0.

__Constraints:__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `0 <= nums[i], k <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始的数组 `nums` ，数组中有 `n` 个整数，另给你一个整数 `k` 。

__半径为 k 的子数组平均值__ 是指: `nums` 中一个以下标 `i` 为 __中心__ 且 __半径__ 为 `k` 的子数组中所有元素的平均值，即下标在 `i - k` 和 `i + k` 范围（__含__ `i - k` 和 `i + k`）内所有元素的平均值。如果在下标 `i` 前或后不足 `k` 个元素，那么 __半径为 k 的子数组平均值__ 是 `-1` 。

构建并返回一个长度为 `n` 的数组 `avgs` ，其中 `avgs[i]` 是以下标 `i` 为中心的子数组的 __半径为 k 的子数组平均值__ 。

`x` 个元素的 __平均值__ 是 `x` 个元素相加之和除以 `x` ，此时使用截断式 __整数除法__ ，即需要去掉结果的小数部分。

- 例如，四个元素 `2`、 `3`、 `1` 和 `5` 的平均值是 `(2 + 3 + 1 + 5) / 4 = 11 / 4 = 2.75`，截断后得到 `2` 。

__示例:__

示例 1：

![2090-2](https://assets.leetcode.com/uploads/2021/11/07/eg1.png)

```text
输入：nums = [7,4,3,9,1,8,5,2,6], k = 3
输出：[-1,-1,-1,5,4,4,-1,-1,-1]
解释：
```

- avg[0]、avg[1] 和 avg[2] 是 -1 ，因为在这几个下标前的元素数量都不足 k 个。
- 中心为下标 3 且半径为 3 的子数组的元素总和是：7 + 4 + 3 + 9 + 1 + 8 + 5 = 37 。
  使用截断式 整数除法，avg[3] = 37 / 7 = 5 。
- 中心为下标 4 的子数组，avg[4] = (4 + 3 + 9 + 1 + 8 + 5 + 2) / 7 = 4 。
- 中心为下标 5 的子数组，avg[5] = (3 + 9 + 1 + 8 + 5 + 2 + 6) / 7 = 4 。
- avg[6]、avg[7] 和 avg[8] 是 -1 ，因为在这几个下标后的元素数量都不足 k 个。

示例 2：

```text
输入：nums = [100000], k = 0
输出：[100000]
解释：
```

- 中心为下标 0 且半径 0 的子数组的元素总和是：100000 。
  avg[0] = 100000 / 1 = 100000 。

示例 3：

```text
输入：nums = [8], k = 100000
输出：[-1]
解释：
```

- avg[0] 是 -1 ，因为在下标 0 前后的元素数量均不足 k 。

__提示：__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `0 <= nums[i], k <= 10 ^ 5`

__思路:__

```text
模拟
初始化 result 数组为 -1
如果 2 * k 不大于 n 才有必要计算
令 radius = 2 * k + 1
先得到 nums[:radius] 的和 s, 则 result[k] = s / radius
以后以 s 为滑动窗口, 减去第一个元素并加上当前元素即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> getAverages(vector<int>& nums, int k) 
    {
        int n = nums.size(), radius = (k << 1) + 1, i = 0;
        vector<int> result(n, -1);
        if (k << 1 < n)
        {
            long long s = accumulate(nums.begin(), nums.begin() + radius, 0LL);
            result[k] = (int)(s / radius);
            for (int i = k + 1; i < n - k; i++)
            {
                s += nums[i + k] - nums[i - k - 1];
                result[i] = s / radius;
            }
        }
        return result;  
    }
};
```

__Java__:

```Java
class Solution {
    public int[] getAverages(int[] nums, int k) {
        int n = nums.length, result[] = new int[n], radius = (k << 1) + 1;
        Arrays.fill(result, -1);
        long s = 0;
        for (int i = 0; i < n; i++) {
            s += nums[i];
            if (i >= (k << 1)) {
                result[i - k] = (int)(s / radius);
                s -= nums[i - (k << 1)];
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getAverages(self, nums: List[int], k: int) -> List[int]:
        result, radius = [-1] * (n := len(nums)), (k << 1) + 1
        if k << 1 < n:
            result[k] = (total := sum(nums[:radius])) // radius
            for i in range(k + 1, n - k):
                total += nums[i + k] - nums[i - k - 1]
                result[i] = total // radius
        return result
```
