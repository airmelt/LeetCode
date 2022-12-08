# 1414 Find the Minimum Number of Fibonacci Numbers Whose Sum Is K 和为K的最少斐波那契数字数目

__Description:__

Given an integer `k`, _return the minimum number of Fibonacci numbers whose sum is equal to_ `k`. The same Fibonacci number can be used multiple times.

The Fibonacci numbers are defined as:

- `F1 = 1`
- `F2 = 1`
- `Fn = Fn-1 + Fn-2` for `n > 2.`

__Example:__

Example 1:

```text
Input: k = 7
Output: 2 
Explanation: The Fibonacci numbers are: 1, 1, 2, 3, 5, 8, 13, ... 
For k = 7 we can use 2 + 5 = 7.
```

Example 2:

```text
Input: k = 10
Output: 2 
Explanation: For k = 10 we can use 2 + 8 = 10.
```

Example 3:

```text
Input: k = 19
Output: 3 
Explanation: For k = 19 we can use 1 + 5 + 13 = 19.
```

__Constraints:__

- `1 <= k <= 10 ^ 9`

__题目描述:__

给你数字 `k` ，请你返回和为 `k` 的斐波那契数字的最少数目，其中，每个斐波那契数字都可以被使用多次。

斐波那契数字定义为：

- F1 = 1
- F2 = 1
- Fn = Fn-1 + Fn-2 ， 其中 n > 2 。

数据保证对于给定的 `k` ，一定能找到可行解。

__示例:__

示例 1：

```text
输入：k = 7
输出：2 
解释：斐波那契数字为：1，1，2，3，5，8，13，……
对于 k = 7 ，我们可以得到 2 + 5 = 7 。
```

示例 2：

```text
输入：k = 10
输出：2 
解释：对于 k = 10 ，我们可以得到 2 + 8 = 10 。
```

示例 3：

```text
输入：k = 19
输出：3 
解释：对于 k = 19 ，我们可以得到 1 + 5 + 13 = 19 。
```

__提示：__

- `1 <= k <= 10 ^ 9`

__思路:__

```text
贪心
求出不大于 k 的斐波拉契数组
从最后一个数开始一直尝试加入
直到 k 为 0
注意用除法和取余代替减法
时间复杂度为 O(logN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findMinFibonacciNumbers(int k) 
    {
        vector<int> f{1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597, 2584, 4181, 6765, 10946, 17711, 28657, 46368, 75025, 121393, 196418, 317811, 514229, 832040, 1346269, 2178309, 3524578, 5702887, 9227465, 14930352, 24157817, 39088169, 63245986, 102334155, 165580141, 267914296, 433494437, 701408733};
        int result = 0, i = f.size() - 1;
        while (k > 0) 
        {
            result += k / f[i];
            k %= f[i--];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findMinFibonacciNumbers(int k) {
        int f[] = new int[]{1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597, 2584, 4181, 6765, 10946, 17711, 28657, 46368, 75025, 121393, 196418, 317811, 514229, 832040, 1346269, 2178309, 3524578, 5702887, 9227465, 14930352, 24157817, 39088169, 63245986, 102334155, 165580141, 267914296, 433494437, 701408733}, result = 0, i = f.length - 1;
        while (k > 0) {
            result += k / f[i];
            k %= f[i--];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findMinFibonacciNumbers(self, k: int) -> int:
        f, i, result = deque([1, 1]), -1, 0
        while f[-1] + f[-2] < 10 ** 9:
            f.append(f[-1] + f[-2])
        while k:
            result += k // f[i]
            k %= f[i]
            i -= 1
        return result
```
