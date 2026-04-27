# 1830 Minimum Number of Operations to Make String Sorted 使字符串有序的最少操作次数

__Description:__

You are given a string `s` (__0-indexed__)​​​​​​. You are asked to perform the following operation on `s`​​​​​​ until you get a sorted string:

Return _the number of operations needed to make the string sorted._ Since the answer can be too large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: s = "cba"
Output: 5
Explanation: The simulation goes as follows:
Operation 1: i=2, j=2. Swap s[1] and s[2] to get s="cab", then reverse the suffix starting at 2. Now, s="cab".
Operation 2: i=1, j=2. Swap s[0] and s[2] to get s="bac", then reverse the suffix starting at 1. Now, s="bca".
Operation 3: i=2, j=2. Swap s[1] and s[2] to get s="bac", then reverse the suffix starting at 2. Now, s="bac".
Operation 4: i=1, j=1. Swap s[0] and s[1] to get s="abc", then reverse the suffix starting at 1. Now, s="acb".
Operation 5: i=2, j=2. Swap s[1] and s[2] to get s="abc", then reverse the suffix starting at 2. Now, s="abc".
```

Example 2:

```text
Input: s = "aabaa"
Output: 2
Explanation: The simulation goes as follows:
Operation 1: i=3, j=4. Swap s[2] and s[4] to get s="aaaab", then reverse the substring starting at 3. Now, s="aaaba".
Operation 2: i=4, j=4. Swap s[3] and s[4] to get s="aaaab", then reverse the substring starting at 4. Now, s="aaaab".
```

__Constraints:__

- `1 <= s.length <= 3000`
- `s`​​​​​​ consists only of lowercase English letters.

__题目描述:__

给你一个字符串 `s` （__下标从 0 开始__）。你需要对 `s` 执行以下操作直到它变为一个有序字符串:

请你返回将字符串变成有序的最少操作次数。由于答案可能会很大，请返回它对 `10 ^ 9 + 7` __取余__ 的结果。

__示例:__

示例 1：

```text
输入：s = "cba"
输出：5
解释：模拟过程如下所示：
操作 1：i=2，j=2。交换 s[1] 和 s[2] 得到 s="cab" ，然后反转下标从 2 开始的后缀字符串，得到 s="cab" 。
操作 2：i=1，j=2。交换 s[0] 和 s[2] 得到 s="bac" ，然后反转下标从 1 开始的后缀字符串，得到 s="bca" 。
操作 3：i=2，j=2。交换 s[1] 和 s[2] 得到 s="bac" ，然后反转下标从 2 开始的后缀字符串，得到 s="bac" 。
操作 4：i=1，j=1。交换 s[0] 和 s[1] 得到 s="abc" ，然后反转下标从 1 开始的后缀字符串，得到 s="acb" 。
操作 5：i=2，j=2。交换 s[1] 和 s[2] 得到 s="abc" ，然后反转下标从 2 开始的后缀字符串，得到 s="abc" 。
```

示例 2：

```text
输入：s = "aabaa"
输出：2
解释：模拟过程如下所示：
操作 1：i=3，j=4。交换 s[2] 和 s[4] 得到 s="aaaab" ，然后反转下标从 3 开始的后缀字符串，得到 s="aaaba" 。
操作 2：i=4，j=4。交换 s[3] 和 s[4] 得到 s="aaaab" ，然后反转下标从 4 开始的后缀字符串，得到 s="aaaab" 。
```

示例 3：

```text
输入：s = "cdbea"
输出：63
```

示例 4：

```text
输入：s = "leetcodeleetcodeleetcode"
输出：982157772
```

__提示：__

- `1 <= s.length <= 3000`
- `s`​ 只包含小写英文字母。

__思路:__

```text
组合数学
实际上就是求 s 在所有排列中从小到大的位置
所有排列的总数为 n! / (c0! * c1! * ... * cn!), 其中 n 为字符串 s 的长度, c0, c1, ... cn 为字符串 s 各字符的数量
记阶乘数组 fac[i] = i! % MOD
为了快速计算阶乘的除法可以使用乘法逆元 fac_inv[i] = pow(fac[i], mod - 2) % MOD
对于 s[i], 统计小于 s[i] 的组合数
具体来说, 先求出比 s[i] 小的字符的总数 sum, 字符数可以用哈希表或者数组记录
那么从 s[i] 到 s[n - 1] 共计 n - i - 1 个位置的排列总数为 sum * fac[n - i - 1]
考虑到重复的需要乘上所有的 fac_inv[c](即除以这些阶乘数)
将上述值加到结果中, 字符数量自减即可
时间复杂度为 O(N), 空间复杂度为 O(N), 字符数 26 和取模均为常数不计入
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int makeStringSorted(string s) 
    {
        int n = s.size(), result = 0;
        vector<int> fac(n + 1, 1), fac_inv(n + 1, 1), freq(26);
        ++freq[s[0] - 'a'];
        for (int i = 1; i < n; i++)
        {
            fac[i] = (long long)fac[i - 1] * i % MOD;
            fac_inv[i] = quick_pow(fac[i], MOD - 2);
            ++freq[s[i] - 'a'];
        }
        for (int i = 0; i < n - 1; i++)
        {
            int cur = 0, m = s[i] - 'a';
            for (int j = 0; j < m; j++) cur += freq[j];
            cur = (long long)cur * fac[n - i - 1] % MOD;
            for (int j = 0; j < 26; j++) cur = (long long)cur * fac_inv[freq[j]] % MOD;
            result = (result + cur) % MOD;
            --freq[m];
        }
        return result % MOD;
    }
