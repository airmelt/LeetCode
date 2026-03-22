# 1909 Remove One Element to Make the Array Strictly Increasing 删除一个元素使数组严格递增

__Description:__

Given a __0-indexed__ integer array `nums`, return `true` _if it can be made __strictly increasing__ after removing __exactly one__ element, or_ `false` _otherwise. If the array is already strictly increasing, return_ `true`.

The array `nums` is __strictly increasing__ if `nums[i - 1] < nums[i]` for each index `(1 <= i < nums.length).`

__Example:__

Example 1:

```text
Input: nums = [1,2,10,5,7]
Output: true
Explanation: By removing 10 at index 2 from nums, it becomes [1,2,5,7].
[1,2,5,7] is strictly increasing, so return true.
```

Example 2:

```text
Input: nums = [2,3,1,2]
Output: false
Explanation:
[3,1,2] is the result of removing the element at index 0.
[2,1,2] is the result of removing the element at index 1.
[2,3,2] is the result of removing the element at index 2.
[2,3,1] is the result of removing the element at index 3.
No resulting array is strictly increasing, so return false.
```

Example 3:

```text
Input: nums = [1,1,1]
Output: false
Explanation: The result of removing any element is [1,1].
[1,1] is not strictly increasing, so return false.
```

__Constraints:__

- `2 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，如果 __恰好__ 删除 __一个__ 元素后，数组 __严格递增__ ，那么请你返回 `true` ，否则返回 `false` 。如果数组本身已经是严格递增的，请你也返回 `true` 。

数组 `nums` 是 __严格递增__ 的定义为:对于任意下标的 `1 <= i < nums.length` 都满足 `nums[i - 1] < nums[i]` 。

__示例:__

示例 1：

```text
输入：nums = [1,2,10,5,7]
输出：true
解释：从 nums 中删除下标 2 处的 10 ，得到 [1,2,5,7] 。
[1,2,5,7] 是严格递增的，所以返回 true 。
```

示例 2：

```text
输入：nums = [2,3,1,2]
输出：false
解释：
[3,1,2] 是删除下标 0 处元素后得到的结果。
[2,1,2] 是删除下标 1 处元素后得到的结果。
[2,3,2] 是删除下标 2 处元素后得到的结果。
[2,3,1] 是删除下标 3 处元素后得到的结果。
没有任何结果数组是严格递增的，所以返回 false 。
```

示例 3：

```text
输入：nums = [1,1,1]
输出：false
解释：删除任意元素后的结果都是 [1,1] 。
[1,1] 不是严格递增的，所以返回 false 。
```

示例 4：

```text
输入：nums = [1,2,3]
输出：true
解释：[1,2,3] 已经是严格递增的，所以返回 true 。
```

__提示：__

- `2 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`

__思路:__

```text
1. 暴力法
对每一个下标尝试移出之后判断是否严格递增
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 贪心法
遍历数组, 如果出现 nums[i] <= nums[i - 1] 的情况, 则需要删除一个元素
如果已经出现过一次, 则返回 false
否则, 将 flag 置为 true, 并且判断是否需要将 nums[i] 赋值为 nums[i - 1], 如果当前元素比前两个元素还小, 可以将后一个元素直接赋值给当前元素
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canBeIncreasing(vector<int>& nums) 
    {
        bool flag = false;
        for (int i = 1, n = nums.size(); i < n; i++)
        {
            if (nums[i] <= nums[i - 1])
            {
                if (flag) return !flag;
                flag = !flag;
                if (i > 1 and nums[i] <= nums[i - 2]) nums[i] = nums[i - 1];
            }
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canBeIncreasing(int[] nums) {
        boolean flag = false;
        for (int i = 1, n = nums.length; i < n; i++) {
            if (nums[i] <= nums[i - 1]) {
                if (flag) return !flag;
                flag = !flag;
                if (i > 1 && nums[i] <= nums[i - 2]) nums[i] = nums[i - 1];
            }
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def canBeIncreasing(self, nums: List[int]) -> bool:
        return any(check(nums[:i] + nums[i + 1:]) for i in range(len(nums))) if (check := lambda a: all(a[j] < a[j + 1] for j in range(len(a) - 1))) else False
```
