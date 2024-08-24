# 2221 Find Triangular Sum of an Array 数组的三角和

__Description:__

You are given a __0-indexed__ integer array `nums`, where `nums[i]` is a digit between `0` and `9` (__inclusive__).

The __triangular sum__ of `nums` is the value of the only element present in `nums` after the following process terminates:

Return _the triangular sum of_ `nums`.

__Example:__

Example 1:

![2221-1](https://assets.leetcode.com/uploads/2022/02/22/ex1drawio.png)

```text
Input: nums = [1,2,3,4,5]
Output: 8
Explanation:
The above diagram depicts the process from which we obtain the triangular sum of the array.
```

Example 2:

```text
Input: nums = [5]
Output: 5
Explanation:
Since there is only one element in nums, the triangular sum is the value of that element itself.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，其中 `nums[i]` 是 `0` 到 `9` 之间（两者都包含）的一个数字。

`nums` 的 __三角和__ 是执行以下操作以后最后剩下元素的值:

请你返回 `nums` 的三角和。

__示例:__

示例 1：

![2221-2](https://assets.leetcode.com/uploads/2022/02/22/ex1drawio.png)

```text
输入：nums = [1,2,3,4,5]
输出：8
解释：
上图展示了得到数组三角和的过程。
```

示例 2：

```text
输入：nums = [5]
输出：5
解释：
由于 nums 中只有一个元素，数组的三角和为这个元素自己。
```

__提示：__

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 9`

__思路:__

```text
1. 模拟
遍历 n - 1 次数组
每次遍历将数组中的元素相邻两两相加, 并对 10 取模
可以直接在原数组上进行操作, 无需额外空间
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 数学
根据组合数的性质, 可以直接计算出结果
每个数需要加 comb(n - 1, i) 次
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int triangularSum(vector<int>& nums)
    {
        for (int n = nums.size(); n > 1; n--) for (int i = 1; i < n; i++) nums[i - 1] = (nums[i - 1] + nums[i]) % 10;
        return nums.front();
    }
};
```

__Java__:

```Java
class Solution {
    public int triangularSum(int[] nums) {
        for (int n = nums.length; n > 1; n--) for (int i = 1; i < n; i++) nums[i - 1] = (nums[i - 1] + nums[i]) % 10;
        return nums[0]; 
    }
}
```

__Python__:

```Python
class Solution:
    def triangularSum(self, nums: List[int]) -> int:
        return sum(comb(len(nums) - 1, i) * nums[i] for i in range(len(nums))) % 10
```
