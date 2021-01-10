# 440 K-th Smallest in Lexicographical Order 字典序的第K小数字

__Description__:
Given integers n and k, find the lexicographically k-th smallest integer in the range from 1 to n.

__Note:__
1 ≤ k ≤ n ≤ 10^9.

__Example:__

Input:
n: 13   k: 2

Output:
10

Explanation:
The lexicographical order is [1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9], so the second smallest number is 10.

__题目描述__:
给定整数 n 和 k，找到 1 到 n 中字典序第 k 小的数字。

__注意：__
1 ≤ k ≤ n ≤ 10^9。

__示例 :__

输入:
n: 13   k: 2

输出:
10

解释:
字典序的排列是 [1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9]，所以第二小的数字是 10。

__思路__:

十叉树
根节点为 1, 每个节点最多有 10个子节点, 按照 0-9的顺序排列
记录 result为当前的前缀, 比如 10的前缀为 1
查找当前节点的子树的节点数量
然后判断是否在当前节点的子树中, 如果不是, 向前移动前缀
时间复杂度O(lgnlgk), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findKthNumber(int n, int k) 
    {
        int result = 1;
        --k;
        while (k) 
        {
            long step = 0, first = result, last = result + 1;
            while (first <= n)
            {
                step += min((long)n + 1, last) - first;
                first *= 10;
                last *= 10;
            }
            if (step <= k)
            {
                k -= step;
                ++result;
            }
            else
            {
                --k;
                result *= 10;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findKthNumber(int n, int k) {
        int result = 1;
        --k;
        while (k > 0) {
            long step = 0, first = result, last = result + 1;
            while (first <= n) {
                step += Math.min((long)n + 1, last) - first;
                first *= 10;
                last *= 10;
            }
            if (step <= k) {
                ++result;
                k -= step;
            } else {
                result *= 10;
                --k;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findKthNumber(self, n: int, k: int) -> int:
        result = 1
        k -= 1
        while k:
            step, first, last = 0, result, result + 1
            while first <= n:
                step += min(n + 1, last) - first
                first *= 10
                last *= 10
            if step <= k:
                result += 1
                k -= step
            else:
                result *= 10
                k -= 1
        return result
```
