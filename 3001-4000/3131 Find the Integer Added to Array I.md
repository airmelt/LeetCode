# 3131 Find the Integer Added to Array I 找出与数组相加的整数 I

__Description:__

You are given two arrays of equal length, `nums1` and `nums2`.

Each element in `nums1` has been increased (or decreased in the case of negative) by an integer, represented by the variable `x`.

As a result, `nums1` becomes equal to `nums2`. Two arrays are considered equal when they contain the same integers with the same frequencies.

Return the integer `x`.

__Example:__

Example 1:

```text
Input: nums1 = [2,6,4], nums2 = [9,7,5]

Output: 3

Explanation:

The integer added to each element of nums1 is 3.
```

Example 2:

```text
Input: nums1 = [10], nums2 = [5]

Output: -5

Explanation:

The integer added to each element of nums1 is -5.
```

Example 3:

```text
Input: nums1 = [1,1,1,1], nums2 = [1,1,1,1]

Output: 0

Explanation:

The integer added to each element of nums1 is 0.
```

__Constraints:__

- `1 <= nums1.length == nums2.length <= 100`
- `0 <= nums1[i], nums2[i] <= 1000`
- The test cases are generated in a way that there is an integer `x` such that `nums1` can become equal to `nums2` by adding `x` to each element of `nums1`.

__题目描述:__

给你两个长度相等的数组 `nums1` 和 `nums2`。

数组 `nums1` 中的每个元素都与变量 `x` 所表示的整数相加。如果 `x` 为负数，则表现为元素值的减少。

在与 `x` 相加后，`nums1` 和 `nums2` __相等__ 。当两个数组中包含相同的整数，并且这些整数出现的频次相同时，两个数组 __相等__ 。

返回整数 x 。

__示例:__

示例 1：

```text
输入：nums1 = [2,6,4], nums2 = [9,7,5]

输出：3

解释：

与 3 相加后，nums1 和 nums2 相等。
```

示例 2：

```text
输入：nums1 = [10], nums2 = [5]

输出：-5

解释：

与 -5 相加后，nums1 和 nums2 相等。
```

示例 3：

```text
输入：nums1 = [1,1,1,1], nums2 = [1,1,1,1]

输出：0

解释：

与 0 相加后，nums1 和 nums2 相等。
```

__提示：__

- `1 <= nums1.length == nums2.length <= 100`
- `0 <= nums1[i], nums2[i] <= 1000`
- 测试用例以这样的方式生成：存在一个整数 `x`，使得 `nums1` 中的每个元素都与 `x` 相加后，`nums1` 与 `nums2` 相等。

__思路:__

```text
模拟
由于 num1 和 nums2 只相差 x
所以最小/大值对应相差 x
找到最小/大值计算即可
时间复杂度为 O(N), 空间复杂度为 O(1)
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
