# 1735 Count Ways to Make Array With Product 生成乘积数组的方案数

__Description:__

You are given a 2D integer array, `queries`. For each `queries[i]`, where `queries[i] = [ni, ki]`, find the number of different ways you can place positive integers into an array of size `ni` such that the product of the integers is `ki`. As the number of ways may be too large, the answer to the `i ^ th` query is the number of ways __modulo__ `10 ^ 9 + 7`.

Return _an integer array_ `answer` _where_ `answer.length == queries.length`_, and_ `answer[i]` _is the answer to the_ `i ^ th` _query._

__Example:__

Example 1:

```text
Input: queries = [[2,6],[5,1],[73,660]]
Output: [4,1,50734910]
Explanation: Each query is independent.
[2,6]: There are 4 ways to fill an array of size 2 that multiply to 6: [1,6], [2,3], [3,2], [6,1].
[5,1]: There is 1 way to fill an array of size 5 that multiply to 1: [1,1,1,1,1].
[73,660]: There are 1050734917 ways to fill an array of size 73 that multiply to 660. 1050734917 modulo 109 + 7 = 50734910.
```

Example 2:

```text
Input: queries = [[1,1],[2,2],[3,3],[4,4],[5,5]]
Output: [1,2,3,10,5]
```

__Constraints:__

- `1 <= queries.length <= 10 ^ 4`
- `1 <= ni, ki <= 10 ^ 4`

__题目描述:__

给你一个二维整数数组 `queries` ，其中 `queries[i] = [ni, ki]` 。第 `i` 个查询 `queries[i]` 要求构造长度为 `ni` 、每个元素都是正整数的数组，且满足所有元素的乘积为 `ki` ，请你找出有多少种可行的方案。由于答案可能会很大，方案数需要对 `10 ^ 9 + 7` __取余__ 。

请你返回一个整数数组 `answer`，满足 `answer.length == queries.length` ，其中 `answer[i]`是第 `i` 个查询的结果。

__示例:__

示例 1：

```text
输入：queries = [[2,6],[5,1],[73,660]]
输出：[4,1,50734910]
解释：每个查询之间彼此独立。
[2,6]：总共有 4 种方案得到长度为 2 且乘积为 6 的数组：[1,6]，[2,3]，[3,2]，[6,1]。
[5,1]：总共有 1 种方案得到长度为 5 且乘积为 1 的数组：[1,1,1,1,1]。
[73,660]：总共有 1050734917 种方案得到长度为 73 且乘积为 660 的数组。1050734917 对 109 + 7 取余得到 50734910 。
```

示例 2 ：

```text
输入：queries = [[1,1],[2,2],[3,3],[4,4],[5,5]]
输出：[1,2,3,10,5]
```

__提示：__

- `1 <= queries.length <= 10 ^ 4`
- `1 <= ni, ki <= 10 ^ 4`

__思路:__

```text
数学
先将查询的数字进行质因数分解
对于每个质因数 p, 设其出现次数为 r, 则对于长度为 n 的数组, 有 n + r - 1 个空位, 从中选出 r 个空位放入 p, 其余空位放入 1(对应该质因数, 放入 0 个), 即隔板法
所以对于每个查询实际上求的是 C(n + r - 1, r), 最后求所有的组合数的乘积
在计算 C(n + r - 1, r) 时, 由于 n + r - 1 和 r 都很大, 所以不能直接计算, 而是先计算分子分母的阶乘, 然后再计算阶乘的乘法逆元
即将除以 r! 的乘法转化为乘以 r! 的乘法逆元
可以用快速幂和费马小定理求乘法逆元
时间复杂度为 O(N(K ^ 1 / 2 + logK)), 空间复杂度为 O(N), 其中 N 为 queries 的长度, K 为 queries 中的查询的最大值, 对每个查询, 需要计算其质因数分解, 时间复杂度为 O(K ^ 1 / 2), 计算阶乘和阶乘的乘法逆元的时间复杂度为 O(logK)
```

__代码:__

__C++__:

```C++
constexpr int MOD = 1e9 + 7;

int power(int a, int n)
{
    int result = 1;
    for (; n; n >>= 1, a = 1LL * a * a % MOD) if (n & 1) result = 1LL * result * a % MOD;
    return result;
}

int inv(int n)
{
    return power(n, MOD - 2);
}

int C(int m, int n)
{
    int result = 1;
    for (int i = 0; i < n; i++) result = 1LL * result * (m - i) % MOD;
    for (int i = 1; i <= n; i++) result = 1LL * result * inv(i) % MOD;
    return result;
}

class Solution 
{
public:
    vector<int> waysToFillArray(vector<vector<int>>& queries) 
    {
        vector<int> result;
        for (const auto& query : queries)
        {
            int n = query.front(), k = query.back(), cur = 1;
            for (int i = 2, r; i * i <= k; i++)
            {
                if (!(k % i))
                {
                    for (r = 0; !(k % i); k /= i) ++r;
                    cur = 1LL * cur * C(n + r - 1, r) % MOD;
                }
            }
            if (k > 1) cur = 1LL * cur * n % MOD;
            result.emplace_back(cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private static List<Integer> primes = new ArrayList<>();
    private static int N = 10030, M = 20;
    private static long MOD = 1_000_000_007L;
    private static long[][] C = new long[N][M];

    static {
        boolean prime[] = new boolean[N];
        for (int p = 2; p * p < N; p++) if (!prime[p]) for (int i = (p << 1); i < N; i += p) prime[i] = true;
        for (int i = 2; i < N; i++) if (!prime[i]) primes.add(i);
        for (int i = 0; i < N; i++) for (int j = 0; j < Math.min(i + 1, M); j++) C[i][j] = (j == 0 || j == i) ? 1 : (C[i - 1][j - 1] + C[i - 1][j]) % MOD;
    }

    public int[] waysToFillArray(int[][] queries) {
        int n = queries.length, result[] = new int[n];
        for (int i = 0; i < n; i++) {
            long cur = 1L;
            for (int prime : primes) {
                int times = 0;
                while (queries[i][1] % prime == 0) {
                    queries[i][1] /= prime;
                    ++times;
                }
                cur = (cur * C[queries[i][0] + times - 1][times] % MOD);
            }
            result[i] = (int)cur;
        }
        return result;
    }
}
```

__Python__:

```Python
MOD = 10 ** 9 + 7

@lru_cache(None)
def factors(n):
    if n == 1: 
        return defaultdict(int)
    for i in range(2, int(n ** 0.5)+1):
        if n % i == 0:
            cur = deepcopy(factors(n // i))
            cur[i] += 1
            return cur
    result = defaultdict(int)
    result[n] = 1
    return result

@lru_cache(None)
def cache_comb(m, n):
    return comb(m, n) % MOD

class Solution:
    def waysToFillArray(self, queries: List[List[int]]) -> List[int]:
        result = []
        for x, y in queries:
            cur, count = list(factors(y).values()), 1
            for i in cur:
                count *= cache_comb(i + x - 1, i) % MOD
            result.append(count % MOD)
        return result
```
