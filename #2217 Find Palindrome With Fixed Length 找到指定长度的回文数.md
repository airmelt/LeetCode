# 2217 Find Palindrome With Fixed Length 找到指定长度的回文数

__Description:__

Given an integer array `queries` and a __positive__ integer `intLength`, return _an array_ `answer` _where_ `answer[i]` _is either the_ `queries[i] ^ th` _smallest __positive palindrome__ of length_ `intLength` _or_ `-1` _if no such palindrome exists_.

A palindrome is a number that reads the same backwards and forwards. Palindromes cannot have leading zeros.

__Example:__

Example 1:

```text
Input: queries = [1,2,3,4,5,90], intLength = 3
Output: [101,111,121,131,141,999]
Explanation:
The first few palindromes of length 3 are:
101, 111, 121, 131, 141, 151, 161, 171, 181, 191, 202, ...
The 90th palindrome of length 3 is 999.
```

Example 2:

```text
Input: queries = [2,4,6], intLength = 4
Output: [1111,1331,1551]
Explanation:
The first six palindromes of length 4 are:
1001, 1111, 1221, 1331, 1441, and 1551.
```

__Constraints:__

- `1 <= queries.length <= 5 * 10 ^ 4`
- `1 <= queries[i] <= 10 ^ 9`
- `1 <= intLength <= 15`

__题目描述:__

给你一个整数数组 `queries` 和一个 __正__ 整数 `intLength` ，请你返回一个数组 `answer` ，其中 `answer[i]` 是长度为 `intLength` 的 __正回文数__ 中第 `queries[i]` 小的数字，如果不存在这样的回文数，则为 `-1` 。

回文数 指的是从前往后和从后往前读一模一样的数字。回文数不能有前导 0 。

__示例:__

示例 1：

```text
输入：queries = [1,2,3,4,5,90], intLength = 3
输出：[101,111,121,131,141,999]
解释：
长度为 3 的最小回文数依次是：
101, 111, 121, 131, 141, 151, 161, 171, 181, 191, 202, ...
第 90 个长度为 3 的回文数是 999 。
```

示例 2：

```text
输入：queries = [2,4,6], intLength = 4
输出：[1111,1331,1551]
解释：
长度为 4 的前 6 个回文数是：
1001, 1111, 1221, 1331, 1441 和 1551 。
```

__提示：__

- `1 <= queries.length <= 5 * 10 ^ 4`
- `1 <= queries[i] <= 10 ^ 9`
- `1 <= intLength <= 15`

__思路:__

```text
构造回文
不考虑前导 0 第一个回文数为 10...0 镜像构造而来
比如 3 位数 101 111 121 131 141 151 161 171 181 191 202 ...
4 位数 1001 1111 1221 1331 1441 1551 1661 1771 1881 1991 2002 ...
第一个都是 10 翻转得到
第二个是 11 翻转得到
依次类推
最后一个是 99 翻转得到
共计 90 个
对于长度为 intLength 的回文数, 令 base = 10 ^ ((intLength - 1) >> 1), 最多有 9 * base 个回文数
由 10 得到 01
可以用 10 * 10 + 10 % 10 得到
即用类似整除和取余的方法构造回文数
也可以用字符串反转构造
时间复杂度为 O(MN), 空间复杂度为 O(1), 如果不用额外字符串存储中间结果
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<long long> kthPalindrome(vector<int>& queries, int intLength) 
    {
        long long base = (long long)pow(10, (intLength - 1LL) >> 1LL), left = 0LL, right = 0LL;
        vector<long long> result(queries.size(), -1LL);
        for (int i = 0, n = queries.size(); i < n; i++) 
        {
            if (queries[i] <= 9LL * base) 
            {
                left = base + queries[i] - 1LL;
                for (right = (intLength & 1LL) == 1LL ? left / 10LL : left; right > 0LL; right /= 10LL) left = left * 10LL + right % 10LL;
                result[i] = left;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long[] kthPalindrome(int[] queries, int intLength) {
        long result[] = new long[queries.length], base = (long)Math.pow(10, (intLength - 1) >>> 1), left = 0L, right = 0L;
        Arrays.fill(result, -1L);
        for (int i = 0, n = queries.length; i < n; i++) {
            if (queries[i] <= 9L * base) {
                left = base + queries[i] - 1L;
                for (right = (intLength & 1L) == 1L ? left / 10L : left; right > 0L; right /= 10L) left = left * 10L + right % 10L;
                result[i] = left;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def kthPalindrome(self, queries: List[int], intLength: int) -> List[int]:
        result, base = [-1] * len(queries), 10 ** ((intLength - 1) >> 1)
        for i, q in enumerate(queries):
            if q <= 9 * base:
                result[i] = int((s := str(base + q - 1)) + (s[-2::-1] if intLength % 2 else s[::-1]))
        return result
```