private:
    static constexpr int MOD = 1e9 + 7;

    int quick_pow(int a, int n)
    {
        int result = 1;
        while (n)
        {
            if (n & 1) result = (long long) result * a % MOD;
            a = (long long)a * a % MOD;
            n >>= 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int makeStringSorted(String s) {
        long MOD = 1_000_000_007L, result = 0L, n = s.length(), fac[] = new long[(int)n + 1], facInv[] = new long[(int)n + 1];
        fac[0] = facInv[0] = 1L;
        Map<Integer, Integer> map = new HashMap<>();
        map.put(s.charAt(0) - 'a', 1);
        for (int i = 1; i < n; i++) {
            fac[i] = fac[i - 1] * i % MOD;
            facInv[i] = pow(fac[i], MOD - 2L, MOD);
            map.merge(s.charAt(i) - 'a', 1, Integer::sum);
        }
        for (int i = 0; i < n - 1; i++) {
            long cur = 0, c = s.charAt(i) - 'a';
            for (Map.Entry<Integer, Integer> entry : map.entrySet()) if (entry.getKey() < (int)c) cur += entry.getValue();
            cur = cur * fac[(int)n - i - 1] % MOD;
            for (Map.Entry<Integer, Integer> entry : map.entrySet()) cur = cur * facInv[entry.getValue()] % MOD;
            result = (result + cur) % MOD;
            map.merge((int)c, -1, Integer::sum);
        }
        return (int)(result % MOD);
    }

    private long pow(long a, long n, long p) {
        long result = 1;
        while (n > 0) {
            if ((n & 1) != 0) result = (a * result) % p;
            a = (a * a) % p;
            n >>= 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def makeStringSorted(self, s: str) -> int:
        MOD, freq, result, fac, fac_inv = 10 ** 9 + 7, Counter(s), 0, [1] + [0] * (n := len(s)), [1] + [0] * n
        for i in range(1, n):
            fac[i], fac_inv[i] = fac[i - 1] * i % MOD, pow(fac[i - 1] * i, MOD - 2, MOD)
        for i in range(n - 1):
            cur = sum(v for c, v in freq.items() if c < s[i]) * fac[n - i - 1]
            for v in freq.values():
                cur = cur * fac_inv[v] % MOD
            result += cur
            freq[s[i]] -= 1
        return result % MOD
```
