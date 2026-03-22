# 164 Maximum Gap 最大间距

__Description__:
Given an unsorted array, find the maximum difference between the successive elements in its sorted form.

Return 0 if the array contains less than 2 elements.

__Example:__

Example 1:

Input: [3,6,9,1]
Output: 3
Explanation: The sorted form of the array is [1,3,6,9], either
             (3,6) or (6,9) has the maximum difference 3.

Example 2:

Input: [10]
Output: 0
Explanation: The array contains less than 2 elements, therefore return 0.

__Note:__
You may assume all elements in the array are non-negative integers and fit in the 32-bit signed integer range.
Try to solve it in linear time/space.

__题目描述__:
给定一个无序的数组，找出数组在排序之后，相邻元素之间最大的差值。

如果数组元素个数小于 2，则返回 0。

__示例 :__

示例 1:

输入: [3,6,9,1]
输出: 3
解释: 排序后的数组是 [1,3,6,9], 其中相邻元素 (3,6) 和 (6,9) 之间都存在最大差值 3。

示例 2:

输入: [10]
输出: 0
解释: 数组元素个数小于 2，因此返回 0。

__说明：__

你可以假设数组中所有元素都是非负整数，且数值在 32 位有符号整数范围内。
请尝试在线性时间复杂度和空间复杂度的条件下解决此问题。

__思路__:

1. 排序之后后项减前项取最大值即可
时间复杂度O(nlgn), 空间复杂度O(1)
2. 桶排序
桶为最大值和最小值中间的一个区间
需要记录每个桶中的元素的最小值和最大值
桶的区间长度gap >= (max - min) / (n - 1), 当且仅当, 数组为等差数列取等号
桶的数量 b = (max - min) / (n + 1)
通过比较相邻非空桶的距离(距离为后一个非空桶的最小值 ➖ 前一个非空桶的最大值)即可
时间复杂度O(n), 空间复杂度O(b), 其中 n为数组nums中元素的个数, b为桶的个数

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maximumGap(vector<int>& nums) 
    {
        if (nums.size() < 2) return 0;
        int nums_max = *max_element(nums.begin(), nums.end()), nums_min = *min_element(nums.begin(), nums.end()), result = 0, n = nums.size();
        if (nums_max == nums_min) return 0;
        vector<int> max_nums(n + 1);
        vector<int> min_nums(n + 1);
        vector<bool> bucket(n + 1);
        for (auto num : nums)
        {
            int index = helper(num, n, nums_max, nums_min);
            max_nums[index] = bucket[index] ? max(max_nums[index], num) : num;
            min_nums[index] = bucket[index] ? min(min_nums[index], num) : num;
            bucket[index] = true;
        }
        int pre = max_nums[0];
        for (int i = 1; i <= n; i++)
        {
            if (bucket[i])
            {
                result = max(result, min_nums[i] - pre);
                pre = max_nums[i];
            }
        }
        return result;
    }
private:
    int helper(long num, long n, long nums_max, long nums_min)
    {
        return (int)((num - nums_min) * n / (nums_max - nums_min));
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumGap(int[] nums) {
        if (nums.length < 2) return 0;
        int result = 0;
        Arrays.sort(nums);
        for (int i = 1; i < nums.length; i++) result = Math.max(nums[i] - nums[i - 1], result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumGap(self, nums: List[int]) -> int:
        return max((j - i for i, j in zip(sorted(nums)[:-1], sorted(nums)[1:]))) if len(nums) > 1 else 0
```
