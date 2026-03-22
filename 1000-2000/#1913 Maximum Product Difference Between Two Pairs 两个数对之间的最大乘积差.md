# 1913 Maximum Product Difference Between Two Pairs 两个数对之间的最大乘积差

__Description:__

The __product difference__ between two pairs `(a, b)` and `(c, d)` is defined as `(a * b) - (c * d)`.

- For example, the product difference between `(5, 6)` and `(2, 7)` is `(5 * 6) - (2 * 7) = 16`.

Given an integer array `nums`, choose four __distinct__ indices `w`, `x`, `y`, and `z` such that the __product difference__ between pairs `(nums[w], nums[x])` and `(nums[y], nums[z])` is __maximized__.

Return the maximum such product difference.

__Example:__

Example 1:

```text
Input: nums = [5,6,2,7,4]
Output: 34
Explanation: We can choose indices 1 and 3 for the first pair (6, 7) and indices 2 and 4 for the second pair (2, 4).
The product difference is (6 * 7) - (2 * 4) = 34.
```

Example 2:

```text
Input: nums = [4,2,5,9,7,4,8]
Output: 64
Explanation: We can choose indices 3 and 6 for the first pair (9, 8) and indices 1 and 5 for the second pair (2, 4).
The product difference is (9 * 8) - (2 * 4) = 64.
```

__Constraints:__

- `4 <= nums.length <= 10 ^ 4`
- `1 <= nums[i] <= 10 ^ 4`

__题目描述:__

两个数对 `(a, b)` 和 `(c, d)` 之间的 __乘积差__ 定义为 `(a * b) - (c * d)` 。

- 例如， `(5, 6)` 和 `(2, 7)` 之间的乘积差是 `(5 * 6) - (2 * 7) = 16` 。

给你一个整数数组 `nums` ，选出四个 __不同的__ 下标 `w`、 `x`、 `y` 和 `z` ，使数对 `(nums[w], nums[x])` 和 `(nums[y], nums[z])` 之间的 __乘积差__ 取到 __最大值__ 。

返回以这种方式取得的乘积差中的 最大值 。

__示例:__

示例 1：

```text
输入：nums = [5,6,2,7,4]
输出：34
解释：可以选出下标为 1 和 3 的元素构成第一个数对 (6, 7) 以及下标 2 和 4 构成第二个数对 (2, 4)
乘积差是 (6 * 7) - (2 * 4) = 34
```

示例 2：

```text
输入：nums = [4,2,5,9,7,4,8]
输出：64
解释：可以选出下标为 3 和 6 的元素构成第一个数对 (9, 8) 以及下标 1 和 5 构成第二个数对 (2, 4)
乘积差是 (9 * 8) - (2 * 4) = 64
```

__提示：__

- `4 <= nums.length <= 10 ^ 4`
- `1 <= nums[i] <= 10 ^ 4`

__思路:__

```text
贪心
排序
返回最大的两个数的乘积减去最小的两个数的乘积
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxProductDifference(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        return nums[nums.size() - 1] * nums[nums.size() - 2] - nums[0] * nums[1];
    }
};
```

__Java__:

```Java
class Solution {
    public int maxProductDifference(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length - 1] * nums[nums.length - 2] - nums[0] * nums[1];
    }
}
```

__Python__:

```Python
class Solution:
    def maxProductDifference(self, nums: List[int]) -> int:
        return sorted(nums)[-1] * sorted(nums)[-2] - sorted(nums)[0] * sorted(nums)[1]
```
