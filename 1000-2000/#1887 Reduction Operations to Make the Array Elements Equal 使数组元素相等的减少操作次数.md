# 1887 Reduction Operations to Make the Array Elements Equal 使数组元素相等的减少操作次数

__Description:__

Given an integer array `nums`, your goal is to make all elements in `nums` equal. To complete one operation, follow these steps:

Return _the number of operations to make all elements in_ `nums` _equal_.

__Example:__

Example 1:

```text
Input: nums = [5,1,3]
Output: 3
Explanation: It takes 3 operations to make all elements in nums equal:
1. largest = 5 at index 0. nextLargest = 3. Reduce nums[0] to 3. nums = [3,1,3].
2. largest = 3 at index 0. nextLargest = 1. Reduce nums[0] to 1. nums = [1,1,3].
3. largest = 3 at index 2. nextLargest = 1. Reduce nums[2] to 1. nums = [1,1,1].
```

Example 2:

```text
Input: nums = [1,1,1]
Output: 0
Explanation: All elements in nums are already equal.
```

Example 3:

```text
Input: nums = [1,1,2,2,3]
Output: 4
Explanation: It takes 4 operations to make all elements in nums equal:
1. largest = 3 at index 4. nextLargest = 2. Reduce nums[4] to 2. nums = [1,1,2,2,2].
2. largest = 2 at index 2. nextLargest = 1. Reduce nums[2] to 1. nums = [1,1,1,2,2].
3. largest = 2 at index 3. nextLargest = 1. Reduce nums[3] to 1. nums = [1,1,1,1,2].
4. largest = 2 at index 4. nextLargest = 1. Reduce nums[4] to 1. nums = [1,1,1,1,1].
```

__Constraints:__

- `1 <= nums.length <= 5 * 10 ^ 4`
- `1 <= nums[i] <= 5 * 10 ^ 4`

__题目描述:__

给你一个整数数组 `nums` ，你的目标是令 `nums` 中的所有元素相等。完成一次减少操作需要遵照下面的几个步骤:

返回使 `nums` 中的所有元素相等的操作次数。

__示例:__

示例 1：

```text
输入：nums = [5,1,3]
输出：3
解释：需要 3 次操作使 nums 中的所有元素相等：
1. largest = 5 下标为 0 。nextLargest = 3 。将 nums[0] 减少到 3 。nums = [3,1,3] 。
2. largest = 3 下标为 0 。nextLargest = 1 。将 nums[0] 减少到 1 。nums = [1,1,3] 。
3. largest = 3 下标为 2 。nextLargest = 1 。将 nums[2] 减少到 1 。nums = [1,1,1] 。
```

示例 2：

```text
输入：nums = [1,1,1]
输出：0
解释：nums 中的所有元素已经是相等的。
```

示例 3：

```text
输入：nums = [1,1,2,2,3]
输出：4
解释：需要 4 次操作使 nums 中的所有元素相等：
1. largest = 3 下标为 4 。nextLargest = 2 。将 nums[4] 减少到 2 。nums = [1,1,2,2,2] 。
2. largest = 2 下标为 2 。nextLargest = 1 。将 nums[2] 减少到 1 。nums = [1,1,1,2,2] 。 
3. largest = 2 下标为 3 。nextLargest = 1 。将 nums[3] 减少到 1 。nums = [1,1,1,1,2] 。 
4. largest = 2 下标为 4 。nextLargest = 1 。将 nums[4] 减少到 1 。nums = [1,1,1,1,1] 。
```

__提示：__

- `1 <= nums.length <= 5 * 10 ^ 4`
- `1 <= nums[i] <= 5 * 10 ^ 4`

__思路:__

```text

时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int reductionOperations(vector<int>& nums) 
    {
        int result = 0, n = nums.size();
        sort(nums.begin(), nums.end());
        for (int i = 1; i < n; i++) if (nums[i] != nums[i - 1]) result += n - i;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int reductionOperations(int[] nums) {
        int result = 0, n = nums.length;
        Arrays.sort(nums);
        for (int i = 1; i < n; i++) if (nums[i] != nums[i - 1]) result += n - i;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def reductionOperations(self, nums: List[int]) -> int:
        return sum(n - i for i in range(1, n) if nums[i] != nums[i - 1]) if (nums := sorted(nums)) and (n := len(nums)) else 0
```
