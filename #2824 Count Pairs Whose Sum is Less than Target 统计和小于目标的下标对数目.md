# 2824 Count Pairs Whose Sum is Less than Target 统计和小于目标的下标对数目

__Description:__

Given a __0-indexed__ integer array `nums` of length `n` and an integer `target`, return _the number of pairs_ `(i, j)` _where_ `0 <= i < j < n` _and_ `nums[i] + nums[j] < target`.

__Example:__

Example 1:

```text
Input: nums = [-1,1,2,3,1], target = 2
Output: 3
Explanation: There are 3 pairs of indices that satisfy the conditions in the statement:
```

- (0, 1) since 0 < 1 and nums[0] + nums[1] = 0 < target
- (0, 2) since 0 < 2 and nums[0] + nums[2] = 1 < target
- (0, 4) since 0 < 4 and nums[0] + nums[4] = 0 < target

Note that (0, 3) is not counted since nums[0] + nums[3] is not strictly less than the target.

Example 2:

```text
Input: nums = [-6,2,5,-2,-7,-1,3], target = -2
Output: 10
Explanation: There are 10 pairs of indices that satisfy the conditions in the statement:
```

- (0, 1) since 0 < 1 and nums[0] + nums[1] = -4 < target
- (0, 3) since 0 < 3 and nums[0] + nums[3] = -8 < target
- (0, 4) since 0 < 4 and nums[0] + nums[4] = -13 < target
- (0, 5) since 0 < 5 and nums[0] + nums[5] = -7 < target
- (0, 6) since 0 < 6 and nums[0] + nums[6] = -3 < target
- (1, 4) since 1 < 4 and nums[1] + nums[4] = -5 < target
- (3, 4) since 3 < 4 and nums[3] + nums[4] = -9 < target
- (3, 5) since 3 < 5 and nums[3] + nums[5] = -3 < target
- (4, 5) since 4 < 5 and nums[4] + nums[5] = -8 < target
- (4, 6) since 4 < 6 and nums[4] + nums[6] = -4 < target

__Constraints:__

- `1 <= nums.length == n <= 50`
- `-50 <= nums[i], target <= 50`

__题目描述:__

给你一个下标从 __0__ 开始长度为 `n` 的整数数组 `nums` 和一个整数 `target` ，请你返回满足 `0 <= i < j < n` 且 `nums[i] + nums[j] < target` 的下标对 `(i, j)` 的数目。

__示例:__

示例 1：

```text
输入：nums = [-1,1,2,3,1], target = 2
输出：3
解释：总共有 3 个下标对满足题目描述：
```

- (0, 1) ，0 < 1 且 nums[0] + nums[1] = 0 < target
- (0, 2) ，0 < 2 且 nums[0] + nums[2] = 1 < target
- (0, 4) ，0 < 4 且 nums[0] + nums[4] = 0 < target

注意 (0, 3) 不计入答案因为 nums[0] + nums[3] 不是严格小于 target 。

示例 2：

```text
输入：nums = [-6,2,5,-2,-7,-1,3], target = -2
输出：10
解释：总共有 10 个下标对满足题目描述：
```

- (0, 1) ，0 < 1 且 nums[0] + nums[1] = -4 < target
- (0, 3) ，0 < 3 且 nums[0] + nums[3] = -8 < target
- (0, 4) ，0 < 4 且 nums[0] + nums[4] = -13 < target
- (0, 5) ，0 < 5 且 nums[0] + nums[5] = -7 < target
- (0, 6) ，0 < 6 且 nums[0] + nums[6] = -3 < target
- (1, 4) ，1 < 4 且 nums[1] + nums[4] = -5 < target
- (3, 4) ，3 < 4 且 nums[3] + nums[4] = -9 < target
- (3, 5) ，3 < 5 且 nums[3] + nums[5] = -3 < target
- (4, 5) ，4 < 5 且 nums[4] + nums[5] = -8 < target
- (4, 6) ，4 < 6 且 nums[4] + nums[6] = -4 < target

__提示：__

- `1 <= nums.length == n <= 50`
- `-50 <= nums[i], target <= 50`

__思路:__

```text
1. 暴力法
遍历所有数对
找出和小于 target 的数对
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 排序 ➕ 双指针
先将数组排序
双指针相对遍历数组
当 nums[left] + nums[right] < target 时, 所有不大于 right 的都满足题意
否则说明 right 偏大, 移动 right
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要开销为排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countPairs(vector<int>& nums, int target) 
    {
        sort(nums.begin(), nums.end());
        int result = 0, n = nums.size(), left = 0, right = n - 1;
        while (left < right) 
        {
            if (nums[left] + nums[right] < target) result += right - left++;
            else --right;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countPairs(List<Integer> nums, int target) {
        Collections.sort(nums);
        int result = 0, n = nums.size(), left = 0, right = n - 1;
        while (left < right) {
            if (nums.get(left) + nums.get(right) < target) result += right - left++;
            else --right;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countPairs(self, nums: List[int], target: int) -> int:
        return sum(nums[i] + nums[j] < target for i in range(len(nums)) for j in range(i + 1, len(nums)))
```
