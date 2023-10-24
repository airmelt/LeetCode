# 1855 Maximum Distance Between a Pair of Values 下标对中的最大距离

__Description:__

You are given two __non-increasing 0-indexed__ integer arrays `nums1`​​​​​​ and `nums2`​​​​​​.

A pair of indices `(i, j)`, where `0 <= i < nums1.length` and `0 <= j < nums2.length`, is __valid__ if both `i <= j` and `nums1[i] <= nums2[j]`. The __distance__ of the pair is `j - i`​​​​.

Return _the __maximum distance__ of any __valid__ pair_ `(i, j)`_. If there are no valid pairs, return_ `0`.

An array `arr` is __non-increasing__ if `arr[i-1] >= arr[i]` for every `1 <= i < arr.length`.

__Example:__

Example 1:

```text
Input: nums1 = [55,30,5,4,2], nums2 = [100,20,10,10,5]
Output: 2
Explanation: The valid pairs are (0,0), (2,2), (2,3), (2,4), (3,3), (3,4), and (4,4).
The maximum distance is 2 with pair (2,4).
```

Example 2:

```text
Input: nums1 = [2,2,2], nums2 = [10,10,1]
Output: 1
Explanation: The valid pairs are (0,0), (0,1), and (1,1).
The maximum distance is 1 with pair (0,1).
```

Example 3:

```text
Input: nums1 = [30,29,19,5], nums2 = [25,25,25,25,25]
Output: 2
Explanation: The valid pairs are (2,2), (2,3), (2,4), (3,3), and (3,4).
The maximum distance is 2 with pair (2,4).
```

__Constraints:__

- `1 <= nums1.length, nums2.length <= 10 ^ 5`
- `1 <= nums1[i], nums2[j] <= 10 ^ 5`
- Both `nums1` and `nums2` are __non-increasing__.

__题目描述:__

给你两个 __非递增__ 的整数数组 `nums1`​​​​​​ 和 `nums2`​​​​​​ ，数组下标均 __从 0 开始__ 计数。

下标对 `(i, j)` 中 `0 <= i < nums1.length` 且 `0 <= j < nums2.length` 。如果该下标对同时满足 `i <= j` 且 `nums1[i] <= nums2[j]` ，则称之为 __有效__ 下标对，该下标对的 __距离__ 为 `j - i`​​ 。​​

返回所有 __有效__ 下标对 `(i, j)` 中的 __最大距离__ 。如果不存在有效下标对，返回 `0` 。

一个数组 `arr` ，如果每个 `1 <= i < arr.length` 均有 `arr[i-1] >= arr[i]` 成立，那么该数组是一个 __非递增__ 数组。

__示例:__

示例 1：

```text
输入：nums1 = [55,30,5,4,2], nums2 = [100,20,10,10,5]
输出：2
解释：有效下标对是 (0,0), (2,2), (2,3), (2,4), (3,3), (3,4) 和 (4,4) 。
最大距离是 2 ，对应下标对 (2,4) 。
```

示例 2：

```text
输入：nums1 = [2,2,2], nums2 = [10,10,1]
输出：1
解释：有效下标对是 (0,0), (0,1) 和 (1,1) 。
最大距离是 1 ，对应下标对 (0,1) 。
```

示例 3：

```text
输入：nums1 = [30,29,19,5], nums2 = [25,25,25,25,25]
输出：2
解释：有效下标对是 (2,2), (2,3), (2,4), (3,3) 和 (3,4) 。
最大距离是 2 ，对应下标对 (2,4) 。
```

__提示：__

- `1 <= nums1.length <= 10 ^ 5`
- `1 <= nums2.length <= 10 ^ 5`
- `1 <= nums1[i], nums2[j] <= 10 ^ 5`
- `nums1` 和 `nums2` 都是 __非递增__ 数组

__思路:__

```text
1. 二分查找
由于数组都是有序的
可以使用二分查找找到差值最大的下标对
并更新结果
时间复杂度为 O(MlogN), 空间复杂度为 O(1), 其中 M 为 nums1 的长度, N 为 nums2 的长度
2. 双指针
由于数组都是有序的
可以使用双指针找到差值最大的下标对
一个指针指向 nums1 的开始
另一个指针指向 nums2 的开始
如果 nums1[i] > nums2[j], 则 i++ 直到找到一个 nums1[i] <= nums2[j]
更新结果即可
时间复杂度为 O(M + N), 空间复杂度为 O(1), 其中 M 为 nums1 的长度, N 为 nums2 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxDistance(vector<int>& nums1, vector<int>& nums2) 
    {
        int m = nums1.size(), n = nums2.size(), i = 0, result = 0;
        for (int j = 0; j < n; j++) 
        {
            while (i < m and nums1[i] > nums2[j]) ++i;
            if (i < m) result = max(result, j - i);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxDistance(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length, i = 0, result = 0;
        for (int j = 0; j < n; j++) {
            while (i < m && nums1[i] > nums2[j]) ++i;
            if (i < m) result = Math.max(result, j - i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxDistance(self, nums1: List[int], nums2: List[int]) -> int:
        result, m, n, nums2 = 0, len(nums1), len(nums2), nums2[::-1]
        for i in range(min(m, n)):
            result = max(result, n - bisect_left(nums2, nums1[i]) - 1 - i)
        return result
```
