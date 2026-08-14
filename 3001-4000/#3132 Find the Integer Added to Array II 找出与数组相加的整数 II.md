# 3132 Find the Integer Added to Array II 找出与数组相加的整数 II

__Description:__

You are given two integer arrays `nums1` and `nums2`.

From `nums1` two elements have been removed, and all other elements have been increased (or decreased in the case of negative) by an integer, represented by the variable `x`.

As a result, `nums1` becomes equal to `nums2`. Two arrays are considered equal when they contain the same integers with the same frequencies.

Return the minimum possible integer `x` that achieves this equivalence.

__Example:__

Example 1:

```text
Input: nums1 = [4,20,16,12,8], nums2 = [14,18,10]

Output: -2

Explanation:

After removing elements at indices [0,4] and adding -2, nums1 becomes [18,14,10].
```

Example 2:

```text
Input: nums1 = [3,5,5,3], nums2 = [7,7]

Output: 2

Explanation:

After removing elements at indices [0,3] and adding 2, nums1 becomes [7,7].
```

__Constraints:__

- `3 <= nums1.length <= 200`
- `nums2.length == nums1.length - 2`
- `0 <= nums1[i], nums2[i] <= 1000`
- The test cases are generated in a way that there is an integer `x` such that `nums1` can become equal to `nums2` by removing two elements and adding `x` to each element of `nums1`.

__题目描述:__

给你两个整数数组 `nums1` 和 `nums2`。

从 `nums1` 中移除两个元素，并且所有其他元素都与变量 `x` 所表示的整数相加。如果 `x` 为负数，则表现为元素值的减少。

执行上述操作后，`nums1` 和 `nums2` 相等 。当两个数组中包含相同的整数，并且这些整数出现的频次相同时，两个数组 __相等__ 。

返回能够实现数组相等的 __最小__ 整数 x 。

返回整数 `x` 。

__示例:__

示例 1：

```text
输入：nums1 = [4,20,16,12,8], nums2 = [14,18,10]

输出：-2

解释：

移除 nums1 中下标为 [0,4] 的两个元素，并且每个元素与 -2 相加后，nums1 变为 [18,14,10] ，与 nums2 相等。
```

示例 2：

```text
输入：nums1 = [3,5,5,3], nums2 = [7,7]

输出：2

解释：

移除 nums1 中下标为 [0,3] 的两个元素，并且每个元素与 2 相加后，nums1 变为 [7,7] ，与 nums2 相等。
```

__提示：__

- `3 <= nums1.length <= 200`
- `nums2.length == nums1.length - 2`
- `0 <= nums1[i], nums2[i] <= 1000`
- 测试用例以这样的方式生成：存在一个整数 `x`，`nums1` 中的每个元素都与 `x` 相加后，再移除两个元素，`nums1` 可以与 `nums2` 相等。

__思路:__

```text
模拟
先将两个数组排序
由于要取到最小的 x
所以从 nums1[2] 开始检查 nums2 是否是 nums1[i:] 的子序列
如果遍历完 nums2 直接返回 x
否则返回 nums2[0] - nums1[0], 题目已经保证能够成立
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要瓶颈在排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int addedInteger(vector<int>& nums1, vector<int>& nums2)
    {
        return *max_element(nums2.begin(), nums2.end()) - *max_element(nums1.begin(), nums1.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int addedInteger(int[] nums1, int[] nums2) {
        return Arrays.stream(nums2).max().getAsInt() - Arrays.stream(nums1).max().getAsInt();
    }
}
```

__Python__:

```Python
class Solution:
    def addedInteger(self, nums1: List[int], nums2: List[int]) -> int:
        return max(nums2) - max(nums1)
```
