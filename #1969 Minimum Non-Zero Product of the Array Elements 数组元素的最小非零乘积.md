# 1969 Minimum Non-Zero Product of the Array Elements 数组元素的最小非零乘积

__Description:__

You are given a positive integer `p`. Consider an array `nums` (__1-indexed__) that consists of the integers in the __inclusive__ range `[1, 2 ^ p - 1]` in their binary representations. You are allowed to do the following operation __any__ number of times:

- Choose two elements `x` and `y` from `nums`.
- Choose a bit in `x` and swap it with its corresponding bit in `y`. Corresponding bit refers to the bit that is in the __same position__ in the other integer.

For example, if `x = 1101` and `y = 0011`, after swapping the `2 ^ nd` bit from the right, we have `x = 1111` and `y = 0001`.

Find the __minimum non-zero__ product of `nums` after performing the above operation __any__ number of times. Return _this product_ ___modulo___ `10 ^ 9 + 7`.

Note: The answer should be the minimum product before the modulo operation is done.

__Example:__

Example 1:

```text
Input: p = 1
Output: 1
Explanation: nums = [1].
There is only one element, so the product equals that element.
```

Example 2:

```text
Input: p = 2
Output: 6
Explanation: nums = [01, 10, 11].
Any swap would either make the product 0 or stay the same.
Thus, the array product of 1 * 2 * 3 = 6 is already minimized.
```

Example 3:

```text
Input: p = 3
Output: 1512
Explanation: nums = [001, 010, 011, 100, 101, 110, 111]
```

- In the first operation we can swap the leftmost bit of the second and fifth elements.
  - The resulting array is [001, 110, 011, 100, 001, 110, 111].
- In the second operation we can swap the middle bit of the third and fourth elements.
  - The resulting array is [001, 110, 001, 110, 001, 110, 111].

The array product is 1 \* 6 \* 1 \* 6 \* 1 \* 6 \* 7 = 1512, which is the minimum possible product.

__Constraints:__

- `1 <= p <= 60`

__题目描述:__

给你一个正整数 `p` 。你有一个下标从 __1__ 开始的数组 `nums` ，这个数组包含范围 `[1, 2 ^ p - 1]` 内所有整数的二进制形式（两端都 __包含__）。你可以进行以下操作 __任意__ 次:

- 从 `nums` 中选择两个元素 `x` 和 `y` 。
- 选择 `x` 中的一位与 `y` 对应位置的位交换。对应位置指的是两个整数 __相同位置__ 的二进制位。

比方说，如果 x = 11___0___1 且 y = 00___1___1 ，交换右边数起第 `2` 位后，我们得到 x = 11___1___1 和 y = 00___0___1 。

请你算出进行以上操作 __任意次__ 以后， `nums` 能得到的 __最小非零__ 乘积。将乘积对 `10 ^ 9 + 7` __取余__ 后返回。

注意：答案应为取余 之前 的最小值。

__示例:__

示例 1：

```text
输入：p = 1
输出：1
解释：nums = [1] 。
只有一个元素，所以乘积为该元素。
```

示例 2：

```text
输入：p = 2
输出：6
解释：nums = [01, 10, 11] 。
所有交换要么使乘积变为 0 ，要么乘积与初始乘积相同。
所以，数组乘积 1 * 2 * 3 = 6 已经是最小值。
```

示例 3：

```text
输入：p = 3
输出：1512
解释：nums = [001, 010, 011, 100, 101, 110, 111]
```

- 第一次操作中，我们交换第二个和第五个元素最左边的数位。
  - 结果数组为 [001, 110, 011, 100, 001, 110, 111] 。
- 第二次操作中，我们交换第三个和第四个元素中间的数位。
  - 结果数组为 [001, 110, 001, 110, 001, 110, 111] 。

数组乘积 1 \* 6 \* 1 \* 6 \* 1 \* 6 \* 7 = 1512 是最小乘积。

__提示：__

- `1 <= p <= 60`

__思路:__

```text
1. 贪心
要求最小非零乘积, 那么就要尽可能的让 1 的个数多
从 1 << (p - 1) 为分界线, 将数组分为两部分
一一对称交换, 使得 1 的个数最多
交换完之后, 1 和 (1 << p) - 2 的个数都为 (1 << (p - 1)) - 1, 还剩下一个 (1 << p) - 1
乘积为 ((1 << p) - 1) * (pow((1 << p) - 2, (1 << (p - 1)) - 1) % mod) % mod
时间复杂度为 O(N), 空间复杂度为 O(1)
2. 打表
由于 P <= 60
预先计算出每个 P 对应的结果即可
时间复杂度为 O(1), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
const int mod = 1e9 + 7;
class Solution 
{
public:
    int minNonZeroProduct(int p) 
    {
        long long result = ((1LL << p) - 1) % mod, k = (1LL << (p - 1)) - 1;
        for (long long a = ((1LL << p) - 2) % mod; k; k >>= 1)
        {
            result = result * a % mod;
            a = a * a % mod;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minNonZeroProduct(int p) {
        return new int[]{0, 1, 6, 1512, 581202553, 202795991, 57405498, 316555604, 9253531, 857438053, 586669277, 647824153, 93512543, 391630296, 187678728, 431467833, 539112180, 368376380, 150112795, 484576688, 212293935, 828477683, 106294648, 618323081, 186692306, 513022074, 109245444, 821184946, 2043018, 26450314, 945196305, 138191773, 505517599, 861896614, 640964173, 112322054, 217659727, 680742062, 673217940, 945471045, 554966674, 190830260, 403329489, 305023508, 229675479, 865308368, 689473871, 161536946, 99452142, 720364340, 172386396, 198445540, 265347860, 504260931, 247773741, 65332879, 891336224, 221172799, 643213635, 926891661, 813987236}[p];
    }
}
```

__Python__:

```Python
class Solution:
    def minNonZeroProduct(self, p: int) -> int:
        def a(i):
            return (2 ** p - 1) * pow(2 ** p - 2 , 2 ** (p - 1) - 1 , (10 ** 9 + 7)) % (10 ** 9 + 7)
        print([a(i) for i in range(1, 61)])
        return (2 ** p - 1) * pow(2 ** p - 2 , 2 ** (p - 1) - 1 , (10 ** 9 + 7)) % (10 ** 9 + 7)
```
