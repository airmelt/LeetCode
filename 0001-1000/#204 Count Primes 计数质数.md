# 204 Count Primes 计数质数

__Description__:
Count the number of prime numbers less than a non-negative number, n.

**Example:**

Input: 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.

__题目描述__:
统计所有小于非负整数 n 的质数的数量。

**示例：**

输入: 10
输出: 4
解释: 小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。

__思路__:

[埃拉托斯特尼筛法 Sieve_of_Eratosthenes-wiki](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)
筛选不大于根号 n的所有素数的倍数
比如10, 找到第一个素数2, 去掉 2的倍数 4, 6, 8; 第二个素数3, 去掉 3的倍数 6, 9, 还剩下的数 2, 3, 5, 7就是小于 10的素数
时间复杂度O(n ^ 1.5), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int countPrimes(int n) 
    {
        if (n < 3) return 0;
        int result = 0;
        vector<int> prime(n);
        for (int i = 2; i < n; i++) 
        {
            if (prime[i]) continue;
            else 
            {
                result++;
                for (int j = i; j < n; j += i) prime[j] = 1;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countPrimes(int n) {
        if (n < 3) return 0;
        boolean[] prime = new boolean[n];
        int result = n / 2;
        for (int i = 3; i * i < n; i += 2) {
            if (prime[i]) continue;
            for (int j = i * i; j < n; j += i * 2) {
                if (!prime[j]) {
                    prime[j] = true;
                    result--;
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
    def countPrimes(self, n: int) -> int:
        if n < 3:
            return 0
        else:
            result = [0] + [0] + [1] * (n - 2)
            for i in range(2, int(n ** 0.5) + 1):
                if result[i] == 1:
                    result[i * i:n:i] = [0] * len(result[i * i:n:i])
        return sum(result)
```
