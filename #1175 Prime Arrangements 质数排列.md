__Description__:
Return the number of permutations of 1 to n so that prime numbers are at prime indices (1-indexed.)

(Recall that an integer is prime if and only if it is greater than 1, and cannot be written as a product of two positive integers both smaller than it.)

Since the answer may be large, return the answer modulo 10^9 + 7.

__Example:__
Example 1:

Input: n = 5
Output: 12
Explanation: For example [1,2,5,4,3] is a valid permutation, but [5,2,3,4,1] is not because the prime number 5 is at index 1.

Example 2:

Input: n = 100
Output: 682289015
 
__Constraints:__

1 <= n <= 100

__题目描述__:
请你帮忙给从 1 到 n 的数设计排列方案，使得所有的「质数」都应该被放在「质数索引」（索引从 1 开始）上；你需要返回可能的方案总数。

让我们一起来回顾一下「质数」：质数一定是大于 1 的，并且不能用两个小于它的正整数的乘积来表示。

由于答案可能会很大，所以请你返回答案 模 mod 10^9 + 7 之后的结果即可。

__示例 :__
示例 1：

输入：n = 5
输出：12
解释：举个例子，[1,2,5,4,3] 是一个有效的排列，但 [5,2,3,4,1] 不是，因为在第二种情况里质数 5 被错误地放在索引为 1 的位置上。

示例 2：

输入：n = 100
输出：682289015
 
__提示：__

1 <= n <= 100

__思路__:
题意是要把质数放在质数位置(下标)上, 非质数放在非质数位置上
1. 打表法, 因为 n <= 100, 直接计算出来查表
时间复杂度O(1), 空间复杂度O(1)
2. 参考[](), 先计算出来小于 n + 1的质数的个数, 分别求质数的个数的全排列数和其他的个数的全排列数的乘积, 记得要取模
时间复杂度O(n ^ 1.5), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int numPrimeArrangements(int n) 
    {
        int prime_count = countPrimes(n + 1);
        return (int)(fac(prime_count) * fac(n - prime_count) % 1000000007);
    }
private:
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
                ++result;
                for (int j = i; j < n; j += i) prime[j] = 1;
            }
        }
        return result;
    }
    
    long fac(int n)
    {
        long result = 1;
        for (int i = 1; i <= n; ++i) result = (result * i) % 1000000007;
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int numPrimeArrangements(int n) {
        int prime_count = countPrimes(n + 1);
        return (int)(fac(prime_count) * fac(n - prime_count) % 1000000007);
    }
    
    private int countPrimes(int n) {
        if (n < 3) return 0;
        int result = 0, prime[] = new int[n];
        for (int i = 2; i < n; i++) 
        {
            if (prime[i] == 1) continue;
            else 
            {
                ++result;
                for (int j = i; j < n; j += i) prime[j] = 1;
            }
        }
        return result;
    }
    
    private long fac(int n) {
        long result = 1;
        for (int i = 1; i <= n; ++i) result = (result * i) % 1000000007;
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def numPrimeArrangements(self, n: int) -> int:
        return [1, 1, 1, 2, 4, 12, 36, 144, 576, 2880, 17280, 86400, 604800, 3628800, 29030400, 261273600, 612735986, 289151874, 180670593, 445364737, 344376809, 476898489, 676578804, 89209194, 338137903, 410206413, 973508979, 523161503, 940068494, 400684877, 13697484, 150672324, 164118783, 610613205, 44103617, 58486801, 462170018, 546040181, 197044608, 320204381, 965722612, 554393872, 77422176, 83910457, 517313696, 36724464, 175182841, 627742601, 715505693, 327193394, 451768713, 263673556, 755921509, 94744060, 600274259, 410695940, 427837488, 541336889, 736149184, 514536044, 125049738, 250895270, 39391803, 772631128, 541031643, 428487046, 567378068, 780183222, 228977612, 448880523, 892906519, 858130261, 622773264, 78238453, 146637981, 918450925, 514800525, 828829204, 243264299, 351814543, 405243354, 909357725, 561463122, 913651722, 732754657, 430788419, 139670208, 938893256, 28061213, 673469112, 448961084, 80392418, 466684389, 201222617, 85583092, 76399490, 500763245, 519081041, 892915734, 75763854, 682289015][n]
```