# 2683 Neighboring Bitwise XOR 相邻值的按位异或

__Description:__

A __0-indexed__ array `derived` with length `n` is derived by computing the __bitwise XOR__ (⊕) of adjacent values in a __binary array__ `original` of length `n`.

Specifically, for each index `i` in the range `[0, n - 1]`:

- If `i = n - 1`, then `derived[i] = original[i] ⊕ original[0]`.
- Otherwise, `derived[i] = original[i] ⊕ original[i + 1]`.

Given an array `derived`, your task is to determine whether there exists a __valid binary array__ `original` that could have formed `derived`.

Return true if such an array exists or false otherwise.

- A binary array is an array containing only __0's__ and __1's__

__Example:__

Example 1:

```text
Input: derived = [1,1,0]
Output: true
Explanation: A valid original array that gives derived is [0,1,0].
derived[0] = original[0] ⊕ original[1] = 0 ⊕ 1 = 1 
derived[1] = original[1] ⊕ original[2] = 1 ⊕ 0 = 1
derived[2] = original[2] ⊕ original[0] = 0 ⊕ 0 = 0
```

Example 2:

```text
Input: derived = [1,1]
Output: true
Explanation: A valid original array that gives derived is [0,1].
derived[0] = original[0] ⊕ original[1] = 1
derived[1] = original[1] ⊕ original[0] = 1
```

Example 3:

```text
Input: derived = [1,0]
Output: false
Explanation: There is no valid original array that gives derived.
```

__Constraints:__

- `n == derived.length`
- `1 <= n <= 10 ^ 5`
- The values in `derived` are either __0's__ or __1's__

__题目描述:__

下标从 __0__ 开始、长度为 `n` 的数组 `derived` 是由同样长度为 `n` 的原始 __二进制数组__ `original` 通过计算相邻值的 __按位异或（⊕）__派生而来。

特别地，对于范围 `[0, n - 1]` 内的每个下标 `i` :

- 如果 `i = n - 1` ，那么 `derived[i] = original[i] ⊕ original[0]`
- 否则 `derived[i] = original[i] ⊕ original[i + 1]`

给你一个数组 `derived` ，请判断是否存在一个能够派生得到 `derived` 的 __有效原始二进制数组__ `original` 。

如果存在满足要求的原始二进制数组，返回 true ；否则，返回 false 。

- 二进制数组是仅由 __0__ 和 __1__ 组成的数组。

__示例:__

示例 1：

```text
输入：derived = [1,1,0]
输出：true
解释：能够派生得到 [1,1,0] 的有效原始二进制数组是 [0,1,0] ：
derived[0] = original[0] ⊕ original[1] = 0 ⊕ 1 = 1 
derived[1] = original[1] ⊕ original[2] = 1 ⊕ 0 = 1
derived[2] = original[2] ⊕ original[0] = 0 ⊕ 0 = 0
```

示例 2：

```text
输入：derived = [1,1]
输出：true
解释：能够派生得到 [1,1] 的有效原始二进制数组是 [0,1] ：
derived[0] = original[0] ⊕ original[1] = 1
derived[1] = original[1] ⊕ original[0] = 1
```

示例 3：

```text
输入：derived = [1,0]
输出：false
解释：不存在能够派生得到 [1,0] 的有效原始二进制数组。
```

__提示：__

- `n == derived.length`
- `1 <= n <= 10 ^ 5`
- `derived` 中的值不是 __0__ 就是 __1__ 。

__思路:__

```text
位运算
把所有 derived 进行异或得到 result
则每个 original 中的元素出现了 2 次
那么这个 result 必须为 0 才有解
由于数组中都是二进制数
也可以求和得到偶数
或者计算 1 的个数为偶数
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool doesValidArrayExist(vector<int>& derived) 
    {
        return reduce(derived.begin(), derived.end(), 0, bit_xor()) == 0;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean doesValidArrayExist(int[] derived) {
        int result = 0;
        for (int x : derived) result ^= x;
        return result == 0;
    }
}
```

__Python__:

```Python
class Solution:
    def doesValidArrayExist(self, derived: List[int]) -> bool:
        return derived.count(1) & 1 == 0
```
