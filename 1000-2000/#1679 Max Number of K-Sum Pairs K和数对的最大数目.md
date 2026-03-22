# 1679 Max Number of K-Sum Pairs K和数对的最大数目

__Description:__

You are given an integer array `nums` and an integer `k`.

In one operation, you can pick two numbers from the array whose sum equals `k` and remove them from the array.

Return the maximum number of operations you can perform on the array.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4], k = 5
Output: 2
Explanation: Starting with nums = [1,2,3,4]:
- Remove numbers 1 and 4, then nums = [2,3]
- Remove numbers 2 and 3, then nums = []
There are no more pairs that sum up to 5, hence a total of 2 operations.
```

Example 2:

```text
Input: nums = [3,1,3,4,3], k = 6
Output: 1
Explanation: Starting with nums = [3,1,3,4,3]:
- Remove the first two 3's, then nums = [1,4,3]
There are no more pairs that sum up to 6, hence a total of 1 operation.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` 。

每一步操作中，你需要从数组中选出和为 `k` 的两个整数，并将它们移出数组。

返回你可以对数组执行的最大操作数。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4], k = 5
输出：2
解释：开始时 nums = [1,2,3,4]：
- 移出 1 和 4 ，之后 nums = [2,3]
- 移出 2 和 3 ，之后 nums = []
不再有和为 5 的数对，因此最多执行 2 次操作。
```

示例 2：

```text
输入：nums = [3,1,3,4,3], k = 6
输出：1
解释：开始时 nums = [3,1,3,4,3]：
- 移出前两个 3 ，之后nums = [1,4,3]
不再有和为 6 的数对，因此最多执行 1 次操作。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 9`

__思路:__

```text
双指针
并不需要真的移动数组中的元素, 只需要统计移动的次数即可
先对数组进行排序
然后使用双指针, 一个指向数组的头部, 一个指向数组的尾部
如果两个指针指向的元素之和大于 k, 则尾指针向前移动
如果两个指针指向的元素之和小于 k, 则头指针向后移动
如果两个指针指向的元素之和等于 k, 则两个指针都向中间移动, 并且结果加 1
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 排序的时间复杂度为 O(NlogN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxOperations(vector<int>& nums, int k) 
    {
        sort(nums.begin(), nums.end());
        int result = 0, left = 0, right = nums.size() - 1;
        while (left < right) 
        {
            if (nums[left] + nums[right] < k) ++left;
            else if (nums[left] + nums[right] > k) --right;
            else 
            {
                ++result;
                ++left;
                --right;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxOperations(int[] nums, int k) {
        Arrays.sort(nums);
        int left = 0, right = nums.length - 1, result = 0;
        while (left < right) {
            if (nums[left] + nums[right] > k) --right;
            else if (nums[left] + nums[right] < k) ++left;
            else {
                ++result;
                ++left;
                --right;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxOperations(self, nums: List[int], k: int) -> int:
        nums.sort()
        left, right, result = 0, len(nums) - 1, 0
        while left < right:
            if nums[left] + nums[right] > k:
                right -= 1
            elif nums[left] + nums[right] < k:
                left += 1
            else:
                result += 1
                left += 1
                right -= 1
        return result
```
