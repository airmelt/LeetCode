# 1551 Minimum Operations to Make Array Equal 使数组中所有元素相等的最小操作数

__Description:__

You have an array `arr` of length `n` where `arr[i] = (2 * i) + 1` for all valid values of `i` (i.e., `0 <= i < n`).

In one operation, you can select two indices `x` and `y` where `0 <= x, y < n` and subtract `1` from `arr[x]` and add `1` to `arr[y]` (i.e., perform `arr[x] -=1` and `arr[y] += 1`). The goal is to make all the elements of the array __equal__. It is __guaranteed__ that all the elements of the array can be made equal using some operations.

Given an integer `n`, the length of the array, return _the minimum number of operations_ needed to make all the elements of arr equal.

__Example:__

Example 1:

```text
Input: n = 3
Output: 2
Explanation: arr = [1, 3, 5]
First operation choose x = 2 and y = 0, this leads arr to be [2, 3, 4]
In the second operation choose x = 2 and y = 0 again, thus arr = [3, 3, 3].
```

Example 2:

```text
Input: n = 6
Output: 9
```

__Constraints:__

- `1 <= n <= 10 ^ 4`

__题目描述:__

存在一个长度为 `n` 的数组 `arr` ，其中 `arr[i] = (2 * i) + 1` （ `0 <= i < n` ）。

一次操作中，你可以选出两个下标，记作 `x` 和 `y` （ `0 <= x, y < n` ）并使 `arr[x]` 减去 `1` 、 `arr[y]` 加上 `1` （即 `arr[x] -=1` 且 `arr[y] += 1` ）。最终的目标是使数组中的所有元素都 __相等__ 。题目测试用例将会 __保证__ ：在执行若干步操作后，数组中的所有元素最终可以全部相等。

给你一个整数 `n`，即数组的长度。请你返回使数组 `arr` 中所有元素相等所需的 __最小操作数__ 。

__示例:__

示例 1：

```text
输入：n = 3
输出：2
解释：arr = [1, 3, 5]
第一次操作选出 x = 2 和 y = 0，使数组变为 [2, 3, 4]
第二次操作继续选出 x = 2 和 y = 0，数组将会变成 [3, 3, 3]
```

示例 2：

```text
输入：n = 6
输出：9
```

__提示：__

- `1 <= n <= 10 ^ 4`

__思路:__

```text
数学
只要将所有的数字都加减到中位数即可
用到等差数列求和
需要分成奇偶讨论
当 n 为奇数时, 需要加减到中间的数字, 总操作数为 (n + 1) ** 2 / 4 - (n + 1) / 2
当 n 为偶数时, 需要加减到 n, 总操作数为 n ** 2 / 4
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(int n) 
    {
        return (n & 1) ? ((n + 1) >> 1) * ((n + 1) >> 1) - ((n + 1) >> 1) : n * n >> 2;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int n) {
        return (n & 1) == 1 ? ((n + 1) >> 1) * ((n + 1) >> 1) - ((n + 1) >> 1) : n * n >> 2;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, n: int) -> int:
        return ((n + 1) >> 1) ** 2 - ((n + 1) >> 1) if (n & 1) else (n >> 1) ** 2
```
