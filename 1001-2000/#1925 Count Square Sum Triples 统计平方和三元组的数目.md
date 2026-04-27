# 1925 Count Square Sum Triples 统计平方和三元组的数目

__Description:__

A __square triple__ `(a,b,c)` is a triple where `a`, `b`, and `c` are __integers__ and `a ^ 2 + b ^ 2 = c ^ 2`.

Given an integer `n`, return _the number of __square triples__ such that_ `1 <= a, b, c <= n`.

__Example:__

Example 1:

```text
Input: n = 5
Output: 2
Explanation: The square triples are (3,4,5) and (4,3,5).
```

Example 2:

```text
Input: n = 10
Output: 4
Explanation: The square triples are (3,4,5), (4,3,5), (6,8,10), and (8,6,10).
```

__Constraints:__

- `1 <= n <= 250`

__题目描述:__

一个 __平方和三元组__ `(a,b,c)` 指的是满足 `a ^ 2 + b ^ 2 = c ^ 2` 的 __整数__ 三元组 `a`， `b` 和 `c` 。

给你一个整数 `n` ，请你返回满足 `1 <= a, b, c <= n` 的 __平方和三元组__ 的数目。

__示例:__

示例 1：

```text
输入：n = 5
输出：2
解释：平方和三元组为 (3,4,5) 和 (4,3,5) 。
```

示例 2：

```text
输入：n = 10
输出：4
解释：平方和三元组为 (3,4,5)，(4,3,5)，(6,8,10) 和 (8,6,10) 。
```

__提示：__

- `1 <= n <= 250`

__思路:__

```text
1. 暴力法
遍历 [1, n] 中的数对, 检查是否满足 a ^ 2 + b ^ 2 = c ^ 2
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 数学
a = k * (m ^ 2 - n ^ 2), b = k * 2 * m * n, c = k * (m ^ 2 + n ^ 2), 其中 m > n, m 和 n 互质, m 和 n 均为奇数
枚举 [1, n ^ 1 / 2] 中满足条件的数对 (m, n), 对 n / (m ^ 2 + n ^ 2) 求和即可
时间复杂度为 O(NlogN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countTriples(int n) 
    {
        int result = 0;
        for (int i = 1; i * i < n; i++) for (int j = i + 1 ; i * i + j * j <= n; j++) if (__gcd(i,j) == 1 && !(i * j & 1)) result += n / (i * i + j * j);
        return result << 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int countTriples(int n) {
        int result = 0;
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 1; i <= n; i++) map.put(i * i, 1);
        for (Integer a : map.keySet()) for (Integer b : map.keySet()) result += map.getOrDefault(a + b, 0);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countTriples(self, n: int) -> int:
        return sum(n // (i * i + j * j) for i in range(1, int(sqrt(n)) + 1) for j in range(i + 1, n) if (i * i + j * j <= n and gcd(i, j) == 1 and not (i * j & 1))) << 1
```
