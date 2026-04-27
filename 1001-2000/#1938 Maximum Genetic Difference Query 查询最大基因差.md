# 1938 Maximum Genetic Difference Query 查询最大基因差

__Description:__

There is a rooted tree consisting of `n` nodes numbered `0` to `n - 1`. Each node's number denotes its __unique genetic value__ (i.e. the genetic value of node `x` is `x`). The __genetic difference__ between two genetic values is defined as the __bitwise-____XOR__ of their values. You are given the integer array `parents`, where `parents[i]` is the parent for node `i`. If node `x` is the __root__ of the tree, then `parents[x] == -1`.

You are also given the array `queries` where `queries[i] = [nodei, vali]`. For each query `i`, find the __maximum genetic difference__ between `vali` and `pi`, where `pi` is the genetic value of any node that is on the path between `nodei` and the root (including `nodei` and the root). More formally, you want to maximize `vali XOR pi`.

Return _an array_ `ans` _where_ `ans[i]` _is the answer to the_ `i ^ th` _query_.

__Example:__

Example 1:

![1938-1](https://assets.leetcode.com/uploads/2021/06/29/c1.png)

```text
Input: parents = [-1,0,1,1], queries = [[0,2],[3,2],[2,5]]
Output: [2,3,7]
Explanation: The queries are processed as follows:
```

- [0,2]: The node with the maximum genetic difference is 0, with a difference of 2 XOR 0 = 2.
- [3,2]: The node with the maximum genetic difference is 1, with a difference of 2 XOR 1 = 3.
- [2,5]: The node with the maximum genetic difference is 2, with a difference of 5 XOR 2 = 7.

Example 2:

![1938-2](https://assets.leetcode.com/uploads/2021/06/29/c2.png)

```text
Input: parents = [3,7,-1,2,0,7,0,2], queries = [[4,6],[1,15],[0,5]]
Output: [6,14,7]
Explanation: The queries are processed as follows:
```

- [4,6]: The node with the maximum genetic difference is 0, with a difference of 6 XOR 0 = 6.
- [1,15]: The node with the maximum genetic difference is 1, with a difference of 15 XOR 1 = 14.
- [0,5]: The node with the maximum genetic difference is 2, with a difference of 5 XOR 2 = 7.

__Constraints:__

- `2 <= parents.length <= 10 ^ 5`
- `0 <= parents[i] <= parents.length - 1` for every node `i` that is __not__ the root.
- `parents[root] == -1`
- `1 <= queries.length <= 3 * 10 ^ 4`
- `0 <= nodei <= parents.length - 1`
- `0 <= vali <= 2 * 10 ^ 5`

__题目描述:__

给你一棵 `n` 个节点的有根树，节点编号从 `0` 到 `n - 1` 。每个节点的编号表示这个节点的 __独一无二的基因值__ （也就是说节点 `x` 的基因值为 `x`）。两个基因值的 __基因差__ 是两者的 __异或和__ 。给你整数数组 `parents` ，其中 `parents[i]` 是节点 `i` 的父节点。如果节点 `x` 是树的 __根__ ，那么 `parents[x] == -1` 。

给你查询数组 `queries` ，其中 `queries[i] = [nodei, vali]` 。对于查询 `i` ，请你找到 `vali` 和 `pi` 的 __最大基因差__ ，其中 `pi` 是节点 `nodei` 到根之间的任意节点（包含 `nodei` 和根节点）。更正式的，你想要最大化 `vali XOR pi` 。

请你返回数组 `ans` ，其中 `ans[i]` 是第 `i` 个查询的答案。

__示例:__

示例 1：

![1938-3](https://assets.leetcode.com/uploads/2021/06/29/c1.png)

```text
输入：parents = [-1,0,1,1], queries = [[0,2],[3,2],[2,5]]
输出：[2,3,7]
解释：查询数组处理如下：
```

- [0,2]：最大基因差的对应节点为 0 ，基因差为 2 XOR 0 = 2 。
- [3,2]：最大基因差的对应节点为 1 ，基因差为 2 XOR 1 = 3 。
- [2,5]：最大基因差的对应节点为 2 ，基因差为 5 XOR 2 = 7 。

示例 2：

![1938-4](https://assets.leetcode.com/uploads/2021/06/29/c2.png)

```text
输入：parents = [3,7,-1,2,0,7,0,2], queries = [[4,6],[1,15],[0,5]]
输出：[6,14,7]
解释：查询数组处理如下：
```

- [4,6]：最大基因差的对应节点为 0 ，基因差为 6 XOR 0 = 6 。
- [1,15]：最大基因差的对应节点为 1 ，基因差为 15 XOR 1 = 14 。
- [0,5]：最大基因差的对应节点为 2 ，基因差为 5 XOR 2 = 7 。

__提示：__

- `2 <= parents.length <= 10 ^ 5`
- 对于每个 __不是__ 根节点的 `i` ，有 `0 <= parents[i] <= parents.length - 1` 。
- `parents[root] == -1`
- `1 <= queries.length <= 3 * 10 ^ 4`
- `0 <= nodei <= parents.length - 1`
- `0 <= vali <= 2 * 10 ^ 5`

__思路:__

```text
前缀树(字典树)
字典树可以完成添加结点, 查询最大异或值, 删除结点的操作
由于最大值不超过 2 * 10 ^ 5, 所以可以用 18 位二进制数表示
在字典树中存储每个数的二进制表示, 从高位到低位依次存储
按照查询的结点值对查询进行分类, 从根节点开始, 依次查询每个结点的异或值
在 dfs 时加入当前结点, 在 dfs 结束后删除当前结点
时间复杂度为 O(N + M), 空间复杂度为 O(N + M), 其中 N 为结点数, M 为查询数
```

__代码:__

__C++__:

```C++
struct Trie 
{
    Trie* left;
    Trie* right;
    int cnt;
    Trie(): left(nullptr), right(nullptr), cnt(0) {}
};

class Solution 
{
public:
    vector<int> maxGeneticDifference(vector<int>& parents, vector<vector<int>>& queries) {
        int n = parents.size(), root = -1, m = queries.size();
        vector<vector<int>> data(n);
        for (int i = 0; i < n; i++) 
        {
            if (parents[i] == -1) root = i;
            else data[parents[i]].emplace_back(i);
        }
        vector<vector<pair<int, int>>> q(n);
        vector<int> result(m);
        for (int i = 0; i < m; i++) q[queries[i].front()].emplace_back(i, queries[i].back());
        Trie* trie = new Trie();
        auto trie_update = [&](int val) 
        {
            Trie* cur = trie;
            for (int i = 17; i > -1; i--) 
            {
                if (val & (1 << i)) 
                {
                    if (!cur -> right) cur -> right = new Trie();
                    cur = cur -> right;
                }
                else 
                {
                    if (!cur -> left) cur -> left = new Trie();
                    cur = cur -> left;
                }
                ++cur -> cnt;
            }
        };
        auto trie_query = [&](int val) -> int 
        {
            int result = 0;
            Trie* cur = trie;
            for (int i = 17; i > -1; i--) 
            {
                if (val & (1 << i)) 
                {
                    if (cur -> left and cur -> left -> cnt) 
                    {
                        result |= (1 << i);
                        cur = cur -> left;
                    }
                    else cur = cur -> right;
                }
                else 
                {
                    if (cur -> right and cur -> right -> cnt) 
                    {
                        result |= (1 << i);
                        cur = cur -> right;
                    }
                    else cur = cur -> left;
                }
            }
            return result;
        };
        auto trie_delete = [&](int val) 
        {
            Trie* cur = trie;
            for (int i = 17; i > -1; i--) 
            {
                if (val & (1 << i)) cur = cur -> right;
                else cur = cur -> left;
                --cur -> cnt;
            }
        };
        function<void(int)> dfs = [&](int fa) 
        {
            trie_update(fa);
            for (const auto& [node, val]: q[fa]) result[node] = trie_query(val);
            for (int child : data[fa]) dfs(child);
            trie_delete(fa);
        };
        dfs(root);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    static class Trie {
        Trie[] children;
        int count;

        Trie() {
            children = new Trie[2];
            count = 0;
        }

        void update(int val) {
            Trie cur = this;
            for (int i = 17; i > -1; i--) {
                int bit = (val >> i) & 1;
                if (cur.children[bit] == null) cur.children[bit] = new Trie();
                cur = cur.children[bit];
                cur.count++;
            }
        }

        void delete(int val) {
            Trie cur = this;
            for (int i = 17; i > -1; i--) {
                int bit = (val >> i) & 1;
                cur = cur.children[bit];
                --cur.count;
            }
        }

        int query(int val) {
            Trie cur = this;
            int result = 0;
            for (int i = 17; i > -1; i--) {
                int bit = (val >> i) & 1, rev = bit ^ 1;
                if (cur.children[rev] != null) {
                    if (cur.children[rev].count > 0) {
                        result |= (rev << i);
                        cur = cur.children[rev];
                    } else {
                        result |= (bit << i);
                        cur = cur.children[bit];
                    }
                } else {
                    if (cur.children[bit].count > 0) {
                        result |= (bit << i);
                        cur = cur.children[bit];
                    } else {
                        result |= (rev << i);
                        cur = cur.children[rev];
                    }
                }
            }
            return result ^ val;
        }
    }

    public int[] maxGeneticDifference(int[] parents, int[][] queries) {
        int n = parents.length, root = -1, m = queries.length;
        data = new ArrayList[n];
        for (int i = 0; i < n; i++) data[i] = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if (parents[i] == -1) root = i;
            else data[parents[i]].add(i);
        }
        q = new ArrayList[n];
        for (int i = 0; i < n; i++) q[i] = new ArrayList<>();
        for (int i = 0; i < m; i++) q[queries[i][0]].add(new int[]{ i, queries[i][1] });
        result = new int[m];
        trie = new Trie();
        dfs(root);
        return result;
    }

    List<Integer>[] data;
    List<int[]>[] q;
    int[] result;
    Trie trie;

    private void dfs(int fa) {
        trie.update(fa);
        for (var p : q[fa]) result[p[0]] = trie.query(p[1]);
        for (int child : data[fa]) dfs(child);
        trie.delete(fa);
    }
}
```

__Python__:

```Python
class Trie:
    def __init__(self) -> NoReturn:
        self.data = defaultdict(int)
        self.max_length = 18
    
    def update(self, val: int) -> NoReturn:
        num, cur = bin(val)[2:].zfill(self.max_length), self.data
        for i in num:
            if i not in cur:
                cur[i] = defaultdict(int)
            cur = cur[i]
            cur['count'] += 1

    def query(self, val: int) -> int:
        num, cur, pre = bin(val)[2:].zfill(self.max_length), self.data, '0b'
        for i in num:
            if i == '0' and '1' in cur and cur['1']['count'] > 0:
                pre += '1'
                cur = cur['1']
            elif i == '1' and '0' in cur and cur['0']['count'] > 0:
                pre += '1'
                cur = cur['0']
            else:
                pre += '0'
                cur = cur[i]
        return int(pre, 2)

    def delete(self, val: int) -> NoReturn:
        num, cur = bin(val)[2:].zfill(self.max_length), self.data
        for i in num:
            cur[i]['count'] -= 1
            cur = cur[i]    

class Solution:
    def maxGeneticDifference(self, parents: List[int], queries: List[List[int]]) -> List[int]:
        result, data, n, trie = defaultdict(dict), defaultdict(list), len(parents), Trie()
        for i in range(n):
            data[parents[i]].append(i)
        for node, val in queries:
            result[node][val] = 0
        def dfs(root: int) -> NoReturn:
            trie.update(root)
            for k in result[root]:
                result[root][k] = trie.query(k)
            for child in data[root]:
                dfs(child)
                trie.delete(child)

        dfs(data[-1][0])
        return [result[node][val] for node, val in queries]
```
