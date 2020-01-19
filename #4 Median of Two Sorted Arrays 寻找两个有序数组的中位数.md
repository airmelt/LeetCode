__Description__:
There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume nums1 and nums2 cannot be both empty.

__Example:__
Example 1:

nums1 = [1, 3]
nums2 = [2]

The median is 2.0

Example 2:

nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5

__题目描述__:
给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

__示例 :__
示例 1:

nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0

示例 2:

nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5

__思路__:
注意到数组为有序数组, 每次找到两个数组中位数比较大小, 将中位数较大的大于中位数的部分(右边)丢弃, 将中位数较小的小于中位数的部分(左边)丢弃, 直到剩下 1(两个数组元素和为奇数)或者2(两个数组元素和为偶数)数组元素, 返回结果的平均值即可
时间复杂度O(log(m + n)), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        if (nums1.size() > nums2.size()) return findMedianSortedArrays(nums2, nums1);
        int m = nums1.size(), n = nums2.size(), left = 0, right = nums1.size();
        while (left <= right) 
        {
            int i = (left + right) >> 1;
            int j = ((m + n + 1) >> 1) - i;
            int left_max1 = i == 0 ? INT_MIN : nums1[i - 1], right_min1 = i == m ? INT_MAX : nums1[i], left_max2 = j == 0 ? INT_MIN : nums2[j - 1], right_min2 = j == n ? INT_MAX : nums2[j];
            if (left_max1 <= right_min2 && left_max2 <= right_min1) 
            {
                if ((m + n) & 1) return max(left_max1, left_max2);
                else return ((max(left_max1, left_max2) + min(right_min1, right_min2))) / 2.0;
            } else if (left_max2 > right_min1) left = i + 1;
            else right = i - 1;
        }
        return 0.0;
    }
};
```

__Java__:
```Java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int left = (m + n + 1) >>> 1, right = (m + n + 2) >>> 1;
        return (helper(nums1, 0, nums2, 0, left) + helper(nums1, 0, nums2, 0, right)) / 2.0;
    }
    
    private int helper(int[] nums1, int i, int[] nums2, int j, int k) {
        if (i + 1 > nums1.length) return nums2[j + k - 1];
        if (j + 1 > nums2.length) return nums1[i + k - 1];
        if (k == 1) return Math.min(nums1[i], nums2[j]);
        int mid1 = (i + (k >>> 1) - 1 < nums1.length) ? nums1[i + (k >>> 1) - 1] : Integer.MAX_VALUE, mid2 = (j + (k >>> 1) - 1 < nums2.length) ? nums2[j + (k >>> 1) - 1] : Integer.MAX_VALUE;
        if (mid1 < mid2) return helper(nums1, i + (k >>> 1), nums2, j, k - (k >>> 1));
        else return helper(nums1, i, nums2, j + (k >>> 1), k - (k >>> 1));
    }
}
```

__Python__:
```Python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        return 1.0 * sorted(nums1 + nums2)[(len(nums1) + len(nums2)) >> 1] if (len(nums1) + len(nums2)) & 1 else (sorted(nums1 + nums2)[(len(nums1) + len(nums2)) >> 1] + sorted(nums1 + nums2)[((len(nums1) + len(nums2)) >> 1) - 1]) / 2
```