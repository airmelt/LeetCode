# 1486 XOR Operation in an Array 数组异或操作

__Description:__

You are given an integer `n` and an integer `start`.

Define an array `nums` where `nums[i] = start + 2 * i` (__0-indexed__) and `n == nums.length`.

Return _the bitwise XOR of all elements of_ `nums`.

__Example:__

Example 1:

```text
Input: n = 5, start = 0
Output: 8
Explanation: Array nums is equal to [0, 2, 4, 6, 8] where (0  ^  2  ^  4  ^  6  ^  8) = 8.
Where " ^ " corresponds to bitwise XOR operator.
```

Example 2:

```text
Input: n = 4, start = 3
Output: 8
Explanation: Array nums is equal to [3, 5, 7, 9] where (3  ^  5  ^  7  ^  9) = 8.
```

__Constraints:__

- `1 <= n <= 1000`
- `0 <= start <= 1000`
- `n == nums.length`

__题目描述:__

给你两个整数， `n` 和 `start` 。

数组 `nums` 定义为： `nums[i] = start + 2*i`（下标从 0 开始）且 `n == nums.length` 。

请返回 `nums` 中所有元素按位异或（__XOR__）后得到的结果。

__示例:__

示例 1：

```text
输入：n = 5, start = 0
输出：8
解释：数组 nums 为 [0, 2, 4, 6, 8]，其中 (0  ^  2  ^  4  ^  6  ^  8) = 8 。
     " ^ " 为按位异或 XOR 运算符。
```

示例 2：

```text
输入：n = 4, start = 3
输出：8
解释：数组 nums 为 [3, 5, 7, 9]，其中 (3  ^  5  ^  7  ^  9) = 8.
```

示例 3：

```text
输入：n = 1, start = 7
输出：7
```

示例 4：

```text
输入：n = 10, start = 5
输出：2
```

__提示：__

- `1 <= n <= 1000`
- `0 <= start <= 1000`
- `n == nums.length`

__思路:__

```text
1. 数学
注意到 [n, n + 2, n + 4, n + 6] 的异或和为 0, 其中 n 为 4 的倍数
那么可以根据 n % 4 将 n 的异或和的结果分成 4 类
最后根据 start 和 n 的奇偶调整即可
时间复杂度为 O(1), 空间复杂度为 O(1)
2. 模拟
直接按照题意模拟异或和
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int xorOperation(int n, int start) 
    {
        int result = start;
        for (int i = 1; i < n; i++) result ^= (start + (i << 1));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int xorOperation(int n, int start) {
        int result = start;
        for (int i = 1; i < n; i++) result ^= (start + (i << 1));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def xorOperation(self, n: int, start: int) -> int:
        return ((start & 1) and (n & 1)) + (((f := lambda n: [n, 1, n + 1, 0][n % 4] if n >= 0 else 0)((start >> 1) + n - 1) ^ f((start >> 1) - 1)) << 1)
```
