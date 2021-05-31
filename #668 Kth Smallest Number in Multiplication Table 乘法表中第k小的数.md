# 668 Kth Smallest Number in Multiplication Table 乘法表中第k小的数

__Description__:
Nearly everyone has used the Multiplication Table. The multiplication table of size m x n is an integer matrix mat where mat[i][j] == i * j (1-indexed).

Given three integers m, n, and k, return the kth smallest element in the m x n multiplication table.

__Example:__

Example 1:

![multtable1-grid](https://assets.leetcode.com/uploads/2021/05/02/multtable1-grid.jpg)

Input: m = 3, n = 3, k = 5
Output: 3
Explanation: The 5th smallest number is 3.

Example 2:

![multtable2-grid](https://assets.leetcode.com/uploads/2021/05/02/multtable2-grid.jpg)

Input: m = 2, n = 3, k = 6
Output: 6
Explanation: The 6th smallest number is 6.

__Constraints:__

1 <= m, n <= 3 \* 10^4
1 <= k <= m \* n

__题目描述__:
几乎每一个人都用 乘法表。但是你能在乘法表中快速找到第k小的数字吗？

给定高度m 、宽度n 的一张 m * n的乘法表，以及正整数k，你需要返回表中第k 小的数字。

__示例 :__

例 1：

输入: m = 3, n = 3, k = 5
输出: 3
解释:
乘法表:

```text
1   2   3
2   4   6
3   6   9
```

第5小的数字是 3 (1, 2, 2, 3, 3).

例 2：

输入: m = 2, n = 3, k = 6
输出: 6
解释:
乘法表:

```text
1   2   3
2   4   6
```

第6小的数字是 6 (1, 2, 2, 3, 4, 6).

__注意:__

m 和 n 的范围在 [1, 30000] 之间。
k 的范围在 [1, m * n] 之间。

__思路__:

二分查找
乘法表每一行均为有序数组, 尝试使用二分查找
边界为 left = 1, right = m * n, 查找区间为 [left, right], 注意到 mid = (left + right) / 2, 不一定在乘法表中, 最后一定要使得 left == right == mid, 再返回 left(right), 所以循环退出的条件为 left == right
比如 [a1, a2, ..., ax, ..., an] 需要取到 ax, 那么依照题意 ax 之前的数组元素个数一定都小于 k, ax 之后的数组元素个数一定都大于等于 k, 所以更新的时候如果统计的数组个数小于 k, 则 left = mid + 1, 否则 right = mid
统计数组元素的个数可以一行一行的统计, 一行最多取 n 个元素, 最少取 mid / i 个元素
时间复杂度 O(mlgmn), 空间复杂度 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findKthNumber(int m, int n, int k) 
    {
        int left = 1, right = m * n;
        while (left < right) 
        {
            int mid = left + ((right - left) >> 1), count = 0;
            for (int i = 1; i <= m; ++i) count += min(mid / i, n);
            if (k > count) left = mid + 1;
            else right = mid;
        }
        return right;
    }
};
```

__Java__:

```Java
class Solution {
    public int findKthNumber(int m, int n, int k) {
        int left = 1, right = m * n;
        while (left < right) {
            int mid = left + ((right - left) >>> 1);
            if (!enough(mid, m, n, k)) left = mid + 1;
            else right = mid;
        }
        return left;
    }
    
    private boolean enough(int mid, int m, int n, int k) {
        int count = 0;
        for (int i = 1; i < m + 1; i++) count += Math.min(mid / i, n);
        return count >= k;
    }
}
```

__Python__:

```Python
class Solution:
    def findKthNumber(self, m: int, n: int, k: int) -> int:
        left, right = 1, m * n
        while left < right:
            mid = left + ((right - left) >> 1)
            if sum(min(mid // i, n) for i in range(1, m + 1)) < k:
                left = mid + 1
            else:
                right = mid
        return left
```
