# 525 Contiguous Array 连续数组

__Description__:
Given a binary array, find the maximum length of a contiguous subarray with equal number of 0 and 1.

__Example:__

Example 1:
Input: [0,1]
Output: 2
Explanation: [0, 1] is the longest contiguous subarray with equal number of 0 and 1.

Example 2:
Input: [0,1,0]
Output: 2
Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.

__Note:__
The length of the given binary array will not exceed 50,000.

__题目描述__:
给定一个二进制数组, 找到含有相同数量的 0 和 1 的最长连续子数组（的长度）。

__示例 :__

示例 1:

输入: [0,1]
输出: 2
说明: [0, 1] 是具有相同数量0和1的最长连续子数组。

示例 2:

输入: [0,1,0]
输出: 2
说明: [0, 1] (或 [1, 0]) 是具有相同数量0和1的最长连续子数组。

__注意:__
给定的二进制数组的长度不会超过50000。

__思路__:

前缀和
将 0看作 -1, 转换为求连续子数组和为 0的最长长度
用一个哈希表记录和及下标
如果当前的前缀和出现过, 说明当前下标到哈希表的下标的前缀和为 0, 更新长度
时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findMaxLength(vector<int>& nums) 
    {
        unordered_map<int, int> m;
        m[0] = -1;
        int result = 0, count = 0, n = nums.size();
        for (int i = 0; i < n; i++) 
        {
            count += (nums[i] ? 1 : -1);
            if (m.count(count)) result = max(result, i - m[count]);
            else m[count] = i;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findMaxLength(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, -1);
        int result = 0, count = 0, n = nums.length;
        for (int i = 0; i < n; i++) {
            count += (nums[i] == 0 ? -1 : 1);
            if (map.containsKey(count)) result = Math.max(result, i - map.get(count));
            else map.put(count, i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findMaxLength(self, nums: List[int]) -> int:
        d, result, count = {0: -1}, 0, 0
        for i, num in enumerate(nums):
            count += 1 if num else -1
            if count in d:
                result = max(result, i - d[count])
            else:
                d[count] = i
        return result
```
