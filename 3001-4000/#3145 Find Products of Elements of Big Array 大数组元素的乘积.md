# 3145 Find Products of Elements of Big Array 大数组元素的乘积

__Description:__

The __powerful array__ of a non-negative integer `x` is defined as the shortest sorted array of powers of two that sum up to `x`. The table below illustrates examples of how the __powerful array__ is determined. It can be proven that the powerful array of `x` is unique.

| num | Binary Representation | powerful array |
| - | --------------------- | ----- |
| 1 | 00001 | [1] |
| 8 | 01000 | [8] |
| 10 | 01010 | [2, 8] |
| 13 | 01101 | [1, 4, 8] |
| 23 | 10111 | [1, 2, 4, 16] |

The array `big_nums` is created by concatenating the __powerful arrays__ for every positive integer `i` in ascending order: 1, 2, 3, and so on. Thus, `big_nums` begins as `[1, 2, 1, 2, 4, 1, 4, 2, 4, 1, 2, 4, 8, ...]`.

You are given a 2D integer matrix `queries`, where for `queries[i] = [from_i, to_i, mod_i]` you should calculate `(big_nums[from_i] * big_nums[from_i + 1] * ... * big_nums[to_i]) % mod_i`.

Return an integer array `answer` such that `answer[i]` is the answer to the `i_th` query.

__Example:__

Example 1:

```text
Input: queries = [[1,3,7]]

Output: [4]

Explanation:

There is one query.

big_nums[1..3] = [2,1,2]. The product of them is 4. The result is 4 % 7 = 4.
```

Example 2:

```text
Input: queries = [[2,5,3],[7,7,4]]

Output: [2,2]

Explanation:

There are two queries.

First query: big_nums[2..5] = [1,2,4,1]. The product of them is 8. The result is 8 % 3 = 2.

Second query: big_nums[7] = 2. The result is 2 % 4 = 2.
```

__Constraints:__

- `1 <= queries.length <= 500`
- `queries[i].length == 3`
- `0 <= queries[i][0] <= queries[i][1] <= 10 ^ 15`
- `1 <= queries[i][2] <= 10 ^ 5`

__题目描述:__

一个非负整数 `x` 的 __强数组__ 指的是满足元素为 2 的幂且元素总和为 `x` 的最短有序数组。下表说明了如何确定 __强数组__ 的示例。可以证明，`x` 对应的强数组是独一无二的。

| 数字 | 二进制表示 | 强数组 |
| - | --------------------- | ----- |
| 1 | 00001 | [1] |
| 8 | 01000 | [8] |
| 10 | 01010 | [2, 8] |
| 13 | 01101 | [1, 4, 8] |
| 23 | 10111 | [1, 2, 4, 16] |

我们将每一个升序的正整数 `i` （即1，2，3等等）的 __强数组__ 连接得到数组 `big_nums` `，big_nums` 开始部分为 `[1, 2, 1, 2, 4, 1, 4, 2, 4, 1, 2, 4, 8, ...]` 。

给你一个二维整数数组 `queries` ，其中 `queries[i] = [from_i, to_i, mod_i]` ，你需要计算 `(big_nums[from_i] * big_nums[from_i + 1] * ... * big_nums[to_i]) % mod_i` 。

请你返回一个整数数组 `answer` ，其中 `answer[i]` 是第 `i` 个查询的答案。

__示例:__

示例 1：

```text
输入：queries = [[1,3,7]]

输出：[4]

解释：

只有一个查询。

big_nums[1..3] = [2,1,2] 。它们的乘积为 4。结果为 4 % 7 = 4。
```

示例 2：

```text
输入：queries = [[2,5,3],[7,7,4]]

输出：[2,2]

解释：

有两个查询。

第一个查询：big_nums[2..5] = [1,2,4,1] 。它们的乘积为 8 。结果为  8 % 3 = 2。

第二个查询：big_nums[7] = 2 。结果为 2 % 4 = 2。
```

__提示：__

- `1 <= queries.length <= 500`
- `queries[i].length == 3`
- `0 <= queries[i][0] <= queries[i][1] <= 10 ^ 15`
- `1 <= queries[i][2] <= 10 ^ 5`

