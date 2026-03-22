# 2256 Minimum Average Difference 最小平均差

__Description:__

You are given a __0-indexed__ integer array `nums` of length `n`.

The __average difference__ of the index `i` is the __absolute__ __difference__ between the average of the __first__ `i + 1` elements of `nums` and the average of the __last__ `n - i - 1` elements. Both averages should be __rounded down__ to the nearest integer.

Return the index with the minimum average difference. If there are multiple such indices, return the smallest one.

Note:

- The __absolute difference__ of two numbers is the absolute value of their difference.
- The __average__ of `n` elements is the __sum__ of the `n` elements divided (__integer division__) by `n`.
- The average of `0` elements is considered to be `0`.

__Example:__

Example 1:

```text
Input: nums = [2,5,3,9,5,3]
Output: 3
Explanation:
```

- The average difference of index 0 is: |2 / 1 - (5 + 3 + 9 + 5 + 3) / 5| = |2 / 1 - 25 / 5| = |2 - 5| = 3.
- The average difference of index 1 is: |(2 + 5) / 2 - (3 + 9 + 5 + 3) / 4| = |7 / 2 - 20 / 4| = |3 - 5| = 2.
- The average difference of index 2 is: |(2 + 5 + 3) / 3 - (9 + 5 + 3) / 3| = |10 / 3 - 17 / 3| = |3 - 5| = 2.
- The average difference of index 3 is: |(2 + 5 + 3 + 9) / 4 - (5 + 3) / 2| = |19 / 4 - 8 / 2| = |4 - 4| = 0.
- The average difference of index 4 is: |(2 + 5 + 3 + 9 + 5) / 5 - 3 / 1| = |24 / 5 - 3 / 1| = |4 - 3| = 1.
- The average difference of index 5 is: |(2 + 5 + 3 + 9 + 5 + 3) / 6 - 0| = |27 / 6 - 0| = |4 - 0| = 4.

The average difference of index 3 is the minimum average difference so return 3.

Example 2:

```text
Input: nums = [0]
Output: 0
Explanation:
The only index is 0 so return 0.
The average difference of index 0 is: |0 / 1 - 0| = |0 - 0| = 0.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始长度为 `n` 的整数数组 `nums` 。

下标 `i` 处的 __平均差__ 指的是 `nums` 中 __前__ `i + 1` 个元素平均值和 __后__ `n - i - 1` 个元素平均值的 __绝对差__ 。两个平均值都需要 __向下取整__ 到最近的整数。

请你返回产生 最小平均差 的下标。如果有多个下标最小平均差相等，请你返回 最小 的一个下标。

注意：

- 两个数的 __绝对差__ 是两者差的绝对值。
- `n` 个元素的平均值是 `n` 个元素之 __和__ 除以（整数除法） `n` 。
- `0` 个元素的平均值视为 `0` 。

__示例:__

示例 1：

```text
输入：nums = [2,5,3,9,5,3]
输出：3
解释：
```

- 下标 0 处的平均差为：|2 / 1 - (5 + 3 + 9 + 5 + 3) / 5| = |2 / 1 - 25 / 5| = |2 - 5| = 3 。
- 下标 1 处的平均差为：|(2 + 5) / 2 - (3 + 9 + 5 + 3) / 4| = |7 / 2 - 20 / 4| = |3 - 5| = 2 。
- 下标 2 处的平均差为：|(2 + 5 + 3) / 3 - (9 + 5 + 3) / 3| = |10 / 3 - 17 / 3| = |3 - 5| = 2 。
- 下标 3 处的平均差为：|(2 + 5 + 3 + 9) / 4 - (5 + 3) / 2| = |19 / 4 - 8 / 2| = |4 - 4| = 0 。
- 下标 4 处的平均差为：|(2 + 5 + 3 + 9 + 5) / 5 - 3 / 1| = |24 / 5 - 3 / 1| = |4 - 3| = 1 。
- 下标 5 处的平均差为：|(2 + 5 + 3 + 9 + 5 + 3) / 6 - 0| = |27 / 6 - 0| = |4 - 0| = 4 。

下标 3 处的平均差为最小平均差，所以返回 3 。

示例 2：

```text
输入：nums = [0]
输出：0
解释：
唯一的下标是 0 ，所以我们返回 0 。
下标 0 处的平均差为：|0 / 1 - 0| = |0 - 0| = 0 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 5`

__思路:__

```text
前缀和
计算出数组的和
则 i 处的平均差为 abs(pre / (i + 1) - (total - pre) / (n - i - 1))
需要额外处理 i == n - 1 的情况
注意数组和可能会超过 int 的范围, 所以需要使用 long long
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumAverageDifference(vector<int>& nums) 
    {
        long long total = accumulate(nums.begin(), nums.end(), 0LL), pre = 0LL, result = -1LL, cur = INT_MAX, diff = 0LL;
        for (int i = 0, n = nums.size(); i < n; i++) 
        {
            pre += nums[i];
            if (i == n - 1) diff = pre / n;
            else diff = abs(pre / (i + 1) - (total - pre) / (n - i - 1));
            if (cur > diff) 
            {
                cur = diff;
                result = i;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumAverageDifference(int[] nums) {
        long total = Arrays.stream(nums).mapToLong(i -> i).sum(), pre = 0L, result = -1L, cur = Integer.MAX_VALUE, diff = 0L;
        for (int i = 0, n = nums.length; i < n; i++) {
            pre += nums[i];
            if (i == n - 1) diff = pre / n;
            else diff = Math.abs(pre / (i + 1) - (total - pre) / (n - i - 1));
            if (cur > diff) {
                cur = diff;
                result = i;
            }
        }
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumAverageDifference(self, nums: List[int]) -> int:
        n, total, pre, result = len(nums), sum(nums), 0, (inf, -1)
        for i in range(n):
            pre += nums[i]
            if (diff := (pre // n if i == n - 1 else abs(pre // (i + 1) - (total - pre) // (n - i - 1)))) < result[0]:
                result = (diff, i)
        return result[1]
```
