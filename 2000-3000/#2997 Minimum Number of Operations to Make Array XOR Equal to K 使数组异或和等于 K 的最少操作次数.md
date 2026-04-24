# 2997 Minimum Number of Operations to Make Array XOR Equal to K 使数组异或和等于 K 的最少操作次数

__Description:__

You are given a __0-indexed__ integer array `nums` and a positive integer `k`.

You can apply the following operation on the array any number of times:

- Choose __any__ element of the array and __flip__ a bit in its __binary__ representation. Flipping a bit means changing a `0` to `1` or vice versa.

Return _the __minimum__ number of operations required to make the bitwise_ `XOR` _of __all__ elements of the final array equal to_ `k`.

__Note__ that you can flip leading zero bits in the binary representation of elements. For example, for the number `(101)2` you can flip the fourth bit and obtain `(1101)2`.

__Example:__

Example 1:

```text
Input: nums = [2,1,3,4], k = 1
Output: 2
Explanation: We can do the following operations:
```

- Choose element 2 which is 3 == (011)2, we flip the first bit and we obtain (010)2 == 2. nums becomes [2,1,2,4].
- Choose element 0 which is 2 == (010)2, we flip the third bit and we obtain (110)2 = 6. nums becomes [6,1,2,4].

The XOR of elements of the final array is (6 XOR 1 XOR 2 XOR 4) == 1 == k.

It can be shown that we cannot make the XOR equal to k in less than 2 operations.

Example 2:

```text
Input: nums = [2,0,2,0], k = 0
Output: 0
Explanation: The XOR of elements of the array is (2 XOR 0 XOR 2 XOR 0) == 0 == k. So no operation is needed.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 6`
- `0 <= k <= 10 ^ 6`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个正整数 `k` 。

你可以对数组执行以下操作 任意次 ：

- 选择数组里的 __任意__ 一个元素，并将它的 __二进制__ 表示 __翻转__ 一个数位，翻转数位表示将 `0` 变成 `1` 或者将 `1` 变成 `0` 。

你的目标是让数组里 __所有__ 元素的按位异或和得到 `k` ，请你返回达成这一目标的 __最少__ 操作次数。

__注意__，你也可以将一个数的前导 0 翻转。比方说，数字 `(101)2` 翻转第四个数位，得到 `(1101)2` 。

__示例:__

示例 1：

```text
输入：nums = [2,1,3,4], k = 1
输出：2
解释：我们可以执行以下操作：
```

- 选择下标为 2 的元素，也就是 3 == (011)2 ，我们翻转第一个数位得到 (010)2 == 2 。数组变为 [2,1,2,4] 。
- 选择下标为 0 的元素，也就是 2 == (010)2 ，我们翻转第三个数位得到 (110)2 == 6 。数组变为 [6,1,2,4] 。

最终数组的所有元素异或和为 (6 XOR 1 XOR 2 XOR 4) == 1 == k 。
无法用少于 2 次操作得到异或和等于 k 。

示例 2：

```text
输入：nums = [2,0,2,0], k = 0
输出：0
解释：数组所有元素的异或和为 (2 XOR 0 XOR 2 XOR 0) == 0 == k 。所以不需要进行任何操作。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 6`
- `0 <= k <= 10 ^ 6`

__思路:__

```text
位运算
直接将 k 和数组中所有元素进行异或和
得到结果的 1 的位数就是需要翻转的位数
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& nums, int k) 
    {
        return popcount((uint32_t)reduce(nums.begin(), nums.end(), k, bit_xor()));
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[] nums, int k) {
        return Integer.bitCount(Arrays.stream(nums).reduce(0, (a, b) -> a ^ b) ^ k);
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums: List[int], k: int) -> int:
        return (reduce(xor, nums) ^ k).bit_count()
```
