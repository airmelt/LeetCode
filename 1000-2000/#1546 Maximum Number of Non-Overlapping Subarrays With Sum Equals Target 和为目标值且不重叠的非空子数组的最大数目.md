# 1546 Maximum Number of Non-Overlapping Subarrays With Sum Equals Target 和为目标值且不重叠的非空子数组的最大数目

__Description:__

Given an array `nums` and an integer `target`, return _the maximum number of __non-empty__ __non-overlapping__ subarrays such that the sum of values in each subarray is equal to_ `target`.

__Example:__

Example 1:

```text
Input: nums = [1,1,1,1,1], target = 2
Output: 2
Explanation: There are 2 non-overlapping subarrays [1,1,1,1,1] with sum equals to target(2).
```

Example 2:

```text
Input: nums = [-1,3,5,1,4,2,-9], target = 6
Output: 2
Explanation: There are 3 subarrays with sum equal to 6.
([5,1], [4,2], [3,5,1,4,2,-9]) but only the first 2 are non-overlapping.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 4 <= nums[i] <= 10 ^ 4`
- `0 <= target <= 10 ^ 6`

__题目描述:__

给你一个数组 `nums` 和一个整数 `target` 。

请你返回 __非空不重叠__ 子数组的最大数目，且每个子数组中数字和都为 `target` 。

__示例:__

示例 1：

```text
输入：nums = [1,1,1,1,1], target = 2
输出：2
解释：总共有 2 个不重叠子数组（加粗数字表示） [1,1,1,1,1] ，它们的和为目标值 2 。
```

示例 2：

```text
输入：nums = [-1,3,5,1,4,2,-9], target = 6
输出：2
解释：总共有 3 个子数组和为 6 。
([5,1], [4,2], [3,5,1,4,2,-9]) 但只有前 2 个是不重叠的。
```

示例 3：

```text
输入：nums = [-2,6,6,3,5,4,1,2,8], target = 10
输出：3
```

示例 4：

```text
输入：nums = [0,0,0], target = 0
输出：3
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 4 <= nums[i] <= 10 ^ 4`
- `0 <= target <= 10 ^ 6`

__思路:__

```text
贪心
从左到右遍历只要遇到了前缀和为 target 的子数组立即加入到结果中
用一个变量 total 记录当前的数字和
将 total 存入哈希表中加快查找, 初始化哈希表加入 0
只要哈希表中存在 total - target 就说明有子数组的和为 target, 因为 total 记录的是前缀和, 哈希表中记录的也是前缀和, 两个前缀和的差正好为 target
找到一个子数组后, 将哈希表清空, 前缀和置零继续下一个子数组的查找
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxNonOverlapping(vector<int>& nums, int target) 
    {
        unordered_set<int> s;
        s.insert(0);
        int total = 0, result = 0;
        for (const auto& num: nums) 
        {
            total += num;
            if (s.find(total - target) != s.end())  
            {
                ++result;
                s.clear();
                total = 0;
            }
            s.insert(total);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxNonOverlapping(int[] nums, int target) {
        Set<Integer> set = new HashSet<>();
        set.add(0);
        int total = 0, result = 0;
        for (int num: nums) {
            total += num;
            if (set.contains(total - target)) {
                ++result;
                set.clear();
                total = 0;
            }
            set.add(total);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxNonOverlapping(self, nums: List[int], target: int) -> int:
        s, result, total = {0}, 0, 0
        for num in nums:
            total += num
            if total - target in s:
                result += 1
                s, total = {0}, 0
            s.add(total)
        return result
```
