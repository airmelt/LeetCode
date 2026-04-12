# 2971 Find Polygon With the Largest Perimeter 找到最大周长的多边形

__Description:__

You are given an array of __positive__ integers `nums` of length `n`.

A __polygon__ is a closed plane figure that has at least `3` sides. The __longest side__ of a polygon is __smaller__ than the sum of its other sides.

Conversely, if you have `k` ( `k >= 3`) __positive__ real numbers `a1`, `a2`, `a3`, ..., `ak` where `a1 <= a2 <= a3 <= ... <= ak` __and__ `a1 + a2 + a3 + ... + ak-1 > ak`, then there __always__ exists a polygon with `k` sides whose lengths are `a1`, `a2`, `a3`, ..., `ak`.

The perimeter of a polygon is the sum of lengths of its sides.

Return _the __largest__ possible __perimeter__ of a __polygon__ whose sides can be formed from_ `nums`, _or_ `-1` _if it is not possible to create a polygon_.

__Example:__

Example 1:

```text
Input: nums = [5,5,5]
Output: 15
Explanation: The only possible polygon that can be made from nums has 3 sides: 5, 5, and 5. The perimeter is 5 + 5 + 5 = 15.
```

Example 2:

```text
Input: nums = [1,12,1,2,5,50,3]
Output: 12
Explanation: The polygon with the largest perimeter which can be made from nums has 5 sides: 1, 1, 2, 3, and 5. The perimeter is 1 + 1 + 2 + 3 + 5 = 12.
We cannot have a polygon with either 12 or 50 as the longest side because it is not possible to include 2 or more smaller sides that have a greater sum than either of them.
It can be shown that the largest possible perimeter is 12.
```

Example 3:

```text
Input: nums = [5,5,50]
Output: -1
Explanation: There is no possible way to form a polygon from nums, as a polygon has at least 3 sides and 50 > 5 + 5.
```

__Constraints:__

- `3 <= n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个长度为 `n` 的 __正__ 整数数组 `nums` 。

__多边形__ 指的是一个至少有 `3` 条边的封闭二维图形。多边形的 __最长边__ 一定 __小于__ 所有其他边长度之和。

如果你有 `k` （ `k >= 3`）个 __正__ 数 `a1`， `a2`， `a3`, ...， `ak` 满足 `a1 <= a2 <= a3 <= ... <= ak` __且__ `a1 + a2 + a3 + ... + ak-1 > ak` ，那么 __一定__ 存在一个 `k` 条边的多边形，每条边的长度分别为 `a1` ， `a2` ， `a3` ， ...， `ak` 。

一个多边形的 周长 指的是它所有边之和。

请你返回从 `nums` 中可以构造的 __多边形__ 的 __最大周长__ 。如果不能构造出任何多边形，请你返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [5,5,5]
输出：15
解释：nums 中唯一可以构造的多边形为三角形，每条边的长度分别为 5 ，5 和 5 ，周长为 5 + 5 + 5 = 15 。
```

示例 2：

```text
输入：nums = [1,12,1,2,5,50,3]
输出：12
解释：最大周长多边形为五边形，每条边的长度分别为 1 ，1 ，2 ，3 和 5 ，周长为 1 + 1 + 2 + 3 + 5 = 12 。
我们无法构造一个包含变长为 12 或者 50 的多边形，因为其他边之和没法大于两者中的任何一个。
所以最大周长为 12 。
```

示例 3：

```text
输入：nums = [5,5,50]
输出：-1
解释：无法构造任何多边形，因为多边形至少要有 3 条边且 50 > 5 + 5 。
```

__提示：__

- `3 <= n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
前缀和
若 nums[i] 为最长边
那么 pre = nums[0] + nums[1] + ... + nums[i - 1] > nums[i]
即 pre + nums[i] > nums[i] * 2
此时 pre + nums[i] 就是最长的周长
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long largestPerimeter(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        long long result = -1LL, pre = 0LL;
        for (const auto& num : nums) if ((pre += num) > num << 1) result = pre;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long largestPerimeter(int[] nums) {
        Arrays.sort(nums);
        long result = -1L, pre = 0L;
        for (int num : nums) if ((pre += num) > num << 1) result = pre;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def largestPerimeter(self, nums: List[int]) -> int:
        return p[-1] if (p := [x + y for x, y in zip(sorted(nums), list(accumulate(sorted(nums), initial=0))) if y > x]) else -1
```
