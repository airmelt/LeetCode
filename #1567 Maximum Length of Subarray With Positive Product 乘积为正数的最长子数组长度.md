# 1567 Maximum Length of Subarray With Positive Product 乘积为正数的最长子数组长度

__Description:__

Given an array of integers `nums`, find the maximum length of a subarray where the product of all its elements is positive.

A subarray of an array is a consecutive sequence of zero or more values taken out of that array.

Return the maximum length of a subarray with positive product.

__Example:__

Example 1:

```text
Input: nums = [1,-2,-3,4]
Output: 4
Explanation: The array nums already has a positive product of 24.
```

Example 2:

```text
Input: nums = [0,1,-2,-3,-4]
Output: 3
Explanation: The longest subarray with positive product is [1,-2,-3] which has a product of 6.
Notice that we cannot include 0 in the subarray since that'll make the product 0 which is not positive.
```

Example 3:

```text
Input: nums = [-1,-2,-3,0,1]
Output: 2
Explanation: The longest subarray with positive product is [-1,-2] or [-2,-3].
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `nums` ，请你求出乘积为正数的最长子数组的长度。

一个数组的子数组是由原数组中零个或者更多个连续数字组成的数组。

请你返回乘积为正数的最长子数组长度。

__示例:__

示例  1：

```text
输入：nums = [1,-2,-3,4]
输出：4
解释：数组本身乘积就是正数，值为 24 。
```

示例 2：

```text
输入：nums = [0,1,-2,-3,-4]
输出：3
解释：最长乘积为正数的子数组为 [1,-2,-3] ，乘积为 6 。
注意，我们不能把 0 也包括到子数组中，因为这样乘积为 0 ，不是正数。
```

示例 3：

```text
输入：nums = [-1,-2,-3,0,1]
输出：2
解释：乘积为正数的最长子数组是 [-1,-2] 或者 [-2,-3] 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__思路:__

```text
动态规划
设当前正数累积有 pos 个, 当前负数累积有 neg 个
当遍历到 nums[i] 为正数时, pos 自增, neg 如果为正数, 自增, 否则保持 0
当遍历到 nums[i] 为负数时, pos 按 neg 为正的逻辑修改值, neg 赋值为 pos + 1, 因为 pos 是正数的个数, 遇到负数乘积就会为负, 此时 neg 刚好为上一次 pos 的值加上当前的值
当 nums[i] 为 0 时, 将 pos, neg 都置零
result 取 pos 的最大值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int getMaxLen(vector<int>& nums) 
    {
        int n = nums.size(), pos = nums.front() > 0, neg = nums.front() < 0, result = pos;
        for (int i = 1; i < n; i++)
        {
            if (nums[i] > 0)
            {
                ++pos;
                neg = neg > 0 ? neg + 1 : 0;
            }
            else if (nums[i] < 0)
            {
                int new_pos = neg > 0 ? neg + 1 : 0, new_neg = pos + 1;
                pos = new_pos;
                neg = new_neg;
            }
            else pos = neg = 0;
            result = max(result, pos);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int getMaxLen(int[] nums) {
        int n = nums.length, pos = nums[0] > 0 ? 1 : 0, neg = nums[0] < 0 ? 1 : 0, result = pos;
        for (int i = 1; i < n; i++) {
            if (nums[i] > 0) {
                ++pos;
                neg = neg > 0 ? neg + 1 : 0;
            } else if (nums[i] < 0) {
                int newPos = neg > 0 ? neg + 1 : 0, newNeg = pos + 1;
                pos = newPos;
                neg = newNeg;
            } else pos = neg = 0;
            result = Math.max(result, pos);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getMaxLen(self, nums: List[int]) -> int:
        nums, last_zero, result, negs, n = [0] + nums + [0], 0, 0, [], len(nums) + 2
        for i in range(1, n): 
            if not nums[i]:
                result, last_zero, negs = max(result, i - last_zero - 1) if not (len(negs) & 1) else max(result, i - negs[0] - 1, negs[-1] - last_zero - 1), i, []
            elif nums[i] < 0:
                negs.append(i)
        return result
```
