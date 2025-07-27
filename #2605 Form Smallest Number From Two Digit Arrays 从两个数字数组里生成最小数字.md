# 2605 Form Smallest Number From Two Digit Arrays 从两个数字数组里生成最小数字

__Description:__

__Example:__

Example 1:

```text
Input: nums1 = [4,1,3], nums2 = [5,7]
Output: 15
Explanation: The number 15 contains the digit 1 from nums1 and the digit 5 from nums2. It can be proven that 15 is the smallest number we can have.
```

Example 2:

```text
Input: nums1 = [3,5,2,6], nums2 = [3,1,7]
Output: 3
Explanation: The number 3 contains the digit 3 which exists in both arrays.
```

__Constraints:__

- `1 <= nums1.length, nums2.length <= 9`
- `1 <= nums1[i], nums2[i] <= 9`
- All digits in each array are __unique__.

__题目描述:__

__示例:__

示例 1：

```text
输入：nums1 = [4,1,3], nums2 = [5,7]
输出：15
解释：数字 15 的数位 1 在 nums1 中出现，数位 5 在 nums2 中出现。15 是我们能得到的最小数字。
```

示例 2：

```text
输入：nums1 = [3,5,2,6], nums2 = [3,1,7]
输出：3
解释：数字 3 的数位 3 在两个数组中都出现了。
```

__提示：__

- `1 <= nums1.length, nums2.length <= 9`
- `1 <= nums1[i], nums2[i] <= 9`
- 每个数组中，元素 __互不相同__ 。

__思路:__

```text
位运算
由于数组中的数字均不大于 9
可以将数组中的所有数字转换为二进制位的形式
如果两个数组有交集, 那么与运算不小于 0
直接取出最低位的数字即可
如果没有交集, 那么取出两个数组中最小的数字, 组合成最小的数字
要么是 10 * min(nums1) + min(nums2), 要么是 10 * min(nums2) + min(nums1)
时间复杂度为 O(M + N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minNumber(vector<int>& nums1, vector<int>& nums2) 
    {
        int mask1 = 0, mask2 = 0, mask = 0, a = 0, b = 0;
        for (const auto& x : nums1) mask1 |= 1 << x;
        for (const auto& x : nums2) mask2 |= 1 << x;
        return (mask = mask1 & mask2) ? __builtin_ctz(mask) : min((a = __builtin_ctz(mask1)) * 10 + (b = __builtin_ctz(mask2)), b * 10 + a);
    }
};
```

__Java__:

```Java
class Solution {
    public int minNumber(int[] nums1, int[] nums2) {
        int mask1 = 0, mask2 = 0, mask = 0, a = 0, b = 0;
        for (int x : nums1) mask1 |= 1 << x;
        for (int x : nums2) mask2 |= 1 << x;
        return (mask = mask1 & mask2) > 0 ? Integer.numberOfTrailingZeros(mask) : Math.min((a = Integer.numberOfTrailingZeros(mask1)) * 10 + (b = Integer.numberOfTrailingZeros(mask2)), b * 10 + a);
    }
}
```

__Python__:

```Python
class Solution:
    def minNumber(self, nums1: List[int], nums2: List[int]) -> int:
        return min(s) if (s := set(nums1) & set(nums2)) else min(min(nums1) * 10 + min(nums2), min(nums2) * 10 + min(nums1))
```
