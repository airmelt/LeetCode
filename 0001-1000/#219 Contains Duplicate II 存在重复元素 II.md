# 219 Contains Duplicate II 存在重复元素 II

__Description__:
Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the absolute difference between i and j is at most k.

**Example:**

Example 1:
Input: nums = [1,2,3,1], k = 3
Output: true

Example 2:
Input: nums = [1,0,1,1], k = 1
Output: true

Example 3:
Input: nums = [1,2,3,1,2,3], k = 2
Output: false

__题目描述__:
给定一个整数数组和一个整数 k，判断数组中是否存在两个不同的索引 i 和 j，使得 nums [i] = nums [j]，并且 i 和 j 的差的绝对值最大为 k。

__示例__:

示例 1:
输入: nums = [1,2,3,1], k = 3
输出: true

示例 2:
输入: nums = [1,0,1,1], k = 1
输出: true

示例 3:
输入: nums = [1,2,3,1,2,3], k = 2
输出: false

__思路__:

利用 map记录出现过的数字, 如果 map中已经存放了这个数字, 要么满足题意输出 true, 要么更新下标值
时间复杂度O(n), 空间复杂度O(k), map中最多记录 k个元素

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) 
    {
        if (nums.size() < 2) return false;
        map<int,int> temp;
        for (int i = 0; i < nums.size(); i++) 
        {
            if (temp.count(nums[i])) 
            {
                if (i - temp[nums[i]] <= k) return true;
                else temp[nums[i]] = i;
            }
            else temp[nums[i]] = i;
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        if (nums.length < 2) return false;
        Map<Integer, Integer> temp = new HashMap<>(k);
        for (int i = 0; i < nums.length; i++) {
            if (temp.containsKey(nums[i])) {
                if (i - temp.get(nums[i]) <= k) return true;
                else temp.put(nums[i], i);
            } else {
                temp.put(nums[i], i);
            }
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        if len(nums) < 2:
            return False
        temp = {}
        for i, num in enumerate(nums):
            if num in temp:
                if i - temp[num] <= k:
                    return True
                else:
                    temp[num] = i
            else:
                temp[num] = i
        return False
```
