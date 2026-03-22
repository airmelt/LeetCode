# 2564 Substring XOR Queries 子字符串异或查询

__Description:__

You are given a __binary string__ `s`, and a __2D__ integer array `queries` where `queries[i] = [first_i, second_i]`.

For the `i ^ th` query, find the __shortest substring__ of `s` whose __decimal value__, `val`, yields `second_i` when __bitwise XORed__ with `first_i`. In other words, `val ^ first_i == second_i`.

The answer to the `i ^ th` query is the endpoints (__0-indexed__) of the substring `[left_i, right_i]` or `[-1, -1]` if no such substring exists. If there are multiple answers, choose the one with the __minimum__ `left_i`.

_Return an array_ `ans` _where_ `ans[i] = [left_i, right_i]` _is the answer to the_ `i ^ th` _query._

A substring is a contiguous non-empty sequence of characters within a string.

__Example:__

Example 1:

```text
Input:  s = "101101", queries = [[0,5],[1,2]]
Output:  [[0,2],[2,3]]
Explanation:  For the first query the substring in range [0,2] is "101" which has a decimal value of 5, and 5 ^ 0 = 5, hence the answer to the first query is [0,2]. In the second query, the substring in range [2,3] is "11", and has a decimal value of 3, and 3 ^ 1 = 2. So, [2,3] is returned for the second query.
```

Example 2:

```text
Input:  s = "0101", queries = [[12,8]]
Output:  [[-1,-1]]
Explanation:  In this example there is no substring that answers the query, hence [-1,-1] is returned.
```

Example 3:

```text
Input:  s = "1", queries = [[4,5]]
Output:  [[0,0]]
Explanation:  For this example, the substring in range [0,0] has a decimal value of 1, and 1 ^ 4 = 5. So, the answer is [0,0].
```

__Constraints:__

- `1 <= s.length <= 10 ^ 4`
- `s[i]` is either `'0'` or `'1'`.
- `1 <= queries.length <= 10 ^ 5`
- `0 <= first_i, second_i <= 10 ^ 9`

__题目描述:__

给你一个 __二进制字符串__ `s` 和一个整数数组 `queries` ，其中 `queries[i] = [first_i, second_i]` 。

对于第 `i` 个查询，找到 `s` 的 __最短子字符串__ ，它对应的 __十进制__ 值 `val` 与 `first_i` _按位异或_ 得到 `second_i` ，换言之， `val ^ first_i == second_i` 。

第 `i` 个查询的答案是子字符串 `[left_i, right_i]` 的两个端点（下标从 __0__ 开始），如果不存在这样的子字符串，则答案为 `[-1, -1]` 。如果有多个答案，请你选择 `left_i` 最小的一个。

请你返回一个数组 `ans` ，其中 `ans[i] = [left_i, right_i]` 是第 `i` 个查询的答案。

子字符串 是一个字符串中一段连续非空的字符序列。

__示例:__

示例 1：

```text
输入: s = "101101", queries = [[0,5],[1,2]]
输出: [[0,2],[2,3]]
解释: 第一个查询，端点为 [0,2] 的子字符串为 "101" ，对应十进制数字 5 ，且 5 ^ 0 = 5 ，所以第一个查询的答案为 [0,2]。第二个查询中，端点为 [2,3] 的子字符串为 "11" ，对应十进制数字 3 ，且 3 ^ 1 = 2。所以第二个查询的答案为 [2,3] 。
```

示例 2：

```text
输入: s = "0101", queries = [[12,8]]
输出: [[-1,-1]]
解释: 这个例子中，没有符合查询的答案，所以返回 [-1,-1] 。
```

示例 3：

```text
输入: s = "1", queries = [[4,5]]
输出: [[0,0]]
解释: 这个例子中，端点为 [0,0] 的子字符串对应的十进制值为 1，且 1 ^ 4 = 5。所以答案为 [0,0] 。
```

__提示：__

- `1 <= s.length <= 10 ^ 4`
- `s[i]` 要么是 `'0'` ，要么是 `'1'` 。
- `1 <= queries.length <= 10 ^ 5`
- `0 <= first_i, second_i <= 10 ^ 9`

__思路:__

```text
预处理
由 a ^ b = c 可得 a = b ^ c
因此可以将每个查询转换为 a = b ^ c 的形式
由于数字都在 int 范围内, 因此可以预处理二进制字符串
将数字存储在哈希表中, 键为数字, 值为该数字在字符串中最先出现的位置
先找到字符串中第一个 0 的位置, 如果存在则记录为 0 的位置
由于需要找最先出现的位置, 在后序的查询中, 不需要再处理 0
对于每一个位置, 如果该位置的字符为 1, 则从该位置开始向后遍历最多 30 位
将哈希表中没有出现的数字都记录下来
对于每个查询, 直接在哈希表中查找对应的数字
如果存在则返回该数字的最先出现的位置, 否则返回 [-1, -1]
时间复杂度为 O(NlogM + Q), 空间复杂度为 O(NlogM), M 为 max(queries), N 为字符串长度, Q 为查询数量
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> substringXorQueries(string s, vector<vector<int>>& queries) 
    {
        unordered_map<int, vector<int>> pos;
        if (auto a = s.find('0'); a != string::npos) pos[0] = {(int)a, (int)a};
        int n = s.size(), m = queries.size();
        for (int i = 0; i < n; i++) if (s[i] & 1) for (int j = i, cur = 0; j < min(n, i + 30); j++) if (!pos.count(cur = (cur << 1) | (s[j] & 1))) pos[cur] = {i, j};
        vector<vector<int>> result(m);
        for (int i = 0, cur = 0; i < m; i++) result[i] = pos.count(cur = queries[i][0] ^ queries[i][1]) ? pos[cur] : vector<int>{-1, -1};
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] substringXorQueries(String s, int[][] queries) {
        var pos = new HashMap<Integer, int[]>();
        int n = s.length(), m = queries.length, a = s.indexOf('0'), result[][] = new int[m][], NOT_FOUND[] = new int[]{-1, -1};
        if (a > -1) pos.put(0, new int[]{a ,a});
        for (int i = 0; i < n; i++) if (s.charAt(i) != '0') for (int j = i, cur = 0; j < Math.min(n, i + 30); j++) pos.putIfAbsent(cur = (cur << 1) | (s.charAt(j) & 1), new int[]{i, j});
        for (int i = 0; i < m; i++) result[i] = pos.getOrDefault(queries[i][0] ^ queries[i][1], NOT_FOUND);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def substringXorQueries(self, s: str, queries: List[List[int]]) -> List[List[int]]:
        queries, pos, n = [first ^ second for first, second in queries], {0: [(a := (s.index('0') if '0' in s else -1)), a], 1: [(b := (s.index('1') if '1' in s else -1)), b]}, len(s)
        for i in range(n):
            if s[i] != '0':
                for j in range(i + 2, min(n + 1, i + 33)):
                    if (cur := int(s[i:j], 2)) not in pos:
                        pos[cur] = [i, j - 1]
        return [pos.get(cur, [-1, -1]) for cur in queries]
```
