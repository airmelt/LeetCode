# 2717 Semi-Ordered Permutation 半有序排列

__Description:__

You are given a __0-indexed__ permutation of `n` integers `nums`.

A permutation is called __semi-ordered__ if the first number equals `1` and the last number equals `n`. You can perform the below operation as many times as you want until you make `nums` a __semi-ordered__ permutation:

- Pick two adjacent elements in `nums`, then swap them.

Return _the minimum number of operations to make_ `nums` _a __semi-ordered permutation___.

A __permutation__ is a sequence of integers from `1` to `n` of length `n` containing each number exactly once.

__Example:__

Example 1:

```text
Input: nums = [2,1,4,3]
Output: 2
Explanation: We can make the permutation semi-ordered using these sequence of operations: 
1 - swap i = 0 and j = 1. The permutation becomes [1,2,4,3].
2 - swap i = 2 and j = 3. The permutation becomes [1,2,3,4].
It can be proved that there is no sequence of less than two operations that make nums a semi-ordered permutation.
```

Example 2:

```text
Input: nums = [2,4,1,3]
Output: 3
Explanation: We can make the permutation semi-ordered using these sequence of operations:
1 - swap i = 1 and j = 2. The permutation becomes [2,1,4,3].
2 - swap i = 0 and j = 1. The permutation becomes [1,2,4,3].
3 - swap i = 2 and j = 3. The permutation becomes [1,2,3,4].
It can be proved that there is no sequence of less than three operations that make nums a semi-ordered permutation.
```

Example 3:

```text
Input: nums = [1,3,4,2,5]
Output: 0
Explanation: The permutation is already a semi-ordered permutation.
```

__Constraints:__

- `2 <= nums.length == n <= 50`
- `1 <= nums[i] <= 50`
- `nums is a permutation.`

__题目描述:__

给你一个下标从 __0__ 开始、长度为 `n` 的整数排列 `nums` 。

如果排列的第一个数字等于 `1` 且最后一个数字等于 `n` ，则称其为 __半有序排列__ 。你可以执行多次下述操作，直到将 `nums` 变成一个 __半有序排列__ :

- 选择 `nums` 中相邻的两个元素，然后交换它们。

返回使 `nums` 变成 __半有序排列__ 所需的最小操作次数。

__排列__ 是一个长度为 `n` 的整数序列，其中包含从 `1` 到 `n` 的每个数字恰好一次。

__示例:__

示例 1：

```text
输入：nums = [2,1,4,3]
输出：2
解释：可以依次执行下述操作得到半有序排列：
1 - 交换下标 0 和下标 1 对应元素。排列变为 [1,2,4,3] 。
2 - 交换下标 2 和下标 3 对应元素。排列变为 [1,2,3,4] 。
可以证明，要让 nums 成为半有序排列，不存在执行操作少于 2 次的方案。
```

示例 2：

```text
输入：nums = [2,4,1,3]
输出：3
解释：
可以依次执行下述操作得到半有序排列：
1 - 交换下标 1 和下标 2 对应元素。排列变为 [2,1,4,3] 。
2 - 交换下标 0 和下标 1 对应元素。排列变为 [1,2,4,3] 。
3 - 交换下标 2 和下标 3 对应元素。排列变为 [1,2,3,4] 。
可以证明，要让 nums 成为半有序排列，不存在执行操作少于 3 次的方案。
```

示例 3：

```text
输入：nums = [1,3,4,2,5]
输出：0
解释：这个排列已经是一个半有序排列，无需执行任何操作。
```

__提示：__

- `2 <= nums.length == n <= 50`
- `1 <= nums[i] <= 50`
- `nums` 是一个 __排列__

__思路:__

```text
模拟
先找到 1 的下标 p 和 n 的下标 q
将 1 移动到开头需要 p 次交换
将 n 移动到结尾需要 n - q - 1 次
如果 p > q 即 1 在 n 的后面
上面的操作 1 和 n 会交换一次需要减去这一次
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int semiOrderedPermutation(vector<int>& nums) 
    {
        int n = nums.size(), p = 0, q = 0;
        for (int i = 0; i < n; i++) 
        {
            if (nums[i] == 1) p = i;
            else if (nums[i] == n) q = i;
        }
        return p - q + n - 1 - (p > q);
    }
};
```

__Java__:

```Java
class Solution {
    public int semiOrderedPermutation(int[] nums) {
        int n = nums.length, p = 0, q = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] == 1) p = i;
            else if (nums[i] == n) q = i;
        }
        return p - q + n - 1 - (p > q ? 1 : 0);
    }
}
```

__Python__:

```Python
class Solution:
    def semiOrderedPermutation(self, nums: List[int]) -> int:
        return (p := nums.index(1)) + (n := len(nums)) - 1 - (q := nums.index(n)) - (p > q)
```
