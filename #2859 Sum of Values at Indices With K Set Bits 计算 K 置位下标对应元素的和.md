# 2859 Sum of Values at Indices With K Set Bits 计算 K 置位下标对应元素的和

__Description:__

You are given a __0-indexed__ integer array `nums` and an integer `k`.

Return _an integer that denotes the __sum__ of elements in_ `nums` _whose corresponding __indices__ have __exactly___ `k` _set bits in their binary representation._

The __set bits__ in an integer are the `1`'s present when it is written in binary.

- For example, the binary representation of `21` is `10101`, which has `3` set bits.

__Example:__

Example 1:

```text
Input: nums = [5,10,1,5,2], k = 1
Output: 13
Explanation: The binary representation of the indices are: 
0 = 0b000
1 = 0b001
2 = 0b010
3 = 0b011
4 = 0b100
Indices 1, 2, and 4 have k = 1 set bits in their binary representation.
Hence, the answer is nums[1] + nums[2] + nums[4] = 13.
```

Example 2:

```text
Input: nums = [4,3,2,1], k = 2
Output: 1
Explanation: The binary representation of the indices are:
0 = 0b00
1 = 0b01
2 = 0b10
3 = 0b11
Only index 3 has k = 2 set bits in its binary representation.
Hence, the answer is nums[3] = 1.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 5`
- `0 <= k <= 10`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个整数 `k` 。

请你用整数形式返回 `nums` 中的特定元素之 __和__ ，这些特定元素满足:其对应下标的二进制表示中恰存在 `k` 个置位。

整数的二进制表示中的 1 就是这个整数的 置位 。

例如， `21` 的二进制表示为 `10101` ，其中有 `3` 个置位。

__示例:__

示例 1：

```text
输入：nums = [5,10,1,5,2], k = 1
输出：13
解释：下标的二进制表示是： 
0 = 0b000
1 = 0b001
2 = 0b010
3 = 0b011
4 = 0b100
下标 1、2 和 4 在其二进制表示中都存在 k = 1 个置位。
因此，答案为 nums[1] + nums[2] + nums[4] = 13 。
```

示例 2：

```text
输入：nums = [4,3,2,1], k = 2
输出：1
解释：下标的二进制表示是： 
0 = 0b00
1 = 0b01
2 = 0b10
3 = 0b11
只有下标 3 的二进制表示中存在 k = 2 个置位。
因此，答案为 nums[3] = 1 。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 5`
- `0 <= k <= 10`

__思路:__

```text
模拟
将下标 i 为 k 的值过滤出来
求总和即可
时间复杂度为 O(N), 空间复杂度为 O(1), 计算下标为 O(1), 在整形范围内最多计算 32 次
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumIndicesWithKSetBits(vector<int>& nums, int k) 
    {
        int result = 0, n = nums.size();
        for (int i = 0; i < n; i++) if (__builtin_popcount(i) == k) result += nums[i]; 
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumIndicesWithKSetBits(List<Integer> nums, int k) {
        return IntStream.range(0, nums.size()).filter(i -> Integer.bitCount(i) == k).mapToObj(nums::get).mapToInt(Integer::intValue).sum();
    }
}
```

__Python__:

```Python
class Solution:
    def sumIndicesWithKSetBits(self, nums: List[int], k: int) -> int:
        return sum(num for i, num in enumerate(nums) if i.bit_count() == k)
```
