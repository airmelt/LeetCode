# 4 Median of Two Sorted Arrays 寻找两个有序数组的中位数

__Description__:
There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume nums1 and nums2 cannot be both empty.

__Example:__

Example 1:

```text
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

Example 2:

```text
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

__题目描述__:
给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

__示例 :__

示例 1:

```text
nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
```

示例 2:

```text
nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
```

__思路__:

```text
二分
将两个数组中的元素均匀分成两个组
比如
nums1 = [1, 3, 5, 7, 9]
nums2 = [0, 2, 4, 6, 8]
假定将 nums1, 3 之前的数分给第一组
由于是均匀分组
nums2 中的前 3 个数将会自动分给第一组
此时第一组中最大的数是 max(3, 4) = 4
而第二组中最小的数是 min(5, 6) = 5
此时 4 <= 5, 这个分组就是恰好合法的, 中位数为 (4 + 5) / 2.0
所以从 (-1, m) 上找到一个分界点, 使得 nums1[mid] <= nums2[((m + n - 3) >> 1) - mid] 最大
((m + n - 3) >> 1) - mid 是因为每个组需要选择 (m + n) >> 1 个数
如果 nums1 选 mid 个数, 还需要 ((m + n) >> 1) - mid 个数
当为奇数时, 将多的那个数放入第一组
那么实际上是 ((m + n + 1) >> 1) - mid
为了保证这两个数不越界可以都加上 1
mid + 1 and ((m + n + 1) >> 1) - (mid + 1)
移动 1 到右边可以得到
mid, ((m + n - 3) >> 1) - mid
比较这两个位置的大小选最大值
最后找到分界点之后
取 mid1 = left
那么 mid2 = ((m + n - 3) >> 1) - mid1
a1 = nums1[mid1], 非法时取 -inf
b1 = nums2[mid2], 非法时取 -inf
a2 = nums1[mid1 + 1], 非法时取 inf
b2 = nums2[mid2 + 1], 非法时取 inf
max(a1, b1) 即第一组最大值
min(a2, b2) 即第二组最小值
按照奇偶性返回结果即可
时间复杂度O(log(m + n)), 空间复杂度O(1)
```

__代码__:
__C++__:

```C++
class Solution 
{
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) 
    {
        if (nums1.size() > nums2.size()) return findMedianSortedArrays(nums2, nums1);
        int m = nums1.size(), n = nums2.size(), left = -1, right = m, mid = 0, inf = 0x3f3f3f3f;
        while (left + 1 < right) 
        {
            if (nums1[mid = left + ((right - left) >> 1)] <= nums2[((m + n - 3) >> 1) - mid + 1]) left = mid;
            else right = mid;
        }
        return ((m + n) & 1) == 1 ? max(left > -1 ? nums1[left] : -inf, (mid = ((m + n - 3) >> 1) - left) > -1 ? nums2[mid] : -inf) : (max(left > -1 ? nums1[left] : -inf, (mid = ((m + n - 3) >> 1) - left) > -1 ? nums2[mid] : -inf) + min(left + 1 < m ? nums1[left + 1] : inf, mid + 1 < n ? nums2[mid + 1] : inf)) / 2.0;
    }
};
```

__Java__:

```Java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if (nums1.length > nums2.length) return findMedianSortedArrays(nums2, nums1);
        int m = nums1.length, n = nums2.length, left = -1, right = m, mid = 0, inf = 0x3f3f3f3f;
        while (left + 1 < right) {
            if (nums1[mid = left + ((right - left) >> 1)] <= nums2[((m + n - 3) >> 1) - mid + 1]) left = mid;
            else right = mid;
        }
        return ((m + n) & 1) == 1 ? Math.max(left > -1 ? nums1[left] : -inf, (mid = ((m + n - 3) >> 1) - left) > -1 ? nums2[mid] : -inf) : (Math.max(left > -1 ? nums1[left] : -inf, (mid = ((m + n - 3) >> 1) - left) > -1 ? nums2[mid] : -inf) + Math.min(left + 1 < m ? nums1[left + 1] : inf, mid + 1 < n ? nums2[mid + 1] : inf)) / 2.0;
    }
}
```

__Python__:

```Python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        return 1.0 * sorted(nums1 + nums2)[(len(nums1) + len(nums2)) >> 1] if (len(nums1) + len(nums2)) & 1 else (sorted(nums1 + nums2)[(len(nums1) + len(nums2)) >> 1] + sorted(nums1 + nums2)[((len(nums1) + len(nums2)) >> 1) - 1]) / 2
```
