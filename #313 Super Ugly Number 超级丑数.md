__Description__:
Write a program to find the nth super ugly number.

Super ugly numbers are positive numbers whose all prime factors are in the given prime list primes of size k.

__Example:__

Input: n = 12, primes = [2,7,13,19]
Output: 32 
Explanation: [1,2,4,7,8,13,14,16,19,26,28,32] is the sequence of the first 12 
             super ugly numbers given primes = [2,7,13,19] of size 4.

__Note:__

1 is a super ugly number for any given primes.
The given numbers in primes are in ascending order.
0 < k ≤ 100, 0 < n ≤ 10^6, 0 < primes[i] < 1000.
The nth super ugly number is guaranteed to fit in a 32-bit signed integer.

__题目描述__:
编写一段程序来查找第 n 个超级丑数。

超级丑数是指其所有质因数都是长度为 k 的质数列表 primes 中的正整数。

__示例 :__

输入: n = 12, primes = [2,7,13,19]
输出: 32 
解释: 给定长度为 4 的质数列表 primes = [2,7,13,19]，前 12 个超级丑数序列为：[1,2,4,7,8,13,14,16,19,26,28,32] 。

__说明:__

1 是任何给定 primes 的超级丑数。
 给定 primes 中的数字以升序排列。
0 < k ≤ 100, 0 < n ≤ 10^6, 0 < primes[i] < 1000 。
第 n 个超级丑数确保在 32 位有符整数范围内。

__思路__:
动态规划
参考[LeetCode #264 Ugly Number II 丑数 II](https://www.jianshu.com/p/f6f1f3879733)
1. 设置 primes.size()个指针, 每次选择最小的进行更新 dp中的内容
dp[i] = min(dp[i], dp[pointer[j]] * primes[j], 0 <= j <= primes.size() - 1
时间复杂度O(nm), 空间复杂度O(Max(n, m)), 其中 m为 primes数组的长度
2. 采用小顶堆, 每次不用完全遍历 primes, 可以缩短时间
时间复杂度O(nlgm), 空间复杂度O(Max(n, m)), 其中 m为 primes数组的长度

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int nthSuperUglyNumber(int n, vector<int>& primes) 
    {
        vector<int> dp(n, 1), pointer(primes.size());
        for (int i = 1; i < n; i++)
        {
            int cur = INT_MAX;
            for (int j = 0; j < primes.size(); j++) cur = min(cur, dp[pointer[j]] * primes[j]);
            dp[i] = cur;
            for (int j = 0; j < primes.size(); j++) if (cur == dp[pointer[j]] * primes[j]) ++pointer[j];
        }
        return dp.back();
    }
};
```

__Java__:
```Java
class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {
        int dp[] = new int[n], pointer[] = new int[primes.length], m = primes.length;
        dp[0] = 1;
        for (int i = 1; i < n; i++) {
            int cur = Integer.MAX_VALUE;
            for (int j = 0; j < m; j++) cur = Math.min(cur, dp[pointer[j]] * primes[j]);
            for (int j = 0; j < m; j++) if (cur == dp[pointer[j]] * primes[j]) ++pointer[j];
            dp[i] = cur;
        }
        return dp[n - 1];
    }
}
```

__Python__:
```Python
class Solution:
    def nthSuperUglyNumber(self, n: int, primes: List[int]) -> int:
        dp, s, q = [1], {1}, []
        for i in range(len(primes)):
            heapq.heappush(q, (primes[i], 0, primes[i]))
            s.add(primes[i])
        while len(dp) < n:
            num, i, prime = heapq.heappop(q)
            dp.append(num)
            while num in s:
                i += 1
                num = prime * dp[i]
            s.add(num)
            heapq.heappush(q, (num, i, prime))
        return dp[-1]
```