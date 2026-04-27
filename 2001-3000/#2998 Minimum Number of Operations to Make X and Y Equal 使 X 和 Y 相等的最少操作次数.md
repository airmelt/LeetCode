# 2998 Minimum Number of Operations to Make X and Y Equal 使 X 和 Y 相等的最少操作次数

__Description:__

You are given two positive integers `x` and `y`.

In one operation, you can do one of the four following operations:

1. Divide `x` by `11` if `x` is a multiple of `11`.
2. Divide `x` by `5` if `x` is a multiple of `5`.
3. Decrement `x` by `1`.
4. Increment `x` by `1`.

Return _the __minimum__ number of operations required to make_  `x` _and_ `y` equal.

__Example:__

Example 1:

```text
Input: x = 26, y = 1
Output: 3
Explanation: We can make 26 equal to 1 by applying the following operations: 
```

1. Decrement x by 1
2. Divide x by 5
3. Divide x by 5

It can be shown that 3 is the minimum number of operations required to make 26 equal to 1.

Example 2:

```text
Input: x = 54, y = 2
Output: 4
Explanation: We can make 54 equal to 2 by applying the following operations: 
```

1. Increment x by 1
2. Divide x by 11
3. Divide x by 5
4. Increment x by 1

It can be shown that 4 is the minimum number of operations required to make 54 equal to 2.

Example 3:

```text
Input: x = 25, y = 30
Output: 5
Explanation: We can make 25 equal to 30 by applying the following operations: 
```

1. Increment x by 1
2. Increment x by 1
3. Increment x by 1
4. Increment x by 1
5. Increment x by 1

It can be shown that 5 is the minimum number of operations required to make 25 equal to 30.

__Constraints:__

- `1 <= x, y <= 10 ^ 4`

__题目描述:__

给你两个正整数 `x` 和 `y` 。

一次操作中，你可以执行以下四种操作之一：

1. 如果 `x` 是 `11` 的倍数，将 `x` 除以 `11` 。
2. 如果 `x` 是 `5` 的倍数，将 `x` 除以 `5` 。
3. 将 `x` 减 `1` 。
4. 将 `x` 加 `1` 。

请你返回让 `x` 和 `y` 相等的 __最少__ 操作次数。

__示例:__

示例 1：

```text
输入：x = 26, y = 1
输出：3
解释：我们可以通过以下操作将 26 变为 1 ：
```

1. 将 x 减 1
2. 将 x 除以 5
3. 将 x 除以 5

将 26 变为 1 最少需要 3 次操作。

示例 2：

```text
输入：x = 54, y = 2
输出：4
解释：我们可以通过以下操作将 54 变为 2 ：
```

1. 将 x 加 1
2. 将 x 除以 11
3. 将 x 除以 5
4. 将 x 加 1

将 54 变为 2 最少需要 4 次操作。

示例 3：

```text
输入：x = 25, y = 30
输出：5
解释：我们可以通过以下操作将 25 变为 30 ：
```

1. 将 x 加 1
2. 将 x 加 1
3. 将 x 加 1
4. 将 x 加 1
5. 将 x 加 1

将 25 变为 30 最少需要 5 次操作。

__提示：__

- `1 <= x, y <= 10 ^ 4`

__思路:__

```text
记忆化搜索
当 x <= y 时只能用加 1 操作, 返回 y - x
其他情况
要么减 1 直到 x == y, 需要 x - y 次操作
要么加 1 到达一个能整除 11 的数
这需要加上 11 - x % 11 再加上一次操作次数变为 x / 11 + 1
要么减 1 到达一个能整除 11 的数
这需要减去 x % 11 再加上一次操作次数变为 x / 11
除以 5 的类似
记录下来所有路径
如果出现过直接返回
时间复杂度为 O(log ^ 2 (X / Y)), 空间复杂度为 O(log ^ 2 (X / Y))
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumOperationsToMakeEqual(int x, int y) 
    {
        if (x <= y) return y - x;
        if (memo.count(x)) memo[x];
        return memo[x] = min({ x - y, minimumOperationsToMakeEqual(x / 11, y) + x % 11 + 1, minimumOperationsToMakeEqual(x / 11 + 1, y) + 11 - x % 11 + 1, minimumOperationsToMakeEqual(x / 5, y) + x % 5 + 1, minimumOperationsToMakeEqual(x / 5 + 1, y) + 5 - x % 5 + 1 });
    }
private:
    unordered_map<int, int> memo;
};
```

__Java__:

```Java
class Solution {
    private final Map<Integer, Integer> memo = new HashMap<>();
    
    public int minimumOperationsToMakeEqual(int x, int y) {
        if (x <= y) return y - x;
        if (memo.containsKey(x)) memo.get(x);
        int result = Math.min(Math.min(Math.min(Math.min(x - y, minimumOperationsToMakeEqual(x / 11, y) + x % 11 + 1), minimumOperationsToMakeEqual(x / 11 + 1, y) + 11 - x % 11 + 1), minimumOperationsToMakeEqual(x / 5, y) + x % 5 + 1), minimumOperationsToMakeEqual(x / 5 + 1, y) + 5 - x % 5 + 1);
        memo.put(x, result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    @lru_cache(None)
    def minimumOperationsToMakeEqual(self, x: int, y: int) -> int:
        return min(x - y, self.minimumOperationsToMakeEqual(x // 11, y) + x % 11 + 1, self.minimumOperationsToMakeEqual(x // 11 + 1, y) + 11 - x % 11 + 1, self.minimumOperationsToMakeEqual(x // 5, y) + x % 5 + 1, self.minimumOperationsToMakeEqual(x // 5 + 1, y) + 5 - x % 5 + 1) if x > y else y - x
```
