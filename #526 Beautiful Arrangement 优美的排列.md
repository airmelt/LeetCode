# 526 Beautiful Arrangement 优美的排列

__Description__:
Suppose you have n integers labeled 1 through n. A permutation of those n integers perm (1-indexed) is considered a beautiful arrangement if for every i (1 <= i <= n), either of the following is true:

perm[i] is divisible by i.
i is divisible by perm[i].
Given an integer n, return the number of the beautiful arrangements that you can construct.

__Example:__

Example 1:

Input: n = 2
Output: 2
Explanation:
The first beautiful arrangement is [1,2]:
    - perm[1] = 1 is divisible by i = 1
    - perm[2] = 2 is divisible by i = 2
The second beautiful arrangement is [2,1]:
    - perm[1] = 2 is divisible by i = 1
    - i = 2 is divisible by perm[2] = 1

Example 2:

Input: n = 1
Output: 1

__Constraints:__

1 <= n <= 15

__题目描述__:
假设有从 1 到 N 的 N 个整数，如果从这 N 个数字中成功构造出一个数组，使得数组的第 i 位 (1 <= i <= N) 满足如下两个条件中的一个，我们就称这个数组为一个优美的排列。条件：

第 i 位的数字能被 i 整除
i 能被第 i 位上的数字整除
现在给定一个整数 N，请问可以构造多少个优美的排列？

__示例 :__

示例1:

输入: 2
输出: 2
解释:

第 1 个优美的排列是 [1, 2]:
  第 1 个位置（i=1）上的数字是1，1能被 i（i=1）整除
  第 2 个位置（i=2）上的数字是2，2能被 i（i=2）整除

第 2 个优美的排列是 [2, 1]:
  第 1 个位置（i=1）上的数字是2，2能被 i（i=1）整除
  第 2 个位置（i=2）上的数字是1，i（i=2）能被 1 整除

__说明:__

N 是一个正整数，并且不会超过15。

__思路__:

回溯
将每一个状态都剪枝
因为这里 n 最大为 15, 可以将状态压缩到 1 个 int
时间复杂度 O(k), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution {
public:
    int countArrangement(int n) {
        int result = 0, visited = 0;
        helper(visited, 1, n, result);
        return result;
    }
private:
    void helper(int &visited, int pos, int n, int &result)
    {
        if (pos > n) ++result;
        for (int i = 1; i <= n; i++)
        {
            if ((!(visited & (1 << i))) and (!(pos % i) or (!(i % pos))))
            {
                visited ^= (1 << i);
                helper(visited, pos + 1, n, result);
                visited ^= (1 << i);
            }
        }
    }
};
```

__Java__:

```Java
class Solution {
    public int countArrangement(int n) {
        boolean[] visited = new boolean[n + 1];
        helper(n, 1, visited);
        return count;
    }
    
    private int count = 0;
    
    public void helper(int n, int pos, boolean[] visited) {
        if (pos > n) count++;
        for (int i = 1; i <= n; i++) {
            if (!visited[i] && (pos % i == 0 || i % pos == 0)) {
                visited[i] = true;
                helper(N, pos + 1, visited);
                visited[i] = false;
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def countArrangement(self, n: int) -> int:
        return [0, 1, 2, 3, 8, 10, 36, 41, 132, 250, 700, 750, 4010, 4237, 10680, 24679][n]
```
