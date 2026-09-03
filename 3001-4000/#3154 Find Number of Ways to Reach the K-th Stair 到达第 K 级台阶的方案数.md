# 3154 Find Number of Ways to Reach the K-th Stair 到达第 K 级台阶的方案数

__Description:__

You are given a __non-negative__ integer `k`. There exists a staircase with an infinite number of stairs, with the __lowest__ stair numbered 0.

Alice has an integer `jump`, with an initial value of 0. She starts on stair 1 and wants to reach stair `k` using any number of __operations__. If she is on stair `i`, in one __operation__ she can:

- Go down to stair `i - 1`. This operation __cannot__ be used consecutively or on stair 0.
- Go up to stair `i + 2 ^ jump`. And then, `jump` becomes `jump + 1`.

Return the _total_ number of ways Alice can reach stair `k`.

__Note__ that it is possible that Alice reaches the stair `k`, and performs some operations to reach the stair `k` again.

__Example:__

Example 1:

```text
Input: k = 0

Output: 2

Explanation:

The 2 possible ways of reaching stair 0 are:
```

- Alice starts at stair 1.
  - Using an operation of the first type, she goes down 1 stair to reach stair 0.
- Alice starts at stair 1.
  - Using an operation of the first type, she goes down 1 stair to reach stair 0.
  - Using an operation of the second type, she goes up 20 stairs to reach stair 1.
  - Using an operation of the first type, she goes down 1 stair to reach stair 0.

Example 2:

```text
Input: k = 1

Output: 4

Explanation:

The 4 possible ways of reaching stair 1 are:
```

- Alice starts at stair 1. Alice is at stair 1.
- Alice starts at stair 1.
  - Using an operation of the first type, she goes down 1 stair to reach stair 0.
  - Using an operation of the second type, she goes up 20 stairs to reach stair 1.
- Alice starts at stair 1.
  - Using an operation of the second type, she goes up 20 stairs to reach stair 2.
  - Using an operation of the first type, she goes down 1 stair to reach stair 1.
- Alice starts at stair 1.
  - Using an operation of the first type, she goes down 1 stair to reach stair 0.
  - Using an operation of the second type, she goes up 20 stairs to reach stair 1.
  - Using an operation of the first type, she goes down 1 stair to reach stair 0.
  - Using an operation of the second type, she goes up 21 stairs to reach stair 2.
  - Using an operation of the first type, she goes down 1 stair to reach stair 1.

__Constraints:__

- `1 <= k <= 10 ^ 9`

__题目描述:__

给你有一个 __非负__ 整数 `k` 。有一个无限长度的台阶，__最低__ 一层编号为 `0` 。

Alice 有一个整数 `jump` ，一开始值为 0 。Alice 从台阶 1 开始，可以使用 __任意__ 次操作，目标是到达第 `k` 级台阶。假设 Alice 位于台阶 `i` ，一次 __操作__ 中，Alice 可以：

- 向下走一级到 `i - 1` ，但该操作 __不能__ 连续使用，如果在台阶第 0 级也不能使用。
- 向上走到台阶 `i + 2 ^ jump` 处，然后 `jump` 变为 `jump + 1` 。

请你返回 Alice 到达台阶 `k` 处的总方案数。

__注意__，Alice 可能到达台阶 `k` 处后，通过一些操作重新回到台阶 `k` 处，这视为不同的方案。

__示例:__

示例 1：

```text
输入：k = 0

输出：2

解释：

2 种到达台阶 0 的方案为：
```

- Alice 从台阶 1 开始。
  - 执行第一种操作，从台阶 1 向下走到台阶 0 。
- Alice 从台阶 1 开始。
  - 执行第一种操作，从台阶 1 向下走到台阶 0 。
  - 执行第二种操作，向上走 20 级台阶到台阶 1 。
  - 执行第一种操作，从台阶 1 向下走到台阶 0 。

示例 2：

```text
输入：k = 1

输出：4

解释：

4 种到达台阶 1 的方案为：
```

- Alice 从台阶 1 开始，已经到达台阶 1 。
- Alice 从台阶 1 开始。
  - 执行第一种操作，从台阶 1 向下走到台阶 0 。
  - 执行第二种操作，向上走 20 级台阶到台阶 1 。
- Alice 从台阶 1 开始。
  - 执行第二种操作，向上走 20 级台阶到台阶 2 。
  - 执行第一种操作，向下走 1 级台阶到台阶 1 。
- Alice 从台阶 1 开始。
  - 执行第一种操作，从台阶 1 向下走到台阶 0 。
  - 执行第二种操作，向上走 20 级台阶到台阶 1 。
  - 执行第一种操作，向下走 1 级台阶到台阶 0 。
  - 执行第二种操作，向上走 21 级台阶到台阶 2 。
  - 执行第一种操作，向下走 1 级台阶到台阶 1 。

__提示：__

- `1 <= k <= 10 ^ 9`

__思路:__

```text
1. 动态规划
设 dp[i][pre][jump] 表示是否可以向下走(pre), 以及当前 jump 的值情况下到达 i 的方案数
初始入口 dp[0][1][0] 表示从 0 出发, jump 为 0, 可以往下跳
当超过 k + 1 时必然不能回到 k, 剪枝返回 0
否则返回当前是否到 k, 往下跳或者往上跳的和
时间复杂度为 O(log ^ 2K), 空间复杂度为 O(log ^ 2K), M 为数组的最大值
2. 数学
由 1 + 2 + ... + 2 ^ (jump - 1) - down = k
即 down = 2 ^ jump - k
由于 down 不能连续使用
所以 down 需要插板法插入每一次 jump 里
就是 C(j + 1, m)
从 0 到 29 枚举 j 求和即可
由于需要 2 ^ jump >= k, down 不能为负数
所以 jump 可以从 log(k) 开始枚举
时间复杂度为 O(1), 空间复杂度为 O(1), 不计入预处理
```

__代码:__

__C++__:

```C++
int c[31][31];

auto init = [] 
{
    for (int i = 0; i < 31; i++) 
    {
        c[i][0] = c[i][i] = 1;
        for (int j = 1; j < i; j++) c[i][j] = c[i - 1][j - 1] + c[i - 1][j];
    }
    return 0;
}();

class Solution 
{
public:
    int waysToReachStair(int k) 
    {
        int result = 0;
        for (int j = k ? __lg(k - 1) + 1 : 0; (1 << j) - k <= j + 1; j++) result += c[j + 1][(1 << j) - k];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private static final int MX = 31;
    private static final int[][] c = new int[MX][MX];

    static {
        for (int i = 0; i < MX; i++) {
            c[i][0] = c[i][i] = 1;
            for (int j = 1; j < i; j++) c[i][j] = c[i - 1][j - 1] + c[i - 1][j];
        }
    }

    public int waysToReachStair(int k) {
        int result = 0;
        for (int j = 32 - Integer.numberOfLeadingZeros(Math.max(k - 1, 0)); (1 << j) - k <= j + 1; j++) result += c[j + 1][(1 << j) - k];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def waysToReachStair(self, k: int) -> int:
        @lru_cache(None)
        def dfs(cur: int, pre: int, jump: int) -> int:
            return 0 if cur > k + 1 else (cur == k) + (pre and cur and dfs(cur - 1, 0, jump)) + dfs(cur + (1 << jump), 1, jump + 1)
        return dfs(1, 1, 0) % (10 ** 9 + 7)
```
