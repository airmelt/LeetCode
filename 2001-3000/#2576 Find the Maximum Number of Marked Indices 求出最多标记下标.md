# 2576 Find the Maximum Number of Marked Indices 求出最多标记下标

__Description:__

You are given a __0-indexed__ integer array `nums`.

Initially, all of the indices are unmarked. You are allowed to make this operation any number of times:

- Pick two __different unmarked__ indices `i` and `j` such that `2 * nums[i] <= nums[j]`, then mark `i` and `j`.

Return _the maximum possible number of marked indices in `nums` using the above operation any number of times_.

__Example:__

Example 1:

```text
Input: nums = [3,5,2,4]
Output: 2
Explanation: In the first operation: pick i = 2 and j = 1, the operation is allowed because 2 * nums[2] <= nums[1]. Then mark index 2 and 1.
It can be shown that there's no other valid operation so the answer is 2.
```

Example 2:

```text
Input: nums = [9,2,5,4]
Output: 4
Explanation: In the first operation: pick i = 3 and j = 0, the operation is allowed because 2 * nums[3] <= nums[0]. Then mark index 3 and 0.
In the second operation: pick i = 1 and j = 2, the operation is allowed because 2 * nums[1] <= nums[2]. Then mark index 1 and 2.
Since there is no other operation, the answer is 4.
```

Example 3:

```text
Input: nums = [7,6,8]
Output: 0
Explanation: There is no valid operation to do, so the answer is 0.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。

一开始，所有下标都没有被标记。你可以执行以下操作任意次：

- 选择两个 __互不相同且未标记__ 的下标 `i` 和 `j` ，满足 `2 * nums[i] <= nums[j]` ，标记下标 `i` 和 `j` 。

请你执行上述操作任意次，返回 `nums` 中最多可以标记的下标数目。

__示例:__

示例 1：

```text
输入：nums = [3,5,2,4]
输出：2
解释：第一次操作中，选择 i = 2 和 j = 1 ，操作可以执行的原因是 2 * nums[2] <= nums[1] ，标记下标 2 和 1 。
没有其他更多可执行的操作，所以答案为 2 。
```

示例 2：

```text
输入：nums = [9,2,5,4]
输出：4
解释：第一次操作中，选择 i = 3 和 j = 0 ，操作可以执行的原因是 2 * nums[3] <= nums[0] ，标记下标 3 和 0 。
第二次操作中，选择 i = 1 和 j = 2 ，操作可以执行的原因是 2 * nums[1] <= nums[2] ，标记下标 1 和 2 。
没有其他更多可执行的操作，所以答案为 4 。
```

示例 3：

```text
输入：nums = [7,6,8]
输出：0
解释：没有任何可以执行的操作，所以答案为 0 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
排序 ➕ 贪心
排序之后
最小的数可以从数组中间开始检查是否匹配
如果匹配成功, 第二个数也只能从它的后面开始匹配才能成功
使用同向双指针, 一个指向最小的数, 一个指向数组中间的数
每次匹配成功, 都将两个指针都向后移动一位
直到遍历完数组
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxNumOfMarkedIndices(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        int result = 0, n = nums.size();
        for (int i = (n + 1) >> 1; i < n; i++) if ((nums[result] << 1) <= nums[i]) ++result;
        return result << 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxNumOfMarkedIndices(int[] nums) {
        Arrays.sort(nums);
        int result = 0, n = nums.length;
        for (int i = (n + 1) >>> 1; i < n; i++) if ((nums[result] << 1) <= nums[i]) ++result;
        return result << 1;
    }
}
```

__Python__:

```Python
class Solution:
    def maxNumOfMarkedIndices(self, nums: List[int]) -> int:
        nums.sort()
        result, n = 0, len(nums)
        for num in nums[(n + 1) >> 1:]:
            if (nums[result] << 1) <= num:
                result += 1
        return result << 1
```
