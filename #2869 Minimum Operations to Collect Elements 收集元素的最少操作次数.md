# 2869 Minimum Operations to Collect Elements 收集元素的最少操作次数

__Description:__

You are given an array `nums` of positive integers and an integer `k`.

In one operation, you can remove the last element of the array and add it to your collection.

Return _the __minimum number of operations__ needed to collect elements_ `1, 2, ..., k`.

__Example:__

Example 1:

```text
Input: nums = [3,1,5,4,2], k = 2
Output: 4
Explanation: After 4 operations, we collect elements 2, 4, 5, and 1, in this order. Our collection contains elements 1 and 2. Hence, the answer is 4.
```

Example 2:

```text
Input: nums = [3,1,5,4,2], k = 5
Output: 5
Explanation: After 5 operations, we collect elements 2, 4, 5, 1, and 3, in this order. Our collection contains elements 1 through 5. Hence, the answer is 5.
```

Example 3:

```text
Input: nums = [3,2,5,3,1], k = 3
Output: 4
Explanation: After 4 operations, we collect elements 1, 3, 5, and 2, in this order. Our collection contains elements 1 through 3. Hence, the answer is 4.
```

__Constraints:__

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= nums.length`
- `1 <= k <= nums.length`
- The input is generated such that you can collect elements `1, 2, ..., k`.

__题目描述:__

给你一个正整数数组 `nums` 和一个整数 `k` 。

一次操作中，你可以将数组的最后一个元素删除，将该元素添加到一个集合中。

请你返回收集元素 `1, 2, ..., k` 需要的 __最少操作次数__ 。

__示例:__

示例 1：

```text
输入：nums = [3,1,5,4,2], k = 2
输出：4
解释：4 次操作后，集合中的元素依次添加了 2 ，4 ，5 和 1 。此时集合中包含元素 1 和 2 ，所以答案为 4 。
```

示例 2：

```text
输入：nums = [3,1,5,4,2], k = 5
输出：5
解释：5 次操作后，集合中的元素依次添加了 2 ，4 ，5 ，1 和 3 。此时集合中包含元素 1 到 5 ，所以答案为 5 。
```

示例 3：

```text
输入：nums = [3,2,5,3,1], k = 3
输出：4
解释：4 次操作后，集合中的元素依次添加了 1 ，3 ，5 和 2 。此时集合中包含元素 1 到 3  ，所以答案为 4 。
```

__提示：__

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= nums.length`
- `1 <= k <= nums.length`
- 输入保证你可以收集到元素 `1, 2, ..., k` 。

__思路:__

```text
模拟
从后往前遍历
由于数组中的元素不大于 50 所以可以用一个 long 型二进制记录是否出现
如果最后等于 k 对应的全 1 的二进制数就返回 n - i
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& nums, int k) 
    {
        long long target = (2LL << k) - 2LL, s = 0LL;
        for (int n = nums.size(), i = n - 1; ; --i) if (((s |= 1LL << nums[i]) & target) == target) return n - i;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(List<Integer> nums, int k) {
        long target = (2L << k) - 2L, s = 0L;
        for (int n = nums.size(), i = n - 1; ; --i) if (((s |= 1L << nums.get(i)) & target) == target) return n - i;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums: List[int], k: int) -> int:
        return max(nums[::-1].index(i) + 1 for i in range(1, k + 1))
```
