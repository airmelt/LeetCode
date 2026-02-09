# 2896 Apply Operations to Make Two Strings Equal 执行操作使两个字符串相等

__Description:__

You are given two __0-indexed__ binary strings `s1` and `s2`, both of length `n`, and a positive integer `x`.

You can perform any of the following operations on the string `s1` __any__ number of times:

- Choose two indices `i` and `j`, and flip both `s1[i]` and `s1[j]`. The cost of this operation is `x`.
- Choose an index `i` such that `i < n - 1` and flip both `s1[i]` and `s1[i + 1]`. The cost of this operation is `1`.

Return _the __minimum__ cost needed to make the strings_ `s1` _and_ `s2` _equal, or return_ `-1` _if it is impossible._

__Note__ that flipping a character means changing it from `0` to `1` or vice-versa.

__Example:__

Example 1:

```text
Input: s1 = "1100011000", s2 = "0101001010", x = 2
Output: 4
Explanation: We can do the following operations:
```

- Choose i = 3 and apply the second operation. The resulting string is s1 = "1101111000".
- Choose i = 4 and apply the second operation. The resulting string is s1 = "1101001000".
- Choose i = 0 and j = 8 and apply the first operation. The resulting string is s1 = "0101001010" = s2.

The total cost is 1 + 1 + 2 = 4. It can be shown that it is the minimum cost possible.

Example 2:

```text
Input: s1 = "10110", s2 = "00011", x = 4
Output: -1
Explanation: It is not possible to make the two strings equal.
```

__Constraints:__

- `n == s1.length == s2.length`
- `1 <= n, x <= 500`
- `s1` and `s2` consist only of the characters `'0'` and `'1'`.

__题目描述:__

给你两个下标从 __0__ 开始的二进制字符串 `s1` 和 `s2` ，两个字符串的长度都是 `n` ，再给你一个正整数 `x` 。

你可以对字符串 `s1` 执行以下操作 __任意次__ :

- 选择两个下标 `i` 和 `j` ，将 `s1[i]` 和 `s1[j]` 都反转，操作的代价为 `x` 。
- 选择满足 `i < n - 1` 的下标 `i` ，反转 `s1[i]` 和 `s1[i + 1]` ，操作的代价为 `1` 。

请你返回使字符串 `s1` 和 `s2` 相等的 __最小__ 操作代价之和，如果无法让二者相等，返回 `-1` 。

__注意__ ，反转字符的意思是将 `0` 变成 `1` ，或者 `1` 变成 `0` 。

__示例:__

示例 1：

```text
输入：s1 = "1100011000", s2 = "0101001010", x = 2
输出：4
解释：我们可以执行以下操作：
```

- 选择 i = 3 执行第二个操作。结果字符串是 s1 = "1101111000" 。
- 选择 i = 4 执行第二个操作。结果字符串是 s1 = "1101001000" 。
- 选择 i = 0 和 j = 8 ，执行第一个操作。结果字符串是 s1 = "0101001010" = s2 。

总代价是 1 + 1 + 2 = 4 。这是最小代价和。

示例 2：

```text
输入：s1 = "10110", s2 = "00011", x = 4
输出：-1
解释：无法使两个字符串相等。
```

__提示：__

- `n == s1.length == s2.length`
- `1 <= n, x <= 500`
- `s1` 和 `s2` 只包含字符 `'0'` 和 `'1'` 。

__思路:__

```text
动态规划
只需要处理 s1[i] != s2[i] 的部分
如果 s1 == s2 直接返回 0
如果不相等的部分 need 长度为奇数无论如何也不能转换成功, 返回 -1
否则设 dp[i] 表示反转 need 前 i 个需要的代价
如果使用第一种交换方式那么 dp[i] = dp[i - 1] + x / 2
如果使用第二种交换方式需要从 need[i - 1] 到 need[i] 全部相邻字符都交换一次, 总代价为 need[i] - need[i - 1]
所以 dp[i] = dp[i - 2] + need[i] - need[i - 1]
为了简化计算可以先将所有值都乘上 2
dp2[i] = min(dp2[i - 1] + x, dp2[i - 2] + ((need[i] - need[i - 1]) << 1))
最后返回 dp2[-1] >> 1
注意到 dp[i] 只和 dp[i - 1], dp[i - 2] 相关
所以可以用滚动数组将空间复杂度优化为 O(1)
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(string s1, string s2, int x) 
    {
        if (s1 == s2) return 0;
        vector<int> need;
        for (int i = 0, n = s1.size(); i < n; i++) if (s1[i] != s2[i]) need.emplace_back(i);
        int m = need.size(), a = 0, b = x, t = 0;
        if (m & 1) return -1;
        for (int i = 1; i < m; i++) 
        {
            t = b;
            b = min(b + x, a + ((need[i] - need[i - 1]) << 1));
            a = t;
        }
        return b >> 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(String s1, String s2, int x) {
        if (s1.equals(s2)) return 0;
        var need = new ArrayList<Integer>();
        for (int i = 0, n = s1.length(); i < n; i++) if (s1.charAt(i) != s2.charAt(i)) need.add(i);
        int m = need.size(), a = 0, b = x, t = 0;
        if ((m & 1) == 1) return -1;
        for (int i = 1; i < m; i++) {
            t = b;
            b = Math.min(b + x, a + ((need.get(i) - need.get(i - 1)) << 1));
            a = t;
        }
        return b >> 1;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, s1: str, s2: str, x: int) -> int:
        if s1 == s2:
            return 0
        if len(need := [i for i, (a, b) in enumerate(zip(s1, s2)) if a != b]) & 1:
            return -1
        a, b = 0, x
        for i, j in pairwise(need):
            a, b = b, min(b + x, a + ((j - i) << 1))
        return b >> 1
```
