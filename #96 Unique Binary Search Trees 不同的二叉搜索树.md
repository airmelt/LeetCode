__Description__:
Given n, how many structurally unique BST's (binary search trees) that store values 1 ... n?

__Example:__

Input: 3
Output: 5
Explanation:
Given n = 3, there are a total of 5 unique BST's:
```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

__题目描述__:
给定一个整数 n，求以 1 ... n 为节点组成的二叉搜索树有多少种？

__示例 :__

输入: 3
输出: 5
解释:
给定 n = 3, 一共有 5 种不同结构的二叉搜索树:
```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

__思路__:
卡特兰数
时间复杂度O(n), 空间复杂度O(1)
直接用数学公式可以降到 O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int numTrees(int n) 
    {
        return (new int[]{1, 1, 2, 5, 14, 42, 132, 429, 1430, 4862, 16796, 58786, 208012, 742900, 2674440, 9694845, 35357670, 129644790, 477638700, 1767263190})[n];
    }
};
```

__Java__:
```Java
class Solution {
    public int numTrees(int n) {
        return (new int[]{1, 1, 2, 5, 14, 42, 132, 429, 1430, 4862, 16796, 58786, 208012, 742900, 2674440, 9694845, 35357670, 129644790, 477638700, 1767263190})[n];
    }
}
```

__Python__:
```Python
class Solution:
    def numTrees(self, n: int) -> int:
        return self.numTrees(n - 1) * (4 * n - 2) // (n + 1) if n > 1 else 1
```