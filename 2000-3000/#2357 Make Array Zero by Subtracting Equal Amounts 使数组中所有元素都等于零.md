# 2357 Make Array Zero by Subtracting Equal Amounts 使数组中所有元素都等于零

__Description:__

You are given a non-negative integer array `nums`. In one operation, you must:

- Choose a positive integer `x` such that `x` is less than or equal to the __smallest non-zero__ element in `nums`.
- Subtract `x` from every __positive__ element in `nums`.

Return _the __minimum__ number of operations to make every element in_ `nums` _equal to_ `0`.

__Example:__

Example 1:

```text
Input: nums = [1,5,0,3,5]
Output: 3
Explanation:
In the first operation, choose x = 1. Now, nums = [0,4,0,2,4].
In the second operation, choose x = 2. Now, nums = [0,2,0,0,2].
In the third operation, choose x = 2. Now, nums = [0,0,0,0,0].
```

Example 2:

```text
Input: nums = [0]
Output: 0
Explanation: Each element in nums is already 0 so no operations are needed.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`

__题目描述:__

给你一个非负整数数组 `nums` 。在一步操作中，你必须:

- 选出一个正整数 `x` ， `x` 需要小于或等于 `nums` 中 __最小__ 的 __非零__ 元素。
- `nums` 中的每个正整数都减去 `x`。

返回使 `nums` 中所有元素都等于 `0` 需要的 __最少__ 操作数。

__示例:__

示例 1：

```text
输入：nums = [1,5,0,3,5]
输出：3
解释：
第一步操作：选出 x = 1 ，之后 nums = [0,4,0,2,4] 。
第二步操作：选出 x = 2 ，之后 nums = [0,2,0,0,2] 。
第三步操作：选出 x = 2 ，之后 nums = [0,0,0,0,0] 。
```

示例 2：

```text
输入：nums = [0]
输出：0
解释：nums 中的每个元素都已经是 0 ，所以不需要执行任何操作。
```

__提示：__

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`

__思路:__

```text
贪心
每次可以让最小的数字归零
也就是要求出不同非零元素的个数
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumOperations(vector<int>& nums) 
    {
        return (set<int> (nums.begin(), nums.end()).size()) - (find(nums.begin(), nums.end(), 0) != nums.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumOperations(int[] nums) {
        return Arrays.stream(nums).filter(r -> r > 0).boxed().collect(Collectors.toSet()).size();
    }
}
```

__Python__:

```Python
class Solution:
    def minimumOperations(self, nums: List[int]) -> int:
        return len(set(nums) - {0})
```
