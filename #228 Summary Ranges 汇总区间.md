__Description__:
Given a sorted integer array without duplicates, return the summary of its ranges.

__Example:__
Example 1:

Input:  [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: 0,1,2 form a continuous range; 4,5 form a continuous range.

Example 2:

Input:  [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: 2,3,4 form a continuous range; 8,9 form a continuous range.

__题目描述__:
给定一个无重复元素的有序整数数组，返回数组区间范围的汇总。

__示例 :__
示例 1:

输入: [0,1,2,4,5,7]
输出: ["0->2","4->5","7"]
解释: 0,1,2 可组成一个连续的区间; 4,5 可组成一个连续的区间。

示例 2:

输入: [0,2,3,4,6,8,9]
输出: ["0","2->4","6","8->9"]
解释: 2,3,4 可组成一个连续的区间; 8,9 可组成一个连续的区间。

__思路__:
双指针
右指针负责找到第一个 nums[r] + 1 != nums[r + 1]的位置
左指针指向上一个区间的右边 + 1的位置
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<string> summaryRanges(vector<int>& nums) 
    {
        vector<string> result;
        int l = 0, r = 0;
        while (r < nums.size())
        {
            while (r < nums.size() - 1 && nums[r] + 1 == nums[r + 1]) ++r;
            if (l != r) result.push_back(to_string(nums[l]) + "->" + to_string(nums[r]));
            else result.push_back(to_string(nums[l]));
            ++r;
            l = r;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public List<String> summaryRanges(int[] nums) {
        List<String> result = new ArrayList<>();
        int l = 0, r = 0;
        while (r < nums.length) {
            while (r < nums.length - 1 && nums[r] + 1 == nums[r + 1]) ++r;
            if (l != r) result.add(String.valueOf(nums[l]) + "->" + String.valueOf(nums[r]));
            else result.add(String.valueOf(nums[l]));
            ++r;
            l = r;
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        if not nums:
            return []
        result = [[nums[0]]]
        for i in range(1, len(nums)):
            if nums[i] - nums[i - 1] != 1:
                result[-1].append(nums[i - 1])
                result.append([nums[i]])
        result[-1].append(nums[-1])
        return [[f'{a}->{b}', f'{a}'][a == b] for a, b in result]
```