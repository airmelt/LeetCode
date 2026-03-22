# 2350 Shortest Impossible Sequence of Rolls 不可能得到的最短骰子序列

__Description:__

You are given an integer array `rolls` of length `n` and an integer `k`. You roll a `k` sided dice numbered from `1` to `k`, `n` times, where the result of the `i ^ th` roll is `rolls[i]`.

Return _the length of the __shortest__ sequence of rolls so that there's no such subsequence in_ `rolls`.

A __sequence of rolls__ of length `len` is the result of rolling a `k` sided dice `len` times.

__Example:__

Example 1:

```text
Input: rolls = [4,2,1,2,3,3,2,4,1], k = 4
Output: 3
Explanation: Every sequence of rolls of length 1, [1], [2], [3], [4], can be taken from rolls.
Every sequence of rolls of length 2, [1, 1], [1, 2], ..., [4, 4], can be taken from rolls.
The sequence [1, 4, 2] cannot be taken from rolls, so we return 3.
Note that there are other sequences that cannot be taken from rolls.
```

Example 2:

```text
Input: rolls = [1,1,2,2], k = 2
Output: 2
Explanation: Every sequence of rolls of length 1, [1], [2], can be taken from rolls.
The sequence [2, 1] cannot be taken from rolls, so we return 2.
Note that there are other sequences that cannot be taken from rolls but [2, 1] is the shortest.
```

Example 3:

```text
Input: rolls = [1,1,3,2,2,2,3,3], k = 4
Output: 1
Explanation: The sequence [4] cannot be taken from rolls, so we return 1.
Note that there are other sequences that cannot be taken from rolls but [4] is the shortest.
```

__Constraints:__

- `n == rolls.length`
- `1 <= n <= 10 ^ 5`
- `1 <= rolls[i] <= k <= 10 ^ 5`

__题目描述:__

给你一个长度为 `n` 的整数数组 `rolls` 和一个整数 `k` 。你扔一个 `k` 面的骰子 `n` 次，骰子的每个面分别是 `1` 到 `k` ，其中第 `i` 次扔得到的数字是 `rolls[i]` 。

请你返回 __无法__ 从 `rolls` 中得到的 __最短__ 骰子子序列的长度。

扔一个 `k` 面的骰子 `len` 次得到的是一个长度为 `len` 的 __骰子子序列__ 。

注意 ，子序列只需要保持在原数组中的顺序，不需要连续。

__示例:__

示例 1：

```text
输入：rolls = [4,2,1,2,3,3,2,4,1], k = 4
输出：3
解释：所有长度为 1 的骰子子序列 [1] ，[2] ，[3] ，[4] 都可以从原数组中得到。
所有长度为 2 的骰子子序列 [1, 1] ，[1, 2] ，... ，[4, 4] 都可以从原数组中得到。
子序列 [1, 4, 2] 无法从原数组中得到，所以我们返回 3 。
还有别的子序列也无法从原数组中得到。
```

示例 2：

```text
输入：rolls = [1,1,2,2], k = 2
输出：2
解释：所有长度为 1 的子序列 [1] ，[2] 都可以从原数组中得到。
子序列 [2, 1] 无法从原数组中得到，所以我们返回 2 。
还有别的子序列也无法从原数组中得到，但 [2, 1] 是最短的子序列。
```

示例 3：

```text
输入：rolls = [1,1,3,2,2,2,3,3], k = 4
输出：1
解释：子序列 [4] 无法从原数组中得到，所以我们返回 1 。
还有别的子序列也无法从原数组中得到，但 [4] 是最短的子序列。
```

__提示：__

- `n == rolls.length`
- `1 <= n <= 10 ^ 5`
- `1 <= rolls[i] <= k <= 10 ^ 5`

__思路:__

```text
贪心
注意到 rolls 中的数字是 1 - k
构造子序列
只要保证每次都能取到 1 组 1 - k
就能构成一组可以得到的子序列
初始化 result = 1
用一个 set 记录出现过的数字
当 set 中的数字个数等于 k 时, result += 1, 并清空 set
或者标记每个数字出现的次数
遍历 rolls, 如果 mark[x] < result, 则 mark[x] = result
left 用来记录一组中还未出现的数字个数
也就是说每次出现完整的一组数字后, left = k, result += 1
时间复杂度为 O(N), 空间复杂度为 O(K)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int shortestSequence(vector<int>& rolls, int k) 
    {
        int result = 1, left = k;
        vector<int> mark(k + 1);
        for (int x : rolls) 
        {
            if (mark[x] < result) 
            {
                mark[x] = result;
                if (!(--left)) 
                {
                    left = k;
                    ++result;
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int shortestSequence(int[] rolls, int k) {
        int mark[] = new int[k + 1], result = 1, left = k;
        for (int x : rolls) {
            if (mark[x] < result) {
                mark[x] = result;
                if (--left == 0) {
                    left = k;
                    ++result;
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def shortestSequence(self, rolls: List[int], k: int) -> int:
        s, result = set(), 1
        for x in rolls:
            if x not in s:
                s.add(x)
            if len(s) == k:
                result += 1
                s.clear()
        return result
```
