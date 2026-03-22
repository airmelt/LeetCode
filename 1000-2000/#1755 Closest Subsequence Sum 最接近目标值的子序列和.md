# 1755 Closest Subsequence Sum 最接近目标值的子序列和

__Description:__

You are given an integer array `nums` and an integer `goal`.

You want to choose a subsequence of `nums` such that the sum of its elements is the closest possible to `goal`. That is, if the sum of the subsequence's elements is `sum`, then you want to __minimize the absolute difference__ `abs(sum - goal)`.

Return _the __minimum__ possible value of_ `abs(sum - goal)`.

Note that a subsequence of an array is an array formed by removing some elements (possibly all or none) of the original array.

__Example:__

Example 1:

```text
Input: nums = [5,-7,3,5], goal = 6
Output: 0
Explanation: Choose the whole array as a subsequence, with a sum of 6.
This is equal to the goal, so the absolute difference is 0.
```

Example 2:

```text
Input: nums = [7,-9,15,-2], goal = -5
Output: 1
Explanation: Choose the subsequence [7,-9,-2], with a sum of -4.
The absolute difference is abs(-4 - (-5)) = abs(1) = 1, which is the minimum.
```

Example 3:

```text
Input: nums = [1,2,3], goal = -7
Output: 7
```

__Constraints:__

- `1 <= nums.length <= 40`
- `-10 ^ 7 <= nums[i] <= 10 ^ 7`
- `-10 ^ 9 <= goal <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `nums` 和一个目标值 `goal` 。

你需要从 `nums` 中选出一个子序列，使子序列元素总和最接近 `goal` 。也就是说，如果子序列元素和为 `sum` ，你需要 __最小化绝对差__ `abs(sum - goal)` 。

返回 `abs(sum - goal)` 可能的 __最小值__ 。

注意，数组的子序列是通过移除原始数组中的某些元素（可能全部或无）而形成的数组。

__示例:__

示例 1：

```text
输入：nums = [5,-7,3,5], goal = 6
输出：0
解释：选择整个数组作为选出的子序列，元素和为 6 。
子序列和与目标值相等，所以绝对差为 0 。
```

示例 2：

```text
输入：nums = [7,-9,15,-2], goal = -5
输出：1
解释：选出子序列 [7,-9,-2] ，元素和为 -4 。
绝对差为 abs(-4 - (-5)) = abs(1) = 1 ，是可能的最小值。
```

示例 3：

```text
输入：nums = [1,2,3], goal = -7
输出：7
```

__提示：__

- `1 <= nums.length <= 40`
- `-10 ^ 7 <= nums[i] <= 10 ^ 7`
- `-10 ^ 9 <= goal <= 10 ^ 9`

__思路:__

```text
状态压缩 ➕ 双指针
如果 n 在 20 以内可以利用
sum[i] = sum[i - (1 << j)] + nums[j]
快速计算出 i 对应的子集的和
遍历子集就能得到答案
但是 n 的范围为 [0, 40]
需要折半查找
设数组的左半部分的子集和为 left_sum, 右半部分的子集和为 right_sum
遍历两个数组就能得到只从两个子集取的情况
最后一种情况是从左边的子集中取 1 个数和右边的子集中取 1 个数的最小值
将两个数组排序
使用双指针遍历两个数组
比较 cur = left_sum[left] + right_sum[right] - goal 的绝对值的最小值
如果 cur > goal, 移动右指针, 否则移动左指针
时间复杂度为 O(N * 2 ^ (N / 2)), 空间复杂度为 O(N * 2 ^ (N / 2))
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minAbsDifference(vector<int>& nums, int goal) 
    {
        int n = nums.size(), half = n >> 1, left_half = half, right_half = n - left_half, left_total = 1 << left_half, right_total = 1 << right_half, result = INT_MAX, left = 0, right = right_total - 1, cur = 0;
        vector<int> left_sum(left_total), right_sum(right_total);
        for (int i = 1; i < left_total; i++)
        {
            for (int j = 0; j < left_half; j++)
            {
                if (!(i & (1 << j))) continue;
                left_sum[i] = left_sum[i - (1 << j)] + nums[j];
                break;
            }
        }    
        for (int i = 1; i < right_total; i++)
        {
            for (int j = 0; j < right_half; j++)
            {
                if (!(i & (1 << j))) continue;
                right_sum[i] = right_sum[i - (1 << j)] + nums[left_half + j];
                break;
            }
        }
        sort(left_sum.begin(), left_sum.end());
        sort(right_sum.begin(), right_sum.end());  
        for (int x : left_sum) result = min(result, abs(goal - x));
        for (int x : right_sum) result = min(result, abs(goal - x));
        while (left < left_total and right > -1)
        {
            result = min(result, abs(goal - (cur = left_sum[left] + right_sum[right])));
            if (cur > goal) --right;
            else ++left;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minAbsDifference(int[] nums, int goal) {
        int n = nums.length, half = n >> 1, leftHalf = half, rightHalf = n - leftHalf, leftTotal = 1 << leftHalf, rightTotal = 1 << rightHalf, result = Integer.MAX_VALUE, left = 0, right = rightTotal - 1, leftSum[] = new int[leftTotal], rightSum[] = new int[rightTotal], cur = 0;
        for (int i = 1; i < leftTotal; i++) {
            for (int j = 0; j < leftHalf; j++) {
                if ((i & (1 << j)) == 0) continue;
                leftSum[i] = leftSum[i - (1 << j)] + nums[j];
                break;
            }
        }
        for (int i = 1; i < rightTotal; i++) {
            for (int j = 0; j < rightHalf; j++) {
                if ((i & (1 << j)) == 0) continue;
                rightSum[i] = rightSum[i - (1 << j)] + nums[leftHalf + j];
                break;
            }
        }
        Arrays.sort(leftSum);
        Arrays.sort(rightSum);
        for (int x : leftSum) result = Math.min(result, Math.abs(goal - x));
        for (int x : rightSum) result = Math.min(result, Math.abs(goal - x));
        while (left < leftTotal && right > -1) {
            result = Math.min(result, Math.abs(goal - (cur = leftSum[left] + rightSum[right])));
            if (cur > goal) --right;
            else ++left;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minAbsDifference(self, nums: List[int], goal: int) -> int:
        left_sum, right_sum = [0] * (left_total := 1 << (left_half := (n := len(nums)) >> 1)), [0] * (right_total := 1 << (right_half := n - left_half))
        for i in range(1, left_total):
            for j in range(left_half):
                if not i & (1 << j):
                    continue
                left_sum[i] = left_sum[i - (1 << j)] + nums[j]
                break
        for i in range(1, right_total):
            for j in range(right_half):
                if not i & (1 << j):
                    continue
                right_sum[i] = right_sum[i - (1 << j)] + nums[left_half + j]
                break
        result, left, right = min(min(abs(goal - x) for x in left_sum), min(abs(goal - x) for x in right_sum)), 0, right_total - 1
        left_sum.sort()
        right_sum.sort()
        while left < left_total and right > -1:
            result = min(result, abs(goal - (cur := left_sum[left] + right_sum[right])))
            if cur > goal:
                right -= 1
            else:
                left += 1
        return result
```
