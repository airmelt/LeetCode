# 2791 Count Paths That Can Form a Palindrome in a Tree 树中可以形成回文的路径数

__Description:__

You are given a __tree__ (i.e. a connected, undirected graph that has no cycles) __rooted__ at node `0` consisting of `n` nodes numbered from `0` to `n - 1`. The tree is represented by a __0-indexed__ array `parent` of size `n`, where `parent[i]` is the parent of node `i`. Since node `0` is the root, `parent[0] == -1`.

You are also given a string `s` of length `n`, where `s[i]` is the character assigned to the edge between `i` and `parent[i]`. `s[0]` can be ignored.

Return _the number of pairs of nodes_ `(u, v)` _such that_ `u < v` _and the characters assigned to edges on the path from_ `u` _to_ `v` _can be __rearranged__ to form a __palindrome___.

A string is a palindrome when it reads the same backwards as forwards.

__Example:__

Example 1:

![2791-1](https://assets.leetcode.com/uploads/2023/07/15/treedrawio-8drawio.png)

```text
Input: parent = [-1,0,0,1,1,2], s = "acaabc"
Output: 8
Explanation: The valid pairs are:
```

- All the pairs (0,1), (0,2), (1,3), (1,4) and (2,5) result in one character which is always a palindrome.
- The pair (2,3) result in the string "aca" which is a palindrome.
- The pair (1,5) result in the string "cac" which is a palindrome.
- The pair (3,5) result in the string "acac" which can be rearranged into the palindrome "acca".

Example 2:

```text
Input: parent = [-1,0,0,0,0], s = "aaaaa"
Output: 10
Explanation: Any pair of nodes (u,v) where u < v is valid.
```

__Constraints:__

- `n == parent.length == s.length`
- `1 <= n <= 10 ^ 5`
- `0 <= parent[i] <= n - 1` for all `i >= 1`
- `parent[0] == -1`
- `parent` represents a valid tree.
- `s` consists of only lowercase English letters.

__题目描述:__

给你一棵 __树__（即，一个连通、无向且无环的图），__根__ 节点为 `0` ，由编号从 `0` 到 `n - 1` 的 `n` 个节点组成。这棵树用一个长度为 `n` 、下标从 __0__ 开始的数组 `parent` 表示，其中 `parent[i]` 为节点 `i` 的父节点，由于节点 `0` 为根节点，所以 `parent[0] == -1` 。

另给你一个长度为 `n` 的字符串 `s` ，其中 `s[i]` 是分配给 `i` 和 `parent[i]` 之间的边的字符。 `s[0]` 可以忽略。

找出满足 `u < v` ，且从 `u` 到 `v` 的路径上分配的字符可以 __重新排列__ 形成 __回文__ 的所有节点对 `(u, v)` ，并返回节点对的数目。

如果一个字符串正着读和反着读都相同，那么这个字符串就是一个 回文 。

__示例:__

示例 1：

![2791-2](https://assets.leetcode.com/uploads/2023/07/15/treedrawio-8drawio.png)

```text
输入：parent = [-1,0,0,1,1,2], s = "acaabc"
输出：8
解释：符合题目要求的节点对分别是：
```

- (0,1)、(0,2)、(1,3)、(1,4) 和 (2,5) ，路径上只有一个字符，满足回文定义。
- (2,3)，路径上字符形成的字符串是 "aca" ，满足回文定义。
- (1,5)，路径上字符形成的字符串是 "cac" ，满足回文定义。
- (3,5)，路径上字符形成的字符串是 "acac" ，可以重排形成回文 "acca" 。

示例 2：

```text
输入：parent = [-1,0,0,0,0], s = "aaaaa"
输出：10
解释：任何满足 u < v 的节点对 (u,v) 都符合题目要求。
```

__提示：__

- `n == parent.length == s.length`
- `1 <= n <= 10 ^ 5`
- 对于所有 `i >= 1` ， `0 <= parent[i] <= n - 1` 均成立
- `parent[0] == -1`
- `parent` 表示一棵有效的树
- `s` 仅由小写英文字母组成

__思路:__

```text
状态压缩 ➕ DFS
遍历整棵树
用一个 26 位二进制来记录出现过的字符串
如果能够构成回文串, 最多只能出现一个奇数
所以这个二进制为 0 或者 2 ^ k (k < 26)
用一个哈希表记录路径值
将空路径的值 1 放入计数
将当前值与 1 << (s[w] - 'a') 异或就能得到当前值的字符串
将计数里的和自身相等的找出来, 可以异或形成 0, 所以将当前计数累加
或者与 2 ^ k 异或, 这样找出来的值异或的结果就是 2 ^ k, 也进行累加
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countPalindromePaths(vector<int>& parent, string s) 
    {
        unordered_map<int, unordered_set<int>> tree;
        for (int i = 1, n = parent.size(); i < n; i++) tree[parent[i]].insert(i);
        unordered_map<long long, int> cur;
        cur[0LL] = 1;
        auto dfs = [&](auto&& dfs, int u, auto v) -> long long 
        {
            auto result = 0LL, x = 0LL;
            for (int w : tree[u]) 
            {
                result += cur[x = v ^ (1 << (s[w] - 'a'))];
                for (int i = 0; i < 26; i++) result += cur[x ^ (1 << i)];
                ++cur[x];
                result += dfs(dfs, w, x);
            }
            return result;
        };
        return dfs(dfs, 0, 0LL);
    }
};
```

__Java__:

```Java
class Solution {
    public long countPalindromePaths(List<Integer> parent, String s) {
        var tree = new HashMap<Integer, Set<Integer>>();
        for (int i = 1, n = parent.size(); i < n; i++) tree.computeIfAbsent(parent.get(i), k -> new HashSet<>()).add(i);
        var cur = new HashMap<Long, Integer>();
        cur.put(0L, 1);
        return dfs(0, 0L, tree, cur, s.toCharArray());
    }

    private long dfs(int u, long v, Map<Integer, Set<Integer>> tree, Map<Long, Integer> cur, char[] s) {
        long result = 0L, x = 0L;
        for (int w : tree.getOrDefault(u, new HashSet<>())) {
            result += cur.getOrDefault(x = v ^ (1 << (s[w] - 'a')), 0);
            for (int i = 0; i < 26; i++) result += cur.getOrDefault(x ^ (1 << i), 0);
            cur.merge(x, 1, Integer::sum);
            result += dfs(w, x, tree, cur, s);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countPalindromePaths(self, parent: List[int], s: str) -> int:
        n, tree, cur = len(s), defaultdict(set), Counter([0])
        for i in range(1, n):
            tree[parent[i]].add(i)

        def dfs(u: int, v: int) -> int:
            result = 0
            for w in tree[u]:
                result += cur[x := v ^ (1 << (ord(s[w]) - ord('a')))] + sum(cur[x ^ (1 << i)] for i in range(26))
                cur[x] += 1
                result += dfs(w, x)
            return result
        return dfs(0, 0)
```
