# 2455 Average Value of Even Numbers That Are Divisible by Three 可被三整除的偶数的平均值

__Description:__

Given an integer array `nums` of __positive__ integers, return _the average value of all even integers that are divisible by_ `3`_._

Note that the __average__ of `n` elements is the __sum__ of the `n` elements divided by `n` and __rounded down__ to the nearest integer.

__Example:__

Example 1:

```text
Input: nums = [1,3,6,10,12,15]
Output: 9
Explanation: 6 and 12 are even numbers that are divisible by 3. (6 + 12) / 2 = 9.
```

Example 2:

```text
Input: nums = [1,2,4,7,10]
Output: 0
Explanation: There is no single number that satisfies the requirement, so return 0.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`

__题目描述:__

给你一个由正整数组成的整数数组 `nums` ，返回其中可被 `3` 整除的所有偶数的平均值。

注意: `n` 个元素的平均值等于 `n` 个元素 __求和__ 再除以 `n` ，结果 __向下取整__ 到最接近的整数。

__示例:__

示例 1：

```text
输入：nums = [1,3,6,10,12,15]
输出：9
解释：6 和 12 是可以被 3 整除的偶数。(6 + 12) / 2 = 9 。
```

示例 2：

```text
输入：nums = [1,2,4,7,10]
输出：0
解释：不存在满足题目要求的整数，所以返回 0 。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`

__思路:__

```text
模拟
统计 nums 中整除 6 的数字的个数和总和
如果没有整除 6 的数字，返回 0
否则返回总和除以个数
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int averageValue(vector<int>& nums) 
    {
        int result = 0, s = 0;
        for (const auto& i : nums) 
        {
            if (!(i % 6))
            {
                ++s;
                result += i;
            }
        }
        return !s ? s : result / s;
    }
};
```

__Java__:

```Java
class Solution {
    public int averageValue(int[] nums) {
        int result = 0, s = 0;
        for (int i : nums) {
            if (i % 6 == 0) {
                ++s;
                result += i;
            }
        }
        return s == 0 ? s : result / s;
    }
}
```

__Python__:

```Python
class Solution:
    def averageValue(self, nums: List[int]) -> int:
        return int(sum(i for i in nums if not i % 6) / sum(1 for i in nums if not i % 6)) if any(not i % 6 for i in nums) else 0
```
