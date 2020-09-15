__Description__:
Given an array nums of n integers where n > 1,  return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

__Example:__

Input:  [1,2,3,4]
Output: [24,12,8,6]

__Constraint: __
It's guaranteed that the product of the elements of any prefix or suffix of the array (including the whole array) fits in a 32 bit integer.

__Note:__
Please solve it without division and in O(n).

__Follow up:__
Could you solve it with constant space complexity? (The output array does not count as extra space for the purpose of space complexity analysis.)

__题目描述__:
给你一个长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。

__示例 :__

输入: [1,2,3,4]
输出: [24,12,8,6]

__提示：__
题目数据保证数组之中任意元素的全部前缀元素和后缀（甚至是整个数组）的乘积都在 32 位整数范围内。

__说明：__
请不要使用除法，且在 O(n) 时间复杂度内完成此题。

__进阶：__
你可以在常数空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组不被视为额外空间。）

__思路__:
1. 用一个 long记录所有乘积, 然后在原数组上用除法修改
2. 类似矩阵主对角线都是 1, 矩阵(i, j) = nums[j](i != j)
然后将每一行乘起来
用 left和 right记录乘积
left和 right分别为 nums[i]左边和 nums[i]右边所有数的乘积
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
class Solution 
{
public:
    vector<int> productExceptSelf(vector<int>& nums) 
    {
        int left = 1, right = 1, n = nums.size();
        vector<int> result(n, 1);
        for (int i = 0; i < n; i++)
        {
            result[i] *= left;
            left *= nums[i];
            result[n - 1 - i] *= right;
            right *= nums[n - 1 - i];
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int left = 1, right = 1, result[] = new int[nums.length];
        Arrays.fill(result, 1);
        for (int i = 0; i < nums.length; i++) {
            result[i] *= left;
            left *= nums[i];
            result[nums.length - 1 - i] *= right;
            right *= nums[nums.length - 1 - i];
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        result, left, right = [1] * len(nums), 1, 1
        for i in range(len(nums)):
            result[i] *= left
            left *= nums[i]
            result[len(nums) - 1 - i] *= right
            right *= nums[len(nums) - 1 - i]
        return result
```