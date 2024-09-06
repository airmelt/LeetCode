# 2239 Find Closest Number to Zero 找到最接近 0 的数字

__Description:__

Given an integer array `nums` of size `n`, return _the number with the value __closest__ to_ `0` _in_ `nums`. If there are multiple answers, return _the number with the __largest__ value_.

__Example:__

Example 1:

```text
Input: nums = [-4,-2,1,4,8]
Output: 1
Explanation:
The distance from -4 to 0 is |-4| = 4.
The distance from -2 to 0 is |-2| = 2.
The distance from 1 to 0 is |1| = 1.
The distance from 4 to 0 is |4| = 4.
The distance from 8 to 0 is |8| = 8.
Thus, the closest number to 0 in the array is 1.
```

Example 2:

```text
Input: nums = [2,-1,1]
Output: 1
Explanation: 1 and -1 are both the closest numbers to 0, so 1 being larger is returned.
```

__Constraints:__

- `1 <= n <= 1000`
- `-10 ^ 5 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个长度为 `n` 的整数数组 `nums` ，请你返回 `nums` 中最 __接近__ `0` 的数字。如果有多个答案，请你返回它们中的 __最大值__ 。

__示例:__

示例 1：

```text
输入：nums = [-4,-2,1,4,8]
输出：1
解释：
-4 到 0 的距离为 |-4| = 4 。
-2 到 0 的距离为 |-2| = 2 。
1 到 0 的距离为 |1| = 1 。
4 到 0 的距离为 |4| = 4 。
8 到 0 的距离为 |8| = 8 。
所以，数组中距离 0 最近的数字为 1 。
```

示例 2：

```text
输入：nums = [2,-1,1]
输出：1
解释：1 和 -1 都是距离 0 最近的数字，所以返回较大值 1 。
```

__提示：__

- `1 <= n <= 1000`
- `-10 ^ 5 <= nums[i] <= 10 ^ 5`

__思路:__

```text
1. 排序
自定义排序
按照绝对值从小到大排序，如果绝对值相等，按照数值从大到小排序
返回排序后的第一个元素
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
2. 模拟
遍历数组，维护一个最接近 0 的数
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findClosestNumber(vector<int>& nums) 
    {
        int result = nums.front();
        for (const auto& num : nums) if (abs(num) < abs(result) or (abs(num) == abs(result) and num > result)) result = num;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findClosestNumber(int[] nums) {
        int result = nums[0];
        for (int num : nums) if (Math.abs(num) < Math.abs(result) || (Math.abs(num) == Math.abs(result) && num > result)) result = num;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findClosestNumber(self, nums: List[int]) -> int:
        return sorted(nums, key=lambda x: (abs(x), -x))[0]
```
