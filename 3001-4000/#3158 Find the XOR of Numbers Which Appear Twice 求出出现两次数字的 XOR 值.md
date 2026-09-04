# 3158 Find the XOR of Numbers Which Appear Twice 求出出现两次数字的 XOR 值

__Description:__

You are given an array `nums`, where each number in the array appears __either__ once or twice.

Return the bitwise `XOR` of all the numbers that appear twice in the array, or 0 if no number appears twice.

__Example:__

Example 1:

```text
Input: nums = [1,2,1,3]

Output: 1

Explanation:

The only number that appears twice in nums is 1.
```

Example 2:

```text
Input: nums = [1,2,3]

Output: 0

Explanation:

No number appears twice in nums.
```

Example 3:

```text
Input: nums = [1,2,2,1]

Output: 3

Explanation:

Numbers 1 and 2 appeared twice. 1 XOR 2 == 3.
```

__Constraints:__

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= 50`
- Each number in `nums` appears either once or twice.

__题目描述:__

给你一个数组 `nums` ，数组中的数字 __要么__ 出现一次，要么 出现两次。

请你返回数组中所有出现两次数字的按位 `XOR` 值，如果没有数字出现过两次，返回 0 。

__示例:__

示例 1：

```text
输入：nums = [1,2,1,3]

输出：1

解释：

nums 中唯一出现过两次的数字是 1 。
```

示例 2：

```text
输入：nums = [1,2,3]

输出：0

解释：

nums 中没有数字出现两次。
```

示例 3：

```text
输入：nums = [1,2,2,1]

输出：3

解释：

数字 1 和 2 出现过两次。1 XOR 2 == 3 。
```

__提示：__

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= 50`
- `nums` 中每个数字要么出现过一次，要么出现过两次。

__思路:__

```text
1. 哈希表
将 nums 转化为哈希表
然后分别求哈希表和 nums 的所有元素的异或
再取异或
这样出现 1 次的元素由于 a ^ a = 0, 就会互相抵消
留下的元素就是出现 2 次的元素
时间复杂度为 O(N), 空间复杂度为 O(N)
2. 遍历
一遍遍历一遍将元素放入一个整数中
如果出现过的数字
就异或进结果
否则记录第一次出现的数字
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int duplicateNumbersXOR(vector<int>& nums) 
    {
        long long visited = 0LL, result = 0LL;
        for (const auto& num : nums) 
        {
            if (visited >> num & 1LL) result ^= num;
            else visited |= 1LL << num;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int duplicateNumbersXOR(int[] nums) {
        long visited = 0L, result = 0L;
        for (int num : nums) {
            if ((visited >> num & 1L) > 0L) result ^= num;
            else visited |= 1L << num;
        }
        return (int) result;
    }
}
```

__Python__:

```Python
class Solution:
    def duplicateNumbersXOR(self, nums: List[int]) -> int:
        return reduce(xor, nums) ^ reduce(xor, set(nums))
```
