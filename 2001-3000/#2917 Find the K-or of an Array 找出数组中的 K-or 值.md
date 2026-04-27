# 2917 Find the K-or of an Array 找出数组中的 K-or 值

__Description:__

You are given an integer array `nums`, and an integer `k`. Let's introduce __K-or__ operation by extending the standard bitwise OR. In K-or, a bit position in the result is set to `1` if at least `k` numbers in `nums` have a `1` in that position.

Return _the K-or of_ `nums`.

__Example:__

Example 1:

```text
Input: nums = [7,12,9,8,9,15], k = 4

Output: 9

Explanation:

Represent numbers in binary:

Bit 0 is set in 7, 9, 9, and 15. Bit 3 is set in 12, 9, 8, 9, and 15.

Only bits 0 and 3 qualify. The result is (1001)2 = 9.
```

Example 2:

```text
Input: nums = [2,12,1,11,4,5], k = 6

Output: 0

Explanation: No bit appears as 1 in all six array numbers, as required for K-or with k = 6. Thus, the result is 0.
```

Example 3:

```text
Input: nums = [10,8,5,9,11,6,8], k = 1

Output: 15

Explanation: Since k == 1, the 1-or of the array is equal to the bitwise OR of all its elements. Hence, the answer is 10 OR 8 OR 5 OR 9 OR 11 OR 6 OR 8 = 15.
```

__Constraints:__

- `1 <= nums.length <= 50`
- `0 <= nums[i] < 2 ^ 31`
- `1 <= k <= nums.length`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` 。让我们通过扩展标准的按位或来介绍 __K-or__ 操作。在 K-or 操作中，如果在 `nums` 中，至少存在 `k` 个元素的第 `i` 位值为 1 ，那么 K-or 中的第 `i` 位的值是 1 。

返回 `nums` 的 __K-or__ 值。

__示例:__

示例 1：

```text
输入：nums = [7,12,9,8,9,15], k = 4
输出：9
解释：
用二进制表示 numbers：

位 0 在 7, 9, 9, 15 中为 1。位 3 在 12, 9, 8, 9, 15 中为 1。 只有位 0 和 3 满足。结果是 (1001)2 = 9。
```

示例 2：

```text
输入：nums = [2,12,1,11,4,5], k = 6
输出：0
解释：没有位在所有 6 个数字中都为 1，如 k = 6 所需要的。所以，答案为 0。
```

示例 3：

```text
输入：nums = [10,8,5,9,11,6,8], k = 1
输出：15
解释：因为 k == 1 ，数组的 1-or 等于其中所有元素按位或运算的结果。因此，答案为 10 OR 8 OR 5 OR 9 OR 11 OR 6 OR 8 = 15 。
```

__提示：__

- `1 <= nums.length <= 50`
- `0 <= nums[i] < 2 ^ 31`
- `1 <= k <= nums.length`

__思路:__

```text
位运算
遍历 1 - 31 位
将所有 nums 中的元素取出当前位累计
如果不小于 k
将 1 << i 累加到结果中
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findKOr(vector<int>& nums, int k) 
    {
        int result = 0;
        for (int i = 0, cur = 0; i < 31; i++, cur = 0) 
        {
            for (const auto& num : nums) cur += num >> i & 1;
            if (cur >= k) result |= 1 << i;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findKOr(int[] nums, int k) {
        int result = 0;
        for (int i = 0, cur = 0; i < 31; i++, cur = 0) {
            for (int num : nums) cur += num >> i & 1;
            if (cur >= k) result |= 1 << i;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findKOr(self, nums: List[int], k: int) -> int:
        return sum(1 << i for i in range(31) if sum(num >> i & 1 for num in nums) >= k)
```
