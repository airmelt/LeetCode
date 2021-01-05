# 88 Merge Sorted Array 合并两个有序数组

__Description__:
Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

__Note__:
The number of elements initialized in nums1 and nums2 are m and n respectively.
You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2.

__Example__:

Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3
Output: [1,2,2,3,5,6]

__题目描述__:
给定两个有序整数数组 nums1 和 nums2，将 nums2 合并到 nums1 中，使得 num1 成为一个有序数组。

__说明__：
初始化 nums1 和 nums2 的元素数量分别为 m 和 n。
你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。

__示例__:

输入:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3
输出: [1,2,2,3,5,6]

__思路__:

因为数组已经排序, 可以考虑从后往前插入元素
时间复杂度O(m + n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) 
    {
        int i = m-- + n-- - 1;
        while (m > -1 and n > -1) nums1[i--] = nums1[m] > nums2[n] ? nums1[m--] : nums2[n--];
        while (n > -1) nums1[i--] = nums2[n--];
    }
};
```

__Java__:

```Java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m-- + n-- - 1;
        while (m > -1 && n > -1) nums1[i--] = nums1[m] > nums2[n] ? nums1[m--] : nums2[n--];
        while (n > -1) nums1[i--] = nums2[n--];
    }
}
```

__Python__:

```Python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        i = m + n - 1
        m -= 1
        n -= 1
        while m > -1 and n > -1:
            if nums1[m] > nums2[n]:
                nums1[i] = nums1[m]
                m -= 1
            else:
                nums1[i] = nums2[n]
                n -= 1
            i -= 1
        while n > -1:
            nums1[i] = nums2[n]
            n -= 1
            i -= 1
```
