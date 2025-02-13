# 280 Wiggle Sort 摆动排序

__Description:__

Given an integer array `nums`, reorder it such that `nums[0] <= nums[1] >= nums[2] <= nums[3]...`.

You may assume the input array always has a valid answer.

__Example:__

Example 1:

```text
Input: nums = [3,5,2,1,6,4]
Output: [3,5,1,6,2,4]
Explanation: [1,6,2,5,3,4] is also accepted.
```

Example 2:

```text
Input: nums = [6,6,5,6,3,8]
Output: [6,6,5,6,3,8]
```

__Constraints:__

- `1 <= nums.length <= 5 * 10 ^ 4`
- `0 <= nums[i] <= 10 ^ 4`
- It is __guaranteed__ that there will be an answer for the given input `nums`.

__Follow up:__

Could you solve the problem in `O(n)` time complexity?

__题目描述:__

给你一个的整数数组 `nums`, 将该数组重新排序后使 `nums[0] <= nums[1] >= nums[2] <= nums[3]...`

输入数组总是有一个有效的答案。

__示例:__

示例 1：

```text
输入：nums = [3,5,2,1,6,4]
输出：[3,5,1,6,2,4]
解释：[1,6,2,5,3,4]也是有效的答案
```

示例 2：

```text
输入：nums = [6,6,5,6,3,8]
输出：[6,6,5,6,3,8]
```

__提示：__

- `1 <= nums.length <= 5 * 10 ^ 4`
- `0 <= nums[i] <= 10 ^ 4`
- 输入的 `nums` 保证至少有一个答案。

__进阶：__

你能在 `O(n)` 时间复杂度下解决这个问题吗？

__思路:__

```text
贪心
从 nums[0] 开始尝试
若 nums[0] <= nums[1], 则已经满足条件
否则, 交换 nums[0] 和 nums[1]
交换完成之后有 nums[0] <= nums[1]
从 nums[1] 开始尝试
若 nums[2] <= nums[1], 则已经满足条件
否则, 此时一定有 nums[0] <= nums[1] <= nums[2]
交换 nums[1] 和 nums[2]
交换完成之后有 nums[0] <= nums[1] >= nums[2], 满足摆动排序的条件
再看 nums[2] 和 nums[3]
如果 nums[2] <= nums[3], 则已经满足条件
否则, 此时一定有 nums[1] >= nums[2] >= nums[3]
交换 nums[2] 和 nums[3]
交换完成之后有 nums[1] >= nums[2] <= nums[3], 满足摆动排序的条件
以此类推
所以当 i 为奇数时, nums[i] < nums[i + 1]
或者 i 为偶数时, nums[i] > nums[i + 1]  
交换 nums[i] 和 nums[i + 1] 即可
时间复杂度 O(N), 空间复杂度 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    void wiggleSort(vector<int>& nums) 
    {
        for (int i = 0, n = nums.size(); i < n - 1; i++) if ((!(i & 1) and nums[i] > nums[i + 1]) or ((i & 1) and nums[i] < nums[i + 1])) swap(nums[i], nums[i + 1]);
    }
};
```

__Java__:

```Java
class Solution {
    public void wiggleSort(int[] nums) {
        for (int i = 0, n = nums.length; i < n - 1; i++) {
            if ((((i & 1) == 0) && nums[i] > nums[i + 1]) || (((i & 1) == 1) && nums[i] < nums[i + 1])) {
                nums[i] ^= nums[i + 1];
                nums[i + 1] ^= nums[i];
                nums[i] ^= nums[i + 1];
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def wiggleSort(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        for i in range(len(nums) - 1):
            if ((not (i & 1) and nums[i] > nums[i + 1]) or ((i & 1) and nums[i] < nums[i + 1])):
                nums[i], nums[i + 1] = nums[i + 1], nums[i]
```
