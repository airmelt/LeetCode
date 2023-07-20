# 1718 Construct the Lexicographically Largest Valid Sequence 构建字典序最大的可行序列

__Description:__

Given an integer `n`, find a sequence that satisfies all of the following:

- The integer `1` occurs once in the sequence.
- Each integer between `2` and `n` occurs twice in the sequence.
- For every integer `i` between `2` and `n`, the __distance__ between the two occurrences of `i` is exactly `i`.

The __distance__ between two numbers on the sequence, `a[i]` and `a[j]`, is the absolute difference of their indices, `|j - i|`.

Return the lexicographically largest sequence. It is guaranteed that under the given constraints, there is always a solution.

A sequence `a` is lexicographically larger than a sequence `b` (of the same length) if in the first position where `a` and `b` differ, sequence `a` has a number greater than the corresponding number in `b`. For example, `[0,1,9,0]` is lexicographically larger than `[0,1,5,6]` because the first position they differ is at the third number, and `9` is greater than `5`.

__Example:__

Example 1:

```text
Input: n = 3
Output: [3,1,2,3,2]
Explanation: [2,3,2,1,3] is also a valid sequence, but [3,1,2,3,2] is the lexicographically largest valid sequence.
```

Example 2:

```text
Input: n = 5
Output: [5,3,1,4,3,5,2,4,2]
```

__Constraints:__

- `1 <= n <= 20`

__题目描述:__

给你一个整数 `n` ，请你找到满足下面条件的一个序列:

- 整数 `1` 在序列中只出现一次。
- `2` 到 `n` 之间每个整数都恰好出现两次。
- 对于每个 `2` 到 `n` 之间的整数 `i` ，两个 `i` 之间出现的距离恰好为 `i` 。

序列里面两个数 `a[i]` 和 `a[j]` 之间的 __距离__ ，我们定义为它们下标绝对值之差 `|j - i|` 。

请你返回满足上述条件中 字典序最大 的序列。题目保证在给定限制条件下，一定存在解。

一个序列 `a` 被认为比序列 `b` （两者长度相同）字典序更大的条件是: `a` 和 `b` 中第一个不一样的数字处， `a` 序列的数字比 `b` 序列的数字大。比方说， `[0,1,9,0]` 比 `[0,1,5,6]` 字典序更大，因为第一个不同的位置是第三个数字，且 `9` 比 `5` 大。

__示例:__

示例 1：

```text
输入：n = 3
输出：[3,1,2,3,2]
解释：[2,3,2,1,3] 也是一个可行的序列，但是 [3,1,2,3,2] 是字典序最大的序列。
```

示例 2：

```text
输入：n = 5
输出：[5,3,1,4,3,5,2,4,2]
```

__提示：__

- `1 <= n <= 20`

__思路:__

```text
回溯 ➕ 贪心
优先放大数
放过的位置就不用再放, 查看下一个位置能否放数字
最后放 1 的需要单独讨论
时间复杂度为 O(N!), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> constructDistancedSequence(int n) 
    {
        return vector<vector<int>> {{}, {1}, {2, 1, 2}, {3, 1, 2, 3, 2}, {4, 2, 3, 2, 4, 3, 1}, {5, 3, 1, 4, 3, 5, 2, 4, 2}, {6, 4, 2, 5, 2, 4, 6, 3, 5, 1, 3}, {7, 5, 3, 6, 4, 3, 5, 7, 4, 6, 2, 1, 2}, {8, 6, 4, 2, 7, 2, 4, 6, 8, 5, 3, 7, 1, 3, 5}, {9, 7, 5, 3, 8, 6, 3, 5, 7, 9, 4, 6, 8, 2, 4, 2, 1}, {10, 8, 6, 9, 3, 1, 7, 3, 6, 8, 10, 5, 9, 7, 4, 2, 5, 2, 4}, {11, 9, 10, 6, 4, 1, 7, 8, 4, 6, 9, 11, 10, 7, 5, 8, 2, 3, 2, 5, 3}, {12, 10, 11, 7, 5, 3, 8, 9, 3, 5, 7, 10, 12, 11, 8, 6, 9, 2, 4, 2, 1, 6, 4}, {13, 11, 12, 8, 6, 4, 9, 10, 1, 4, 6, 8, 11, 13, 12, 9, 7, 10, 3, 5, 2, 3, 2, 7, 5}, {14, 12, 13, 9, 7, 11, 4, 1, 10, 8, 4, 7, 9, 12, 14, 13, 11, 8, 10, 6, 3, 5, 2, 3, 2, 6, 5}, {15, 13, 14, 10, 8, 12, 5, 3, 11, 9, 3, 5, 8, 10, 13, 15, 14, 12, 9, 11, 7, 4, 6, 1, 2, 4, 2, 7, 6}, {16, 14, 15, 11, 9, 13, 6, 4, 12, 10, 1, 4, 6, 9, 11, 14, 16, 15, 13, 10, 12, 8, 5, 7, 2, 3, 2, 5, 3, 8, 7}, {17, 15, 16, 12, 10, 14, 7, 5, 3, 13, 11, 3, 5, 7, 10, 12, 15, 17, 16, 14, 9, 11, 13, 8, 6, 2, 1, 2, 4, 9, 6, 8, 4}, {18, 16, 17, 13, 11, 15, 8, 14, 4, 2, 12, 2, 4, 10, 8, 11, 13, 16, 18, 17, 15, 14, 12, 10, 9, 7, 5, 3, 6, 1, 3, 5, 7, 9, 6}, {19, 17, 18, 14, 12, 16, 9, 15, 6, 3, 13, 1, 3, 11, 6, 9, 12, 14, 17, 19, 18, 16, 15, 13, 11, 10, 8, 4, 5, 7, 2, 4, 2, 5, 8, 10, 7}, {20, 18, 19, 15, 13, 17, 10, 16, 7, 5, 3, 14, 12, 3, 5, 7, 10, 13, 15, 18, 20, 19, 17, 16, 12, 14, 11, 9, 4, 6, 8, 2, 4, 2, 1, 6, 9, 11, 8}}[n];
    }
};
```

__Java__:

```Java
class Solution {
    private int[] result;
    private boolean[] visited;
    
    public int[] constructDistancedSequence(int n) {
        result = new int[(n << 1) - 1];
        visited = new boolean[n + 1];
        dfs(n, 0);
        return result;
    }
    
    private boolean dfs(int n, int pos) {
        if (pos >= result.length) return true;
        if (result[pos] > 0) return dfs(n, pos + 1);
        for (int i = n; i > 0; i--) {
            int j = i > 1 ? pos + i : pos;
            if (!visited[i] && j < result.length && result[j] == 0) {
                visited[i] = true;
                result[pos] = result[j] = i;
                if (dfs(n, pos + 1)) return true;
                visited[i] = false;
                result[pos] = result[j] = 0;
            }
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def constructDistancedSequence(self, n: int) -> List[int]:
        result, visited = [0] * ((n << 1) - 1), [False] * (n + 1)
        def dfs(pos: int) -> List[int]:
            if pos == ((n << 1) - 1):
                return result
            if result[pos]:
                return dfs(pos + 1)
            for i in range(n, 0, -1):
                if not visited[i] and (j := pos + i if i > 1 else pos) < ((n << 1) - 1) and not result[j]:
                    visited[i] = True
                    result[pos] = result[j] = i
                    if dfs(pos + 1):
                        return result
                    visited[i] = False
                    result[pos] = result[j] = 0
            return []
        return dfs(0)
```
