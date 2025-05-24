# 2540 Minimum Common Value 最小公共值

__Description:__

Given two integer arrays `nums1` and `nums2`, sorted in non-decreasing order, return _the __minimum integer common__ to both arrays_. If there is no common integer amongst `nums1` and `nums2`, return `-1`.

Note that an integer is said to be __common__ to `nums1` and `nums2` if both arrays have __at least one__ occurrence of that integer.

__Example:__

Example 1:

```text
Input: nums1 = [1,2,3], nums2 = [2,4]
Output: 2
Explanation: The smallest element common to both arrays is 2, so we return 2.
```

Example 2:

```text
Input: nums1 = [1,2,3,6], nums2 = [2,3,4,5]
Output: 2
Explanation: There are two common elements in the array 2 and 3 out of which 2 is the smallest, so 2 is returned.
```

__Constraints:__

- `1 <= nums1.length, nums2.length <= 10 ^ 5`
- `1 <= nums1[i], nums2[j] <= 10 ^ 9`
- Both `nums1` and `nums2` are sorted in __non-decreasing__ order.

__题目描述:__

给你两个整数数组 `nums1` 和 `nums2` ，它们已经按非降序排序，请你返回两个数组的 __最小公共整数__ 。如果两个数组 `nums1` 和 `nums2` 没有公共整数，请你返回 `-1` 。

如果一个整数在两个数组中都 __至少出现一次__ ，那么这个整数是数组 `nums1` 和 `nums2` __公共__ 的。

__示例:__

示例 1：

```text
输入：nums1 = [1,2,3], nums2 = [2,4]
输出：2
解释：两个数组的最小公共元素是 2 ，所以我们返回 2 。
```

示例 2：

```text
输入：nums1 = [1,2,3,6], nums2 = [2,3,4,5]
输出：2
解释：两个数组中的公共元素是 2 和 3 ，2 是较小值，所以返回 2 。
```

__提示：__

- `1 <= nums1.length, nums2.length <= 10 ^ 5`
- `1 <= nums1[i], nums2[j] <= 10 ^ 9`
- `nums1` 和 `nums2` 都是 __非降序__ 的。

__思路:__

```text
双指针
由于数组已经按照非降序排列
当 j < m 且 nums2[j] < num 时, j++
当 j < m 且 nums2[j] == num 时, 返回 num
否则说明 nums1[i] 都比 nums2[j:] 小, 继续遍历 nums1
最后退出循环说明没有公共元素, 返回 -1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int getCommon(vector<int>& nums1, vector<int>& nums2) 
    {
        int j = 0, m = nums2.size();
        for (const auto& num : nums1) 
        {
            while (j < m and nums2[j] < num) ++j;
            if (j < m and nums2[j] == num) return num;
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int getCommon(int[] nums1, int[] nums2) {
        int j = 0, m = nums2.length;
        for (int num : nums1) {
            while (j < m && nums2[j] < num) ++j;
            if (j < m && nums2[j] == num) return num;
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def getCommon(self, nums1: List[int], nums2: List[int]) -> int:
        return min(set(nums1) & set(nums2), default=-1)
```
