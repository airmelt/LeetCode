# 2441 Largest Positive Integer That Exists With Its Negative 与对应负数同时存在的最大正整数

__Description:__

Given an integer array `nums` that __does not contain__ any zeros, find __the largest positive__ integer `k` such that `-k` also exists in the array.

Return _the positive integer_ `k`. If there is no such integer, return `-1`.

__Example:__

Example 1:

```text
Input: nums = [-1,2,-3,3]
Output: 3
Explanation: 3 is the only valid k we can find in the array.
```

Example 2:

```text
Input: nums = [-1,10,6,7,-7,1]
Output: 7
Explanation: Both 1 and 7 have their corresponding negative values in the array. 7 has a larger value.
```

Example 3:

```text
Input: nums = [-10,8,6,7,-2,-3]
Output: -1
Explanation: There is no a single valid k, we return -1.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `-1000 <= nums[i] <= 1000`
- `nums[i] != 0`

__题目描述:__

给你一个 __不包含__ 任何零的整数数组 `nums` ，找出自身与对应的负数都在数组中存在的最大正整数 `k` 。

返回正整数 `k` ，如果不存在这样的整数，返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [-1,2,-3,3]
输出：3
解释：3 是数组中唯一一个满足题目要求的 k 。
```

示例 2：

```text
输入：nums = [-1,10,6,7,-7,1]
输出：7
解释：数组中存在 1 和 7 对应的负数，7 的值更大。
```

示例 3：

```text
输入：nums = [-10,8,6,7,-2,-3]
输出：-1
解释：不存在满足题目要求的 k ，返回 -1 。
```

__提示：__

- `1 <= nums.length <= 1000`
- `-1000 <= nums[i] <= 1000`
- `nums[i] != 0`

__思路:__

```text
1. 暴力法
每次碰到一个正数 x
判断是否存在 -x
如果存在则更新答案为 max(result, x)
默认答案为 -1
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 哈希表
将数组转化为哈希表
如果存在 x 和 -x, 则更新答案为 max(result, x)
或者可以一次遍历
将 x 加入哈希表
检查 -x 是否存在
如果存在则更新答案为 max(result, abs(x))
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findMaxK(vector<int>& nums) 
    {
        unordered_set<int> m(nums.begin(), nums.end());
        for (int i = 1000; i; i--) if (m.count(i) and m.count(-i)) return i;
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int findMaxK(int[] nums) {
        int result = -1;
        var set = new HashSet<Integer>();
        for (int num : nums) {
            if (set.contains(-num)) result = Math.max(result, Math.abs(num));
            set.add(num);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findMaxK(self, nums: List[int]) -> int:
        return max((x for x in nums if x > 0 and -x in nums), default=-1)
```