__思路:__

```text
位运算
可以使用前缀和的思想
分别计算 1, 2, 4, 8 的强数组
可以发现 [0, 1] 一共有 1 个 2 ^ 0, [0, 3] 一共有 2 个 2 ^ 1, [0, 7] 一共有 3 个 2 ^ 2
所以 [0, n - 1], n 为 2 的幂次 (n = 2 ^ i) 的时候, 一共有 i * 2 ^ (i - 1) 个 2 的幂
为了找到最大的 i 使得 i * 2 ^ (i - 1) 的和刚好为 total
可以找到第一个不大于 (total + 1).bit_length() - 1 的 i
这个 i 满足 i * 2 ^ (i - 1) 不大于 total
总和为 i * (i - 1) * 2 ^ (i - 1)
累加上 i 的个数和 1 的个数
不足的部分再用 n 的 low_bit 补齐
即可计算出 2 的幂次数
然后使用快速幂计算出乘积
时间复杂度为 O(NlogM), 空间复杂度为 O(1), 其中 M 为 queries 最大值, 本题不超过 10 ^ 15
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findProductsOfElements(vector<vector<long long>>& queries) 
    {
        vector<int> result;
        auto helper = [](long long total) -> long long
        {
            long long result = 0LL, cur = 0LL, n = 0LL, c = 0LL, s = 0LL;
            for (long long i = __lg(total + 1LL); i > -1LL; i--)
            {
                if ((cur = (c << i) + (i << i >> 1LL)) <= total)
                {
                    total -= cur;
                    result += (s << i) + ((i * (i - 1) >> 1LL) << i >> 1LL);
                    s += i;
                    ++c;
                    n |= 1LL << i;
                }
            }
            while (total--)
            {
                result += __builtin_ctzll(n);
                n &= n - 1LL;
            }
            return result;
        };
        auto quick_pow = [](long long a, long long n, long long mod) -> int
        {
            long long result = 1LL % mod;
            for (; n; n >>= 1LL)
            {
                if (n & 1LL) result = result * a % mod;
                a = a * a % mod;
            }
            return result;
        };
        for (const auto& query : queries) result.emplace_back(quick_pow(2, helper(query[1] + 1) - helper(query[0]), query[2]));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findProductsOfElements(long[][] queries) {
        int n = queries.length, result[] = new int[n];
        for (int i = 0; i < n; i++) result[i] = quickPow(2, helper(queries[i][1] + 1) - helper(queries[i][0]), queries[i][2]);
        return result;
    }

    private long helper(long total) {
        long result = 0L, n = 0L, c = 0L, s = 0L, cur = 0L;
        for (long i = 63L - Long.numberOfLeadingZeros(total + 1L); i > -1L; i--) {
            if ((cur = (c << i) + (i << i >>> 1L)) <= total) {
                total -= cur;
                result += (s << i) + (((i * (i - 1L)) >>> 1L) << i >>> 1L);
                s += i;
                ++c;
                n |= 1L << i;
            }
        }
        while (total-- > 0) {
            result += Long.numberOfTrailingZeros(n);
            n &= n - 1L;
        }
        return result;
    }

    private int quickPow(long a, long n, long mod) {
        long result = 1L % mod;
        for (; n > 0; n >>>= 1L) {
            if ((n & 1L) == 1L) result = result * a % mod;
            a = a * a % mod;
        }
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def findProductsOfElements(self, queries: List[List[int]]) -> List[int]:
        def helper(total: int) -> int:
            result = n = c = s = 0
            for i in range((total + 1).bit_length() - 1, -1, -1):
                if (cur := (c << i) + (i << i >> 1)) <= total:
                    total -= cur
                    result += (s << i) + (((i * (i - 1)) >> 1) << i >> 1)
                    s += i
                    c += 1
                    n |= 1 << i
            for _ in range(total):
                result += (low_bit := n & -n).bit_length() - 1
                n ^= low_bit
            return result
        return [pow(2, helper(r + 1) - helper(l), mod) for l, r, mod in queries]
```
