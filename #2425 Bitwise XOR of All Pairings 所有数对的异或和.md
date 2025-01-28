# 2425 Bitwise XOR of All Pairings 所有数对的异或和

__Description:__

You are given two __0-indexed__ arrays, `nums1` and `nums2`, consisting of non-negative integers. There exists another array, `nums3`, which contains the bitwise XOR of __all pairings__ of integers between `nums1` and `nums2` (every integer in `nums1` is paired with every integer in `nums2` __exactly once__).

Return _the __bitwise XOR__ of all integers in_ `nums3`.

__Example:__

Example 1:

```text
Input: nums1 = [2,1,3], nums2 = [10,2,5,0]
Output: 13
Explanation:
A possible nums3 array is [8,0,7,2,11,3,4,1,9,1,6,3].
The bitwise XOR of all these numbers is 13, so we return 13.
```

Example 2:

```text
Input: nums1 = [1,2], nums2 = [3,4]
Output: 0
Explanation:
All possible pairs of bitwise XORs are nums1[0]  ^  nums2[0], nums1[0]  ^  nums2[1], nums1[1]  ^  nums2[0],
and nums1[1]  ^  nums2[1].
Thus, one possible nums3 array is [2,5,1,6].
2  ^  5  ^  1  ^  6 = 0, so we return 0.
```

__Constraints:__

- `1 <= nums1.length, nums2.length <= 10 ^ 5`
- `0 <= nums1[i], nums2[j] <= 10 ^ 9`

__题目描述:__

给你两个下标从 __0__ 开始的数组 `nums1` 和 `nums2` ，两个数组都只包含非负整数。请你求出另外一个数组 `nums3` ，包含 `nums1` 和 `nums2` 中 __所有数对__ 的异或和（ `nums1` 中每个整数都跟 `nums2` 中每个整数 __恰好__ 匹配一次）。

请你返回 `nums3` 中所有整数的 __异或和__ 。

__示例:__

示例 1：

```text
输入：nums1 = [2,1,3], nums2 = [10,2,5,0]
输出：13
解释：
一个可能的 nums3 数组是 [8,0,7,2,11,3,4,1,9,1,6,3] 。
所有这些数字的异或和是 13 ，所以我们返回 13 。
```

示例 2：

```text
输入：nums1 = [1,2], nums2 = [3,4]
输出：0
解释：
所有数对异或和的结果分别为 nums1[0]  ^  nums2[0] ，nums1[0]  ^  nums2[1] ，nums1[1]  ^  nums2[0] 和 nums1[1]  ^  nums2[1] 。
所以，一个可能的 nums3 数组是 [2,5,1,6] 。
2  ^  5  ^  1  ^  6 = 0 ，所以我们返回 0 。
```

__提示：__

- `1 <= nums1.length, nums2.length <= 10 ^ 5`
- `0 <= nums1[i], nums2[j] <= 10 ^ 9`

__思路:__

```text
1. 暴力法
遍历 nums1 和 nums2 的所有数对, 计算异或和
时间复杂度为 O(MN), 空间复杂度为 O(1)
2. 贡献法
对于 nums1 的第一个数 nums1[0]
它与 nums2 的所有数异或, 贡献了 nums2.length 次
即 nums1[0]  ^  nums2[0], nums1[0]  ^  nums2[1], ..., nums1[0]  ^  nums2[nums2.length - 1]
这些数字的异或和中 nums1[0] 出现了 nums2.length 次
所以如果 nums2.length 为奇数, nums1[0] 对结果的贡献为 nums1[0], 否则为 0
同理, 对于 nums2 的第一个数 nums2[0], 只需要考虑 nums1 的长度是否为奇数
分别根据另一个数组的长度计算 nums1 和 nums2 的所有数字的异或和
再将结果异或即可
时间复杂度为 O(M + N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution {
    public int xorAllNums(int[] nums1, int[] nums2) {
        int result = 0;
        if ((nums1.length & 1) == 1) for (int num : nums2) result ^= num;
        if ((nums2.length & 1) == 1) for (int num : nums1) result ^= num;
        return result;
    }
}
```

__Java__:

```Java
class Solution {
    public int xorAllNums(int[] nums1, int[] nums2) {
        int result = 0;
        if ((nums1.length & 1) == 1) for (int num : nums2) result ^= num;
        if ((nums2.length & 1) == 1) for (int num : nums1) result ^= num;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def xorAllNums(self, nums1: List[int], nums2: List[int]) -> int:
        return (reduce(xor, nums2) if len(nums1) & 1 else 0) ^ (reduce(xor, nums1) if len(nums2) & 1 else 0)
```
