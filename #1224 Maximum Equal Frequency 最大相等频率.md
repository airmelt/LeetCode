# 1224 Maximum Equal Frequency 最大相等频率

__Description:__

Given an array nums of positive integers, return the longest possible length of an array prefix of nums, such that it is possible to remove exactly one element from this prefix so that every number that has appeared in it will have the same number of occurrences.

If after removing one element there are no remaining elements, it's still considered that every appeared number has the same number of ocurrences (0).

__Example:__

Example 1:

Input: nums = [2,2,1,1,5,3,3,5]
Output: 7
Explanation: For the subarray [2,2,1,1,5,3,3] of length 7, if we remove nums[4] = 5, we will get [2,2,1,1,3,3], so that each number will appear exactly twice.
Example 2:

Input: nums = [1,1,1,2,2,2,3,3,3,4,4,4,5]
Output: 13

__Constraints:__

2 <= nums.length <= 10^5
1 <= nums[i] <= 10^5

__题目描述：__

给你一个正整数数组 nums，请你帮忙从该数组中找出能满足下面要求的 最长 前缀，并返回该前缀的长度：

从前缀中 恰好删除一个 元素后，剩下每个数字的出现次数都相同。
如果删除这个元素后没有剩余元素存在，仍可认为每个数字都具有相同的出现次数（也就是 0 次）。

__示例：__

示例 1：

输入：nums = [2,2,1,1,5,3,3,5]
输出：7
解释：对于长度为 7 的子数组 [2,2,1,1,5,3,3]，如果我们从中删去 nums[4] = 5，就可以得到 [2,2,1,1,3,3]，里面每个数字都出现了两次。
示例 2：

输入：nums = [1,1,1,2,2,2,3,3,3,4,4,4,5]
输出：13

__提示：__

2 <= nums.length <= 10^5
1 <= nums[i] <= 10^5

__思路：__

模拟
用哈希表统计各个数字出现的次数
记录并更新出现次数的最大值和出现次数的最大值出现的次数
总共只有 3 种情况需要更新结果, 最后的 4 是可以除去的
每个数字仅出现 1 次, 123456/1234564
数组中只有一种数字, 111111/1111114
每种数字出现次数相同, 1112223334/1112223334444
当出现次数的最大值为 1, 说明数组中只有独特的 i 个值, 123456
当出现次数的最大值和出现次数的最大值出现的次数的乘积为 i, 说明数组中出现的值不需要删除, 有两种情况, 全为 1 个数字, 111111, 或者除去一个数字之后每个数字出现的次数都是出现次数的最大值, 1111114/1112223334
当出现次数的最大值的出现次数为 1, 且出现次数的最大值为 i // d + 1, 说明出现次数的最大值出现的次数刚好比出现次数的第二大的值(可为0)多出了1, 删掉该值即可, 1234564/1112223334444
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int maxEqualFreq(vector<int>& nums) 
    {
        unordered_map<int, int> map;
        int max_value = 0, max_count = 0, n = nums.size(), d = 0, result = 0;
        for (int i = 0; i < n; i++) 
        {
            if (!map[nums[i]]++) ++d;
            if (max_value < map[nums[i]])
            {
                max_count = 1;
                max_value = map[nums[i]];
            } 
            else if (max_value == map[nums[i]]) ++max_count;
            if (max_value == 1 or max_count * max_value == i or (max_count == 1 and max_value == i / d + 1)) result = i + 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxEqualFreq(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int maxValue = 0, maxCount = 0, n = nums.length, d = 0, result = 0;
        for (int i = 0; i < n; i++) {
            if (!map.containsKey(nums[i])) ++d;
            map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);
            if (maxValue < map.get(nums[i])) {
                maxCount = 1;
                maxValue = map.get(nums[i]);
            } else if (maxValue == map.get(nums[i])) ++maxCount;
            if (maxValue == 1 || maxCount * maxValue == i || (maxCount == 1 && maxValue == i / d + 1)) result = i + 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxEqualFreq(self, nums: List[int]) -> int:
        count, max_value, max_count, result, n, d = defaultdict(int), 0, 0, 0, len(nums), 0
        for i in range(n):
            if not count[nums[i]]:
                d += 1
            count[nums[i]] += 1
            if max_value < count[nums[i]]:
                max_count, max_value = 1, count[nums[i]]
            elif max_value == count[nums[i]]:
                max_count += 1
            if max_value == 1 or max_value * max_count == i or (max_count == 1 and max_value == i // d + 1):
                result = i + 1
        return result
```
