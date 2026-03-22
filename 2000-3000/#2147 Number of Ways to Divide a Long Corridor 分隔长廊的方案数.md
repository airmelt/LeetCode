# 2147 Number of Ways to Divide a Long Corridor 分隔长廊的方案数

__Description:__

Along a long library corridor, there is a line of seats and decorative plants. You are given a __0-indexed__ string `corridor` of length `n` consisting of letters `'S'` and `'P'` where each `'S'` represents a seat and each `'P'` represents a plant.

One room divider has __already__ been installed to the left of index `0`, and __another__ to the right of index `n - 1`. Additional room dividers can be installed. For each position between indices `i - 1` and `i` ( `1 <= i <= n - 1`), at most one divider can be installed.

Divide the corridor into non-overlapping sections, where each section has exactly two seats with any number of plants. There may be multiple ways to perform the division. Two ways are different if there is a position with a room divider installed in the first way but not in the second way.

Return _the number of ways to divide the corridor_. Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`. If there is no way, return `0`.

__Example:__

Example 1:

![2147-1](https://assets.leetcode.com/uploads/2021/12/04/1.png)

```text
Input: corridor = "SSPPSPS"
Output: 3
Explanation: There are 3 different ways to divide the corridor.
The black bars in the above image indicate the two room dividers already installed.
Note that in each of the ways, each section has exactly two seats.
```

Example 2:

![2147-2](https://assets.leetcode.com/uploads/2021/12/04/2.png)

```text
Input: corridor = "PPSPSP"
Output: 1
Explanation: There is only 1 way to divide the corridor, by not installing any additional dividers.
Installing any would create some section that does not have exactly two seats.
```

Example 3:

![2147-3](https://assets.leetcode.com/uploads/2021/12/12/3.png)

```text
Input: corridor = "S"
Output: 0
Explanation: There is no way to divide the corridor because there will always be a section that does not have exactly two seats.
```

__Constraints:__

- `n == corridor.length`
- `1 <= n <= 10 ^ 5`
- `corridor[i]` is either `'S'` or `'P'`.

__题目描述:__

在一个图书馆的长廊里，有一些座位和装饰植物排成一列。给你一个下标从 __0__ 开始，长度为 `n` 的字符串 `corridor` ，它包含字母 `'S'` 和 `'P'` ，其中每个 `'S'` 表示一个座位，每个 `'P'` 表示一株植物。

在下标 `0` 的左边和下标 `n - 1` 的右边 __已经__ 分别各放了一个屏风。你还需要额外放置一些屏风。每一个位置 `i - 1` 和 `i` 之间（ `1 <= i <= n - 1`），至多能放一个屏风。

请你将走廊用屏风划分为若干段，且每一段内都 恰好有两个座位 ，而每一段内植物的数目没有要求。可能有多种划分方案，如果两个方案中有任何一个屏风的位置不同，那么它们被视为 不同 方案。

请你返回划分走廊的方案数。由于答案可能很大，请你返回它对 `10 ^ 9 + 7` __取余__ 的结果。如果没有任何方案，请返回 `0` 。

__示例:__

示例 1：

![2147-4](https://assets.leetcode.com/uploads/2021/12/04/1.png)

```text
输入：corridor = "SSPPSPS"
输出：3
解释：总共有 3 种不同分隔走廊的方案。
上图中黑色的竖线表示已经放置好的屏风。
上图每种方案中，每一段都恰好有 两个 座位。
```

示例 2：

![2147-5](https://assets.leetcode.com/uploads/2021/12/04/2.png)

```text
输入：corridor = "PPSPSP"
输出：1
解释：只有 1 种分隔走廊的方案，就是不放置任何屏风。
放置任何的屏风都会导致有一段无法恰好有 2 个座位。
```

示例 3：

![2147-6](https://assets.leetcode.com/uploads/2021/12/12/3.png)

```text
输入：corridor = "S"
输出：0
解释：没有任何方案，因为总是有一段无法恰好有 2 个座位。
```

__提示：__

- `n == corridor.length`
- `1 <= n <= 10 ^ 5`
- `corridor[i]` 要么是 `'S'` ，要么是 `'P'` 。

__思路:__

```text
数学
记录上一个椅子的位置为 pre
根据乘法原理
两个椅子之间的所有空格都可以插入一个屏风
所以屏风的方案等于所有 i - pre 的乘积
其中需要保证椅子为大于 2 的奇数
最后需要检查椅子为正偶数
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfWays(string corridor) 
    {
        long long result = 1LL, MOD = 1e9 + 7, count = 0L;
        for (int i = 0, pre = 0, n = corridor.size(); i < n; i++) 
        {
            if (corridor[i] == 'S') 
            {
                ++count;
                if (count > 1 and count & 1) result = result * (i - pre) % MOD;
                pre = i;
            }
        }
        return count and !(count & 1) ? result : 0;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfWays(String corridor) {
        long result = 1L, MOD = 1_000_000_007L, count = 0L;
        for (int i = 0, pre = 0, n = corridor.length(); i < n; i++) {
            if (corridor.charAt(i) == 'S') {
                ++count;
                if (count > 1 && (count & 1) == 1) result = result * (i - pre) % MOD;
                pre = i;
            }
        }
        return count > 1 && (count & 1) == 0 ? (int)result : 0;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfWays(self, corridor: str) -> int:
        result, count, pre, MOD = 1, 0, 0, 10 ** 9 + 7
        for i, c in enumerate(corridor):
            if c == 'S':
                count += 1
                if count > 1 and count & 1:
                    result = result * (i - pre) % MOD
                pre = i
        return result if count and not (count & 1) else 0
```
