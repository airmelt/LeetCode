# 1480 Running Sum of 1d Array 一维数组的动态和

__Description:__

Given an array  `nums`. We define a running sum of an array as  `runningSum[i] = sum(nums[0]…nums[i])`.

Return the running sum of  `nums`.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4]
Output: [1,3,6,10]
Explanation: Running sum is obtained as follows: [1, 1+2, 1+2+3, 1+2+3+4].
```

Example 2:

```text
Input: nums = [1,1,1,1,1]
Output: [1,2,3,4,5]
Explanation: Running sum is obtained as follows: [1, 1+1, 1+1+1, 1+1+1+1, 1+1+1+1+1].
```

Example 3:

```text
Input: nums = [3,1,2,10,1]
Output: [3,4,6,16,17]
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `-10 ^ 6 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个数组  `nums` 。数组「动态和」的计算公式为： `runningSum[i] = sum(nums[0]…nums[i])` 。

请返回  `nums` 的动态和。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4]
输出：[1,3,6,10]
解释：动态和计算过程为 [1, 1+2, 1+2+3, 1+2+3+4] 。
```

示例 2：

```text
输入：nums = [1,1,1,1,1]
输出：[1,2,3,4,5]
解释：动态和计算过程为 [1, 1+1, 1+1+1, 1+1+1+1, 1+1+1+1+1] 。
```

示例 3：

```text
输入：nums = [3,1,2,10,1]
输出：[3,4,6,16,17]
```

__提示：__

- `1 <= nums.length <= 1000`
- `-10 ^ 6 <= nums[i] <= 10 ^ 6`

__思路:__

```text
前缀和
简单的前缀和
使用 nums[i] += nums[i - 1]
可以在原地完成求前缀和的操作
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> runningSum(vector<int>& nums) 
    {
        for (int i = 1, n = nums.size(); i < n; i++) nums[i] += nums[i - 1];
        return nums;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] runningSum(int[] nums) {
        for (int i = 1, n = nums.length; i < n; i++) nums[i] += nums[i - 1];
        return nums;
    }
}
```

__Python__:

```Python
class Solution:
    def runningSum(self, nums: List[int]) -> List[int]:
        return list(accumulate(nums))
```
