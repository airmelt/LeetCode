# 2654 Minimum Number of Operations to Make All Array Elements Equal to 1 使数组所有元素变成 1 的最少操作次数

__Description:__

You are given a __0-indexed__ array `nums` consisiting of __positive__ integers. You can do the following operation on the array __any__ number of times:

- Select an index `i` such that `0 <= i < n - 1` and replace either of `nums[i]` or `nums[i+1]` with their gcd value.

Return _the __minimum__ number of operations to make all elements of_ `nums` _equal to_ `1`. If it is impossible, return `-1`.

The gcd of two integers is the greatest common divisor of the two integers.

__Example:__

Example 1:

```text
Input: nums = [2,6,3,4]
Output: 4
Explanation: We can do the following operations:
```

- Choose index i = 2 and replace nums[2] with gcd(3,4) = 1. Now we have nums = [2,6,1,4].
- Choose index i = 1 and replace nums[1] with gcd(6,1) = 1. Now we have nums = [2,1,1,4].
- Choose index i = 0 and replace nums[0] with gcd(2,1) = 1. Now we have nums = [1,1,1,4].
- Choose index i = 2 and replace nums[3] with gcd(1,4) = 1. Now we have nums = [1,1,1,1].

Example 2:

```text
Input: nums = [2,10,6,14]
Output: -1
Explanation: It can be shown that it is impossible to make all the elements equal to 1.
```

__Constraints:__

- `2 <= nums.length <= 50`
- `1 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个下标从 __0__ 开始的 __正__ 整数数组 `nums` 。你可以对数组执行以下操作 __任意__ 次:

- 选择一个满足 `0 <= i < n - 1` 的下标 `i` ，将 `nums[i]` 或者 `nums[i+1]` 两者之一替换成它们的最大公约数。

请你返回使数组 `nums` 中所有元素都等于 `1` 的 __最少__ 操作次数。如果无法让数组全部变成 `1` ，请你返回 `-1` 。

两个正整数的最大公约数指的是能整除这两个数的最大正整数。

__示例:__

示例 1：

```text
输入：nums = [2,6,3,4]
输出：4
解释：我们可以执行以下操作：
```

- 选择下标 i = 2 ，将 nums[2] 替换为 gcd(3,4) = 1 ，得到 nums = [2,6,1,4] 。
- 选择下标 i = 1 ，将 nums[1] 替换为 gcd(6,1) = 1 ，得到 nums = [2,1,1,4] 。
- 选择下标 i = 0 ，将 nums[0] 替换为 gcd(2,1) = 1 ，得到 nums = [1,1,1,4] 。
- 选择下标 i = 2 ，将 nums[3] 替换为 gcd(1,4) = 1 ，得到 nums = [1,1,1,1] 。

示例 2：

```text
输入：nums = [2,10,6,14]
输出：-1
解释：无法将所有元素都变成 1 。
```

__提示：__

- `2 <= nums.length <= 50`
- `1 <= nums[i] <= 10 ^ 6`

__思路:__

```text
数学
如果数组的最大公约数不为 1, 直接返回 -1
如果数组中有 1, 通过 1 可以把其他数变成 1
所以有几个 1 就可以少做几次操作, 返回 n - cnt1
否则需要找到最短的连续子数组, 使得它的最大公约数为 1
记录最短的子数组长度 mx
需要操作 mx - 1 次把得到一个 1
之后转换为有 1 的情况, 还需要 n - 1 次操作
总次数 mx + n - 2
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& nums) 
    {
        int n = nums.size(), total = 0, cnt1 = 0, mx = n;
        for (const auto& num : nums) 
        {
            total = gcd(total, num);
            if (num == 1) ++cnt1;
        }
        if (total > 1) return -1;
        if (cnt1 > 0) return n - cnt1;
        for (int i = 0; i < n; i++) 
        {
            for (int g = 0, j = i; j < n; j++) 
            {
                if ((g = gcd(g, nums[j])) == 1) 
                {
                    mx = min(mx, j - i);
                    break;
                }
            }
        }
        return mx + n - 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[] nums) {
        int n = nums.length, total = 0, cnt1 = 0, mx = n;
        for (int num : nums) {
            total = gcd(total, num);
            if (num == 1) ++cnt1;
        }
        if (total > 1) return -1;
        if (cnt1 > 0) return n - cnt1;
        for (int i = 0; i < n; i++) {
            for (int g = 0, j = i; j < n; j++) {
                if ((g = gcd(g, nums[j])) == 1) {
                    mx = Math.min(mx, j - i);
                    break;
                }
            }
        }
        return mx + n - 1;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        if gcd(*nums) > 1:
            return -1
        min_size = n = len(nums)
        if (cnt1 := sum(x == 1 for x in nums)):
            return n - cnt1
        for i in range(n):
            g = 0
            for j in range(i, n):
                if (g := gcd(g, nums[j])) == 1:
                    min_size = min(min_size, j - i)
                    break
        return min_size + n - 1
```
