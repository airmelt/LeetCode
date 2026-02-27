# 2918 Minimum Equal Sum of Two Arrays After Replacing Zeros 数组的最小相等和

__Description:__

You are given two arrays `nums1` and `nums2` consisting of positive integers.

You have to replace __all__ the `0`'s in both arrays with __strictly__ positive integers such that the sum of elements of both arrays becomes __equal__.

Return _the __minimum__ equal sum you can obtain, or_ `-1` _if it is impossible_.

__Example:__

Example 1:

```text
Input: nums1 = [3,2,0,1,0], nums2 = [6,5,0]
Output: 12
Explanation: We can replace 0's in the following way:
```

- Replace the two 0's in nums1 with the values 2 and 4. The resulting array is nums1 = [3,2,2,1,4].
- Replace the 0 in nums2 with the value 1. The resulting array is nums2 = [6,5,1].

Both arrays have an equal sum of 12. It can be shown that it is the minimum sum we can obtain.

Example 2:

```text
Input: nums1 = [2,0,2,0], nums2 = [1,4]
Output: -1
Explanation: It is impossible to make the sum of both arrays equal.
```

__Constraints:__

- `1 <= nums1.length, nums2.length <= 10 ^ 5`
- `0 <= nums1[i], nums2[i] <= 10 ^ 6`

__题目描述:__

给你两个由正整数和 `0` 组成的数组 `nums1` 和 `nums2` 。

你必须将两个数组中的 __所有__ `0` 替换为 __严格__ 正整数，并且满足两个数组中所有元素的和 __相等__ 。

返回 __最小__ 相等和 ，如果无法使两数组相等，则返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums1 = [3,2,0,1,0], nums2 = [6,5,0]
输出：12
解释：可以按下述方式替换数组中的 0 ：
```

- 用 2 和 4 替换 nums1 中的两个 0 。得到 nums1 = [3,2,2,1,4] 。
- 用 1 替换 nums2 中的一个 0 。得到 nums2 = [6,5,1] 。

两个数组的元素和相等，都等于 12 。可以证明这是可以获得的最小相等和。

示例 2：

```text
输入：nums1 = [2,0,2,0], nums2 = [1,4]
输出：-1
解释：无法使两个数组的和相等。
```

__提示：__

- `1 <= nums1.length, nums2.length <= 10 ^ 5`
- `0 <= nums1[i], nums2[i] <= 10 ^ 6`

__思路:__

```text
贪心
先用 1 替换所有的 0
得到 nums1 和 nums2 的和为 s1/s2
计算 nums1 和 nums2 的 0 的个数为 c1/c2
如果 s1 > s2, 且 c2 为 0, 那么无论如何也不能相等
反之也是
否则返回 s1 和 s2 较大值即可
这时可以把较小的数组中的 0 改为两者的差值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minSum(vector<int>& nums1, vector<int>& nums2) 
    {
        return ((accumulate(nums1.begin(), nums1.end(), 0LL) + count_if(nums1.begin(), nums1.end(), [](auto x) { return !x; })) > (accumulate(nums2.begin(), nums2.end(), 0LL) + count_if(nums2.begin(), nums2.end(), [](auto x) { return !x; })) and !count_if(nums2.begin(), nums2.end(), [](auto x) { return !x; }) or (accumulate(nums1.begin(), nums1.end(), 0LL) + count_if(nums1.begin(), nums1.end(), [](auto x) { return !x; })) < (accumulate(nums2.begin(), nums2.end(), 0LL) + count_if(nums2.begin(), nums2.end(), [](auto x) { return !x; })) and !count_if(nums1.begin(), nums1.end(), [](auto x) { return !x; })) ? -1 : max(accumulate(nums1.begin(), nums1.end(), 0LL) + count_if(nums1.begin(), nums1.end(), [](auto x) { return !x; }), accumulate(nums2.begin(), nums2.end(), 0LL) + count_if(nums2.begin(), nums2.end(), [](auto x) { return !x; }));
    }
};
```

__Java__:

```Java
class Solution {
    public long minSum(int[] nums1, int[] nums2) {
        return (Arrays.stream(nums1).mapToLong(x -> x).sum() + Arrays.stream(nums1).filter(x -> x == 0).count()) > (Arrays.stream(nums2).mapToLong(x -> x).sum() + Arrays.stream(nums2).filter(x -> x == 0).count()) && Arrays.stream(nums2).filter(x -> x == 0).count() == 0L || (Arrays.stream(nums1).mapToLong(x -> x).sum() + Arrays.stream(nums1).filter(x -> x == 0).count()) < (Arrays.stream(nums2).mapToLong(x -> x).sum() + Arrays.stream(nums2).filter(x -> x == 0).count()) && Arrays.stream(nums1).filter(x -> x == 0).count() == 0L ? -1 : Math.max(Arrays.stream(nums1).mapToLong(x -> x).sum() + Arrays.stream(nums1).filter(x -> x == 0).count(), Arrays.stream(nums2).mapToLong(x -> x).sum() + Arrays.stream(nums2).filter(x -> x == 0).count());
    }
}
```

__Python__:

```Python
class Solution:
    def minSum(self, nums1: List[int], nums2: List[int]) -> int:
        return -1 if (s1 := sum(max(x, 1) for x in nums1)) < (s2 := sum(max(x, 1) for x in nums2)) and 0 not in nums1 or s2 < s1 and 0 not in nums2 else max(s1, s2)
```
