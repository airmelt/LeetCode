# 2698 Find the Punishment Number of an Integer 求一个整数的惩罚数

__Description:__

Given a positive integer `n`, return _the __punishment number___ of `n`.

The __punishment number__ of `n` is defined as the sum of the squares of all integers `i` such that:

- `1 <= i <= n`
- The decimal representation of `i * i` can be partitioned into contiguous substrings such that the sum of the integer values of these substrings equals `i`.

__Example:__

Example 1:

```text
Input: n = 10
Output: 182
Explanation: There are exactly 3 integers i in the range [1, 10] that satisfy the conditions in the statement:
```

- 1 since 1 * 1 = 1
- 9 since 9 * 9 = 81 and 81 can be partitioned into 8 and 1 with a sum equal to 8 + 1 == 9.
- 10 since 10 * 10 = 100 and 100 can be partitioned into 10 and 0 with a sum equal to 10 + 0 == 10.

Hence, the punishment number of 10 is 1 + 81 + 100 = 182

Example 2:

```text
Input: n = 37
Output: 1478
Explanation: There are exactly 4 integers i in the range [1, 37] that satisfy the conditions in the statement:
```

- 1 since 1 * 1 = 1.
- 9 since 9 * 9 = 81 and 81 can be partitioned into 8 + 1.
- 10 since 10 * 10 = 100 and 100 can be partitioned into 10 + 0.
- 36 since 36 * 36 = 1296 and 1296 can be partitioned into 1 + 29 + 6.

Hence, the punishment number of 37 is 1 + 81 + 100 + 1296 = 1478

__Constraints:__

- `1 <= n <= 1000`

__题目描述:__

给你一个正整数 `n` ，请你返回 `n` 的 __惩罚数__ 。

`n` 的 __惩罚数__ 定义为所有满足以下条件 `i` 的数的平方和:

- `1 <= i <= n`
- `i * i` 的十进制表示的字符串可以分割成若干连续子字符串，且这些子字符串对应的整数值之和等于 `i` 。

__示例:__

示例 1：

```text
输入：n = 10
输出：182
解释：总共有 3 个范围在 [1, 10] 的整数 i 满足要求：
```

- 1 ，因为 1 * 1 = 1
- 9 ，因为 9 * 9 = 81 ，且 81 可以分割成 8 + 1 。
- 10 ，因为 10 * 10 = 100 ，且 100 可以分割成 10 + 0 。

因此，10 的惩罚数为 1 + 81 + 100 = 182

示例 2：

```text
输入：n = 37
输出：1478
解释：总共有 4 个范围在 [1, 37] 的整数 i 满足要求：
```

- 1 ，因为 1 * 1 = 1
- 9 ，因为 9 * 9 = 81 ，且 81 可以分割成 8 + 1 。
- 10 ，因为 10 * 10 = 100 ，且 100 可以分割成 10 + 0 。
- 36 ，因为 36 * 36 = 1296 ，且 1296 可以分割成 1 + 29 + 6 。

因此，37 的惩罚数为 1 + 81 + 100 + 1296 = 1478

__提示：__

- `1 <= n <= 1000`

__思路:__

```text
DFS
判断一个数是否是满足要求的数
可以使用 DFS 遍历字符串或者对 10 取模并求和
处理完成之后也可以直接打表
DFS 处理完之后求出前缀和
直接返回前缀和对应的数字即可
时间复杂度为 O(1), 空间复杂度为 O(1), 实际上需要 N 的范围的空间 C, 预处理需要 C ^ (1 + 2lg2) 因为数字 C 的长度为 1 + 2lg2 向下取整
```

__代码:__

__C++__:

```C++
int pre[1001];

int init = []()
{
    for (int i = 1; i < 1001; i++)
    {
        auto dfs = [&](auto&& dfs, int remain, int cur) -> bool
        {
            if (!cur) return remain == 0;
            for (int j = 10; j <= cur * 10 and remain - cur % j >= 0; j *= 10) if (dfs(dfs, remain - cur % j, cur / j)) return true;
            return false;
        };
        pre[i] = pre[i - 1] + dfs(dfs, i, i * i) * (i * i);    
    }
    return 0;
}();

class Solution 
{
public:
    int punishmentNumber(int n) 
    {
        return pre[n];
    }
};
```

__Java__:

```Java
class Solution {
    private static final int[] pre = new int[1001], a = new int[]{ 1, 9, 10, 36, 45, 55, 82, 91, 99, 100, 235, 297, 369, 370, 379, 414, 657, 675, 703, 756, 792, 909, 918, 945, 964, 990, 991, 999, 1000 };

    static {
        for (int i = 1, j = 0; i < 1001; i++) pre[i] = pre[i - 1] + (i == a[j] ? a[j] * a[j++] : 0);
    }

    public int punishmentNumber(int n) {
        return pre[n];    
    }
}
```

__Python__:

```Python
class Solution:
    def punishmentNumber(self, n: int) -> int:
        return sum(x ** 2 for x in nums[:pos]) if (pre := list(accumulate(nums := [0, 1, 9, 10, 36, 45, 55, 82, 91, 99, 100, 235, 297, 369, 370, 379, 414, 657, 675, 703, 756, 792, 909, 918, 945, 964, 990, 991, 999, 1000]))) and (pos := bisect_right(nums, n)) else 0
```
