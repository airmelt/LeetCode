# 2778 Sum of Squares of Special Elements 特殊元素平方和

__Description:__

You are given a __1-indexed__ integer array `nums` of length `n`.

An element `nums[i]` of `nums` is called __special__ if `i` divides `n`, i.e. `n % i == 0`.

Return _the __sum of the squares__ of all __special__ elements of_ `nums`.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4]
Output: 21
Explanation: There are exactly 3 special elements in nums: nums[1] since 1 divides 4, nums[2] since 2 divides 4, and nums[4] since 4 divides 4. 
Hence, the sum of the squares of all special elements of nums is nums[1] * nums[1] + nums[2] * nums[2] + nums[4] * nums[4] = 1 * 1 + 2 * 2 + 4 * 4 = 21.
```

Example 2:

```text
Input: nums = [2,7,1,19,18,3]
Output: 63
Explanation: There are exactly 4 special elements in nums: nums[1] since 1 divides 6, nums[2] since 2 divides 6, nums[3] since 3 divides 6, and nums[6] since 6 divides 6. 
Hence, the sum of the squares of all special elements of nums is nums[1] * nums[1] + nums[2] * nums[2] + nums[3] * nums[3] + nums[6] * nums[6] = 2 * 2 + 7 * 7 + 1 * 1 + 3 * 3 = 63.
```

__Constraints:__

- `1 <= nums.length == n <= 50`
- `1 <= nums[i] <= 50`

__题目描述:__

给你一个下标从 __1__ 开始、长度为 `n` 的整数数组 `nums` 。

对 `nums` 中的元素 `nums[i]` 而言，如果 `n` 能够被 `i` 整除，即 `n % i == 0` ，则认为 `num[i]` 是一个 __特殊元素__ 。

返回 `nums` 中所有 __特殊元素__ 的 __平方和__ 。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4]
输出：21
解释：nums 中共有 3 个特殊元素：nums[1]，因为 4 被 1 整除；nums[2]，因为 4 被 2 整除；以及 nums[4]，因为 4 被 4 整除。 
因此，nums 中所有特殊元素的平方和等于 nums[1] * nums[1] + nums[2] * nums[2] + nums[4] * nums[4] = 1 * 1 + 2 * 2 + 4 * 4 = 21 。
```

示例 2：

```text
输入：nums = [2,7,1,19,18,3]
输出：63
解释：nums 中共有 4 个特殊元素：nums[1]，因为 6 被 1 整除；nums[2] ，因为 6 被 2 整除；nums[3]，因为 6 被 3 整除；以及 nums[6]，因为 6 被 6 整除。 
因此，nums 中所有特殊元素的平方和等于 nums[1] * nums[1] + nums[2] * nums[2] + nums[3] * nums[3] + nums[6] * nums[6] = 2 * 2 + 7 * 7 + 1 * 1 + 3 * 3 = 63 。
```

__提示：__

- `1 <= nums.length == n <= 50`
- `1 <= nums[i] <= 50`

__思路:__

```text
1. 暴力法(Python)
遍历数组
找到满足 n % (i + 1) == 0 的下标 i
累加 num ** 2
时间复杂度为 O(N), 空间复杂度为 O(1)
2. 数学
由于 i 满足 n % i == 0 时, n / i 也一定满足
所以只用计算 i * i <= n 的情况
计算 i 的同时也计算 n / i, 注意要去掉平方项
时间复杂度为 O(N ^ 1 / 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumOfSquares(vector<int>& nums) 
    {
        int result = 0, n = nums.size();
        for (int i = 1; i * i <= n; i++) 
        {
            if (!(n % i))
            {
                result += nums[i - 1] * nums[i - 1];
                if (i * i < n) result += nums[n / i - 1] * nums[n / i - 1];
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumOfSquares(int[] nums) {
        int result = 0, n = nums.length;
        for (int i = 1; i * i <= n; i++) {
            if (n % i == 0) {
                result += nums[i - 1] * nums[i - 1];
                if (i * i < n) result += nums[n / i - 1] * nums[n / i - 1];
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def sumOfSquares(self, nums: List[int]) -> int:
        return sum(num ** 2 for i, num in enumerate(nums) if not len(nums) % (i + 1))
```
