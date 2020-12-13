__Description__:
Given an array of integers A and let n to be its length.

Assume Bk to be an array obtained by rotating the array A k positions clock-wise, we define a "rotation function" F on A as follow:

F(k) = 0 * Bk[0] + 1 * Bk[1] + ... + (n-1) * Bk[n-1].

Calculate the maximum value of F(0), F(1), ..., F(n-1).

__Note:__
n is guaranteed to be less than 10^5.

__Example:__

A = [4, 3, 2, 6]

F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6) = 0 + 3 + 4 + 18 = 25
F(1) = (0 * 6) + (1 * 4) + (2 * 3) + (3 * 2) = 0 + 4 + 6 + 6 = 16
F(2) = (0 * 2) + (1 * 6) + (2 * 4) + (3 * 3) = 0 + 6 + 8 + 9 = 23
F(3) = (0 * 3) + (1 * 2) + (2 * 6) + (3 * 4) = 0 + 2 + 12 + 12 = 26

So the maximum value of F(0), F(1), F(2), F(3) is F(3) = 26.

__题目描述__:
给定一个长度为 n 的整数数组 A 。

假设 Bk 是数组 A 顺时针旋转 k 个位置后的数组，我们定义 A 的“旋转函数” F 为：

F(k) = 0 * Bk[0] + 1 * Bk[1] + ... + (n-1) * Bk[n-1]。

计算F(0), F(1), ..., F(n-1)中的最大值。

__注意:__
可以认为 n 的值小于 10^5。

__示例 :__

A = [4, 3, 2, 6]

F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6) = 0 + 3 + 4 + 18 = 25
F(1) = (0 * 6) + (1 * 4) + (2 * 3) + (3 * 2) = 0 + 4 + 6 + 6 = 16
F(2) = (0 * 2) + (1 * 6) + (2 * 4) + (3 * 3) = 0 + 6 + 8 + 9 = 23
F(3) = (0 * 3) + (1 * 2) + (2 * 6) + (3 * 4) = 0 + 2 + 12 + 12 = 26

所以 F(0), F(1), F(2), F(3) 中的最大值是 F(3) = 26 。

__思路__:
数学法
移动数组元素可以看作将 [0, 1, 2, ..., n - 1]依次轮转
f(0) = 0 * A[0] + 1 * A[1] + ... + (n - 1) * A[n - 1]
f(n) = f(n - 1) - sum(A) + n * A[n - 1]
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int maxRotateFunction(vector<int>& A) 
    {
        long F = 0, sum = 0, n = A.size();
        for (int i = 0; i < n; i++) 
        {
            sum += A[i];
            F += i * A[i];
        }
        int result = F;
        for (int i = 1; i < n; i++) 
        {
            F += sum - n * A[n-i];
            result = max(result, (int)F);
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int maxRotateFunction(int[] A) {
        int F = 0, sum = 0, n = A.length;
        for (int i = 0; i < n; i++) {
            sum += A[i];
            F += i * A[i];
        }
        int result = F;
        for (int i = 1; i < n; i++) {
            F += sum - n * A[n-i];
            result = Math.max(result, F);
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def maxRotateFunction(self, A: List[int]) -> int:
        result, s, n, f = -float('inf'), sum(A), len(A), sum(i * A[i] for i in range(len(A)))
        for i in range(1, n):
            result = max(result, f)
            f += n * A[i - 1] - s
        return max(result, f)
```