# 560 Subarray Sum Equals K 和为K的子数组

__Description__:
Given an array of integers nums and an integer k, return the total number of continuous subarrays whose sum equals to k.

__Example:__

Example 1:

Input: nums = [1,1,1], k = 2
Output: 2

Example 2:

Input: nums = [1,2,3], k = 3
Output: 2

__Constraints:__

1 <= nums.length <= 2 * 10^4
-1000 <= nums[i] <= 1000
-10^7 <= k <= 10^7

__题目描述__:
给定一个整数数组和一个整数 k，你需要找到该数组中和为 k 的连续的子数组的个数。

__示例 :__

示例 1 :

输入:nums = [1,1,1], k = 2
输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。

__说明:__

数组的长度为 [1, 20,000]。
数组中元素的范围是 [-1000, 1000] ，且整数 k 的范围是 [-1e7, 1e7]。

__思路__:

前缀和
用哈希表记录, 查找 target - k, 加到结果中即可
时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int subarraySum(vector<int>& nums, int k) 
    {
        map<int, int> m;
        int s = 0, result = 0;
        m[0] = 1;
        for (auto num : nums) 
        {
            s += num;
            result += m[s - k];
            m[s - k];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        int s = 0, result = 0;
        map.put(0, 1);
        for (int num : nums) {
            s += num;
            if (map.containsKey(s - k)) result += map.get(s - k);
            map.put(s, map.getOrDefault(s, 0) + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        d, s, result = defaultdict(int), 0, 0
        d[0] = 1
        for num in nums:
            s += num
            result += d[s - k]
            d[s] += 1
        return result
```
