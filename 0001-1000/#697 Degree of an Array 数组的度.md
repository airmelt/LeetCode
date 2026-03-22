# 697 Degree of an Array 数组的度

__Description__:
Given a non-empty array of non-negative integers nums, the degree of this array is defined as the maximum frequency of any one of its elements.

Your task is to find the smallest possible length of a (contiguous) subarray of nums, that has the same degree as nums.

__Example:__

Example 1:

Input: [1, 2, 2, 3, 1]
Output: 2
Explanation:
The input array has a degree of 2 because both elements 1 and 2 appear twice.
Of the subarrays that have the same degree:
[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
The shortest length is 2. So return 2.

Example 2:

Input: [1,2,2,3,1,4,2]
Output: 6

__Note:__

nums.length will be between 1 and 50,000.
nums[i] will be an integer between 0 and 49,999.

__题目描述__:
给定一个非空且只包含非负数的整数数组 nums, 数组的度的定义是指数组里任一元素出现频数的最大值。

你的任务是找到与 nums 拥有相同大小的度的最短连续子数组，返回其长度。

__示例 :__

示例 1:

输入: [1, 2, 2, 3, 1]
输出: 2
解释:
输入数组的度是2，因为元素1和2的出现频数最大，均为2.
连续子数组里面拥有相同度的有如下所示:
[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
最短连续子数组[2, 2]的长度为2，所以返回2.

示例 2:

输入: [1,2,2,3,1,4,2]
输出: 6

__注意:__

nums.length 在1到50,000区间范围内。
nums[i] 是一个在0到49,999范围内的整数。

__思路__:

用一个 map记录下各个元素出现的位置, 然后找到出现次数最多的元素, 即为数组的度, 然后找到数组的度对应的元素的起始位置即可
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findShortestSubArray(vector<int>& nums) 
    {
        unordered_map<int, vector<int>> m;
        for (int i = 0; i < nums.size(); i++) m[nums[i]].push_back(i);
        int degree = 1, result = INT_MAX;
        for (auto i : m) degree = max(degree, int(i.second.size()));
        for (auto i : m) if (degree == i.second.size()) result = min(result, i.second.back() - i.second[0] + 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findShortestSubArray(int[] nums) {
        Map<Integer, List<Integer>> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (map.get(nums[i]) == null) {
                List<Integer> list = new ArrayList<>();
                list.add(i);
                map.put(nums[i], list);
            } else map.get(nums[i]).add(i);
        }
        int degree = 1, result = Integer.MAX_VALUE;
        for (Integer key : map.keySet()) degree = Math.max(degree, map.get(key).size());
        for (Integer key : map.keySet()) if (degree == map.get(key).size()) result = Math.min(result, map.get(key).get(map.get(key).size() - 1) - map.get(key).get(0) + 1);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findShortestSubArray(self, nums: List[int]) -> int:
        c = collections.Counter(nums)
        d = max(c.values())
        arr = [k for k, v in c.items() if v == d]
        return min([len(nums) - nums[::-1].index(n) - nums.index(n) for n in arr])
```
