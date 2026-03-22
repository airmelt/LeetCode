# 2438 Range Product Queries of Powers 二的幂数组中查询范围内的乘积

__Description:__

Given a positive integer `n`, there exists a __0-indexed__ array called `powers`, composed of the __minimum__ number of powers of `2` that sum to `n`. The array is sorted in __non-decreasing__ order, and there is __only one__ way to form the array.

You are also given a __0-indexed__ 2D integer array `queries`, where `queries[i] = [left_i, right_i]`. Each `queries[i]` represents a query where you have to find the product of all `powers[j]` with `left_i <= j <= right_i`.

Return _an array_ `answers`_, equal in length to_ `queries`_, where_ `answers[i]` _is the answer to the_ `i ^ th` _query_. Since the answer to the `i ^ th` query may be too large, each `answers[i]` should be returned __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: n = 15, queries = [[0,1],[2,2],[0,3]]
Output: [2,4,64]
Explanation:
For n = 15, powers = [1,2,4,8]. It can be shown that powers cannot be a smaller size.
Answer to 1st query: powers[0] * powers[1] = 1 * 2 = 2.
Answer to 2nd query: powers[2] = 4.
Answer to 3rd query: powers[0] * powers[1] * powers[2] * powers[3] = 1 * 2 * 4 * 8 = 64.
Each answer modulo 10 ^ 9 + 7 yields the same answer, so [2,4,64] is returned.
```

Example 2:

```text
Input: n = 2, queries = [[0,0]]
Output: [2]
Explanation:
For n = 2, powers = [2].
The answer to the only query is powers[0] = 2. The answer modulo 10 ^ 9 + 7 is the same, so [2] is returned.
```

__Constraints:__

- `1 <= n <= 10 ^ 9`
- `1 <= queries.length <= 10 ^ 5`
- `0 <= start_i <= end_i < powers.length`

__题目描述:__

给你一个正整数 `n` ，你需要找到一个下标从 __0__ 开始的数组 `powers` ，它包含 __最少__ 数目的 `2` 的幂，且它们的和为 `n` 。 `powers` 数组是 __非递减__ 顺序的。根据前面描述，构造 `powers` 数组的方法是唯一的。

同时给你一个下标从 __0__ 开始的二维整数数组 `queries` ，其中 `queries[i] = [left_i, right_i]` ，其中 `queries[i]` 表示请你求出满足 `left_i <= j <= right_i` 的所有 `powers[j]` 的乘积。

请你返回一个数组 `answers` ，长度与 `queries` 的长度相同，其中 `answers[i]`是第 `i` 个查询的答案。由于查询的结果可能非常大，请你将每个 `answers[i]` 都对 `10 ^ 9 + 7` __取余__ 。

__示例:__

示例 1：

```text
输入：n = 15, queries = [[0,1],[2,2],[0,3]]
输出：[2,4,64]
解释：
对于 n = 15 ，得到 powers = [1,2,4,8] 。没法得到元素数目更少的数组。
第 1 个查询的答案：powers[0] * powers[1] = 1 * 2 = 2 。
第 2 个查询的答案：powers[2] = 4 。
第 3 个查询的答案：powers[0] * powers[1] * powers[2] * powers[3] = 1 * 2 * 4 * 8 = 64 。
每个答案对 10 ^ 9 + 7 得到的结果都相同，所以返回 [2,4,64] 。
```

示例 2：

```text
输入：n = 2, queries = [[0,0]]
输出：[2]
解释：
对于 n = 2, powers = [2] 。
唯一一个查询的答案是 powers[0] = 2 。答案对 10 ^ 9 + 7 取余后结果相同，所以返回 [2] 。
```

__提示：__

- `1 <= n <= 10 ^ 9`
- `1 <= queries.length <= 10 ^ 5`
- `0 <= start_i <= end_i < powers.length`

__思路:__

```text
模拟 ➕ 前缀和
先将 n 转化为二进制数
最低位可以用 n & -n 得到
再将最低位置 1 即可
由于 powers 数组比较小, 可以预处理得到前缀和
实际上是前缀积
快速得到区间乘积
时间复杂度为 O(M + log ^ 2N), 空间复杂度为 O(log ^ 2N), 其中 M 为 queries 的长度
```

__代码:__

__C++__:

```C++
const int MOD = 1e9 + 7;
class Solution 
{
public:
    vector<int> productQueries(int n, vector<vector<int>>& queries) 
    {
        int lowbit = 0;
        vector<int> powers, result;
        while (n) 
        {
            powers.emplace_back(lowbit = n & -n);
            n ^= lowbit;
        }
        for (const auto& query : queries) result.emplace_back(accumulate(powers.begin() + query.front(), powers.begin() + query.back() + 1, 1, [](int x, int y) { return (long long)x * y % MOD; }));  
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] productQueries(int n, int[][] queries) {
        var powers = new ArrayList<Integer>();
        int lowbit = 0, q = queries.length, MOD = 1_000_000_007, result[] = new int[q];
        while (n > 0) {
            powers.add(lowbit = n & -n);
            n ^= lowbit;
        } 
        var pre = new HashMap<Pair<Integer, Integer>, Integer>();
        for (int i = 0, m = powers.size(); i < m; i++) {
            pre.put(new Pair(i, i), powers.get(i));
            for (int j = i + 1; j < m; j++) pre.put(new Pair(i, j), (int)((long)pre.get(new Pair(i, j - 1)) * powers.get(j) % MOD));
        }
        for (int i = 0; i < q; i++) result[i] = pre.get(new Pair(queries[i][0], queries[i][1]));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def productQueries(self, n: int, queries: List[List[int]]) -> List[int]:
        powers, MOD = [], 10 ** 9 + 7
        while n:
            powers.append(lowbit := n & -n)
            n ^= lowbit
        return [reduce(lambda x, y: x * y % MOD, powers[left:right + 1]) for left, right in queries]
```
