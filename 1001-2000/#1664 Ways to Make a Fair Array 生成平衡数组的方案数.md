# 1664 Ways to Make a Fair Array 生成平衡数组的方案数

__Description:__

You are given an integer array `nums`. You can choose __exactly one__ index (__0-indexed__) and remove the element. Notice that the index of the elements may change after the removal.

For example, if `nums = [6,1,7,4,1]`:

- Choosing to remove index `1` results in `nums = [6,7,4,1]`.
- Choosing to remove index `2` results in `nums = [6,1,4,1]`.
- Choosing to remove index `4` results in `nums = [6,1,7,4]`.

An array is fair if the sum of the odd-indexed values equals the sum of the even-indexed values.

Return the ___number__ of indices that you could choose such that after the removal,_ `nums` _is __fair__._

__Example:__

Example 1:

```text
Input: nums = [2,1,6,4]
Output: 1
Explanation:
Remove index 0: [1,6,4] -> Even sum: 1 + 4 = 5. Odd sum: 6. Not fair.
Remove index 1: [2,6,4] -> Even sum: 2 + 4 = 6. Odd sum: 6. Fair.
Remove index 2: [2,1,4] -> Even sum: 2 + 4 = 6. Odd sum: 1. Not fair.
Remove index 3: [2,1,6] -> Even sum: 2 + 6 = 8. Odd sum: 1. Not fair.
There is 1 index that you can remove to make nums fair.
```

Example 2:

```text
Input: nums = [1,1,1]
Output: 3
Explanation: You can remove any index and the remaining array is fair.
```

Example 3:

```text
Input: nums = [1,2,3]
Output: 0
Explanation: You cannot make a fair array after removing any index.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 4`

__题目描述:__

给你一个整数数组 `nums` 。你需要选择 __恰好__ 一个下标（下标从 __0__ 开始）并删除对应的元素。请注意剩下元素的下标可能会因为删除操作而发生改变。

比方说，如果 `nums = [6,1,7,4,1]` ，那么:

- 选择删除下标 `1` ，剩下的数组为 `nums = [6,7,4,1]` 。
- 选择删除下标 `2` ，剩下的数组为 `nums = [6,1,4,1]` 。
- 选择删除下标 `4` ，剩下的数组为 `nums = [6,1,7,4]` 。

如果一个数组满足奇数下标元素的和与偶数下标元素的和相等，该数组就是一个 平衡数组 。

请你返回删除操作后，剩下的数组 `nums` 是 __平衡数组__ 的 __方案数__ 。

__示例:__

示例 1：

```text
输入：nums = [2,1,6,4]
输出：1
解释：
删除下标 0 ：[1,6,4] -> 偶数元素下标为：1 + 4 = 5 。奇数元素下标为：6 。不平衡。
删除下标 1 ：[2,6,4] -> 偶数元素下标为：2 + 4 = 6 。奇数元素下标为：6 。平衡。
删除下标 2 ：[2,1,4] -> 偶数元素下标为：2 + 4 = 6 。奇数元素下标为：1 。不平衡。
删除下标 3 ：[2,1,6] -> 偶数元素下标为：2 + 6 = 8 。奇数元素下标为：1 。不平衡。
只有一种让剩余数组成为平衡数组的方案。
```

示例 2：

```text
输入：nums = [1,1,1]
输出：3
解释：你可以删除任意元素，剩余数组都是平衡数组。
```

示例 3：

```text
输入：nums = [1,2,3]
输出：0
解释：不管删除哪个元素，剩下数组都不是平衡数组。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 4`

__思路:__

```text
模拟
先按照奇偶求出数组的平衡和
当下标为奇数时, s += nums[i]
当下标为偶数时, s -= nums[i]
这样遍历数组的每一个下标的时候, 当下标为奇数时, 删除当前的元素的方式为 s -= nums[i]
当下标为偶数时, 删除当前的元素的方式为 s += nums[i]
只要 s 为 0 说明是平衡的
然后再恢复 s 使 s 仍然是整个数组的平衡和
因为下一次遍历会改变前面已经遍历过的下标的奇偶性, 所以当当前下标为奇数时, s -= nums[i]
当当前下标为偶数时, s += nums[i]
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int waysToMakeFair(vector<int>& nums) 
    {
        int result = 0, s = 0, n = nums.size();
        for (int i = 0; i < n; i++) s += (i & 1) ? nums[i] : -nums[i];
        for (int i = 0; i < n; i++) 
        {
            s -= (i & 1) ? nums[i] : -nums[i];
            result += s == 0;
            s -= (i & 1) ? nums[i] : -nums[i];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int waysToMakeFair(int[] nums) {
        int result = 0, s = 0, n = nums.length;
        for (int i = 0; i < n; i++) s += (i & 1) == 0 ? -nums[i] : nums[i];
        for (int i = 0; i < n; i++) {
            s -= (i & 1) == 0 ? -nums[i] : nums[i];
            result += (s == 0) ? 1 : 0;
            s -= (i & 1) == 0 ? -nums[i] : nums[i];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def waysToMakeFair(self, nums: List[int]) -> int:
        result, s = 0, sum(x * (-1) ** (i + 1) for i, x in enumerate(nums))
        for i, x in enumerate(nums):
            s -= x if (i & 1) else -x
            result += s == 0
            s -= x if (i & 1) else -x
        return result
```
