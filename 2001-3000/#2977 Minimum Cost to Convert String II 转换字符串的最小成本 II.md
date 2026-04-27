# 2977 Minimum Cost to Convert String II 转换字符串的最小成本 II

__Description:__

You are given two __0-indexed__ strings `source` and `target`, both of length `n` and consisting of __lowercase__ English characters. You are also given two __0-indexed__ string arrays `original` and `changed`, and an integer array `cost`, where `cost[i]` represents the cost of converting the string `original[i]` to the string `changed[i]`.

You start with the string `source`. In one operation, you can pick a __substring__ `x` from the string, and change it to `y` at a cost of `z` __if__ there exists __any__ index `j` such that `cost[j] == z`, `original[j] == x`, and `changed[j] == y`. You are allowed to do __any__ number of operations, but any pair of operations must satisfy __either__ of these two conditions:

- The substrings picked in the operations are `source[a..b]` and `source[c..d]` with either `b < c` __or__ `d < a`. In other words, the indices picked in both operations are __disjoint__.
- The substrings picked in the operations are `source[a..b]` and `source[c..d]` with `a == c` __and__ `b == d`. In other words, the indices picked in both operations are __identical__.

Return _the __minimum__ cost to convert the string_ `source` _to the string_ `target` _using __any__ number of operations_. _If it is impossible to convert_ `source` _to_ `target`, _return_ `-1`.

__Note__ that there may exist indices `i`, `j` such that `original[j] == original[i]` and `changed[j] == changed[i]`.

__Example:__

Example 1:

```text
Input: source = "abcd", target = "acbe", original = ["a","b","c","c","e","d"], changed = ["b","c","b","e","b","e"], cost = [2,5,5,1,2,20]
Output: 28
Explanation: To convert "abcd" to "acbe", do the following operations:
```

- Change substring source[1..1] from "b" to "c" at a cost of 5.
- Change substring source[2..2] from "c" to "e" at a cost of 1.
- Change substring source[2..2] from "e" to "b" at a cost of 2.
- Change substring source[3..3] from "d" to "e" at a cost of 20.

The total cost incurred is 5 + 1 + 2 + 20 = 28.

It can be shown that this is the minimum possible cost.

Example 2:

```text
Input: source = "abcdefgh", target = "acdeeghh", original = ["bcd","fgh","thh"], changed = ["cde","thh","ghh"], cost = [1,3,5]
Output: 9
Explanation: To convert "abcdefgh" to "acdeeghh", do the following operations:
```

- Change substring source[1..3] from "bcd" to "cde" at a cost of 1.
- Change substring source[5..7] from "fgh" to "thh" at a cost of 3. We can do this operation because indices [5,7] are disjoint with indices picked in the first operation.
- Change substring source[5..7] from "thh" to "ghh" at a cost of 5. We can do this operation because indices [5,7] are disjoint with indices picked in the first operation, and identical with indices picked in the second operation.

The total cost incurred is 1 + 3 + 5 = 9.

It can be shown that this is the minimum possible cost.

Example 3:

```text
Input: source = "abcdefgh", target = "addddddd", original = ["bcd","defgh"], changed = ["ddd","ddddd"], cost = [100,1578]
Output: -1
Explanation: It is impossible to convert "abcdefgh" to "addddddd".
If you select substring source[1..3] as the first operation to change "abcdefgh" to "adddefgh", you cannot select substring source[3..7] as the second operation because it has a common index, 3, with the first operation.
If you select substring source[3..7] as the first operation to change "abcdefgh" to "abcddddd", you cannot select substring source[1..3] as the second operation because it has a common index, 3, with the first operation.
```

__Constraints:__

- `1 <= source.length == target.length <= 1000`
- `source`, `target` consist only of lowercase English characters.
- `1 <= cost.length == original.length == changed.length <= 100`
- `1 <= original[i].length == changed[i].length <= source.length`
- `original[i]`, `changed[i]` consist only of lowercase English characters.
- `original[i] != changed[i]`
- `1 <= cost[i] <= 10 ^ 6`

__题目描述:__

给你两个下标从 __0__ 开始的字符串 `source` 和 `target` ，它们的长度均为 `n` 并且由 __小写__ 英文字母组成。

另给你两个下标从 __0__ 开始的字符串数组 `original` 和 `changed` ，以及一个整数数组 `cost` ，其中 `cost[i]` 代表将字符串 `original[i]` 更改为字符串 `changed[i]` 的成本。

你从字符串 `source` 开始。在一次操作中，__如果__ 存在 __任意__ 下标 `j` 满足 `cost[j] == z` 、 `original[j] == x` 以及 `changed[j] == y` ，你就可以选择字符串中的 __子串__ `x` 并以 `z` 的成本将其更改为 `y` 。 你可以执行 __任意数量__ 的操作，但是任两次操作必须满足 __以下两个__ 条件 __之一__ :

- 在两次操作中选择的子串分别是 `source[a..b]` 和 `source[c..d]` ，满足 `b < c` __或__ `d < a` 。换句话说，两次操作中选择的下标 __不相交__ 。
- 在两次操作中选择的子串分别是 `source[a..b]` 和 `source[c..d]` ，满足 `a == c` __且__ `b == d` 。换句话说，两次操作中选择的下标 __相同__ 。

返回将字符串 `source` 转换为字符串 `target` 所需的 __最小__ 成本。如果不可能完成转换，则返回 `-1` 。

__注意__，可能存在下标 `i` 、 `j` 使得 `original[j] == original[i]` 且 `changed[j] == changed[i]` 。

__示例:__

示例 1：

```text
输入：source = "abcd", target = "acbe", original = ["a","b","c","c","e","d"], changed = ["b","c","b","e","b","e"], cost = [2,5,5,1,2,20]
输出：28
解释：将 "abcd" 转换为 "acbe"，执行以下操作：
```

- 将子串 source[1..1] 从 "b" 改为 "c" ，成本为 5 。
- 将子串 source[2..2] 从 "c" 改为 "e" ，成本为 1 。
- 将子串 source[2..2] 从 "e" 改为 "b" ，成本为 2 。
- 将子串 source[3..3] 从 "d" 改为 "e" ，成本为 20 。

产生的总成本是 5 + 1 + 2 + 20 = 28 。

可以证明这是可能的最小成本。

示例 2：

```text
输入：source = "abcdefgh", target = "acdeeghh", original = ["bcd","fgh","thh"], changed = ["cde","thh","ghh"], cost = [1,3,5]
输出：9
解释：将 "abcdefgh" 转换为 "acdeeghh"，执行以下操作：
```

- 将子串 source[1..3] 从 "bcd" 改为 "cde" ，成本为 1 。
- 将子串 source[5..7] 从 "fgh" 改为 "thh" ，成本为 3 。可以执行此操作，因为下标 [5,7] 与第一次操作选中的下标不相交。
- 将子串 source[5..7] 从 "thh" 改为 "ghh" ，成本为 5 。可以执行此操作，因为下标 [5,7] 与第一次操作选中的下标不相交，且与第二次操作选中的下标相同。

产生的总成本是 1 + 3 + 5 = 9 。

可以证明这是可能的最小成本。

示例 3：

```text
输入：source = "abcdefgh", target = "addddddd", original = ["bcd","defgh"], changed = ["ddd","ddddd"], cost = [100,1578]
输出：-1
解释：无法将 "abcdefgh" 转换为 "addddddd" 。
如果选择子串 source[1..3] 执行第一次操作，以将 "abcdefgh" 改为 "adddefgh" ，你无法选择子串 source[3..7] 执行第二次操作，因为两次操作有一个共用下标 3 。
如果选择子串 source[3..7] 执行第一次操作，以将 "abcdefgh" 改为 "abcddddd" ，你无法选择子串 source[1..3] 执行第二次操作，因为两次操作有一个共用下标 3 。
```

__提示：__

- `1 <= source.length == target.length <= 1000`
- `source`、 `target` 均由小写英文字母组成
- `1 <= cost.length == original.length == changed.length <= 100`
- `1 <= original[i].length == changed[i].length <= source.length`
- `original[i]`、 `changed[i]` 均由小写英文字母组成
- `original[i] != changed[i]`
- `1 <= cost[i] <= 10 ^ 6`

__思路:__

```text
Floyd ➕ 字典树 ➕ 动态规划
使用字典树记录下所有可以转换的字符串
为了方便给每个字符串一个编号, 类似哈希表的映射
然后处理所有能够转换的距离
距离中记录的是两个编号之间可达的距离
最后使用动态规划
设 dp[i] 表示 source[:i] 和 target[:i] 转换的最小代价
source[i] == target[i] 时, 可以不转换, 此时 dp[i] = dp[i + 1]
否则就需要转换
dp[i] = min(dp[i], dp[j] + dis[p][q]), 其中 p 和 q 分别为 source[i:j], target[i:j]
时间复杂度为 O(N ^ 2 + MN + M ^ 3), 空间复杂度为 O(N + MN + N ^ 2)
```

__代码:__

__C++__:

```C++
struct Node 
{
    Node* children[26]{};
    int id = -1;
};

class Solution 
{
public:
    long long minimumCost(string source, string target, vector<string>& original, vector<string>& changed, vector<int>& cost) 
    {
        Node* root = new Node();
        int id = 0, n = source.size(), m = original.size(), inf = 0x3f3f3f3f, x = 0, y = 0;
        auto build = [&](const auto& s) -> int 
        {
            Node* cur = root;
            for (const auto& c : s) 
            {
                if (!cur -> children[c - 'a']) cur -> children[c - 'a'] = new Node();
                cur = cur -> children[c - 'a'];
            }
            return cur -> id < 0 ? (cur -> id = id++) : cur -> id;
        };
        vector<vector<int>> dis(m << 1, vector<int>(m << 1, inf));
        for (int i = 0, p = dis.size(); i < p; i++) dis[i][i] = 0;
        for (int i = 0; i < m; i++) dis[x][y] = min(dis[x = build(original[i])][y = build(changed[i])], cost[i]);
        for (int k = 0; k < id; k++) for (int i = 0; i < id; i++) if (dis[i][k] < inf) for (int j = 0; j < id; j++) dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
        vector<long long> dp(n + 1);
        long long inf2 = LLONG_MAX >> 1LL;
        for (int i = n - 1; i > -1; i--) 
        {
            dp[i] = source[i] == target[i] ? dp[i + 1] : inf2;
            Node *p = root, *q = root;
            for (int j = i, d = 0; j < n; j++) 
            {
                if (!(p = p -> children[source[j] - 'a']) or !(q = q -> children[target[j] - 'a'])) break;
                if (p -> id < 0 or q -> id < 0) continue;
                if ((d = dis[p -> id][q -> id]) < inf) dp[i] = min(dp[i], d + dp[j + 1]);
            }
        }
        return dp.front() < inf2 ? dp.front() : -1;
    }
};
```

__Java__:

```Java
class Solution {
    class Node {
        Node[] children = new Node[26];
        int id = -1;
    }

    private int build(String s) {
        Node cur = root;
        for (var c : s.toCharArray()) {
            if (cur.children[c - 'a'] == null) cur.children[c - 'a'] = new Node();
            cur = cur.children[c - 'a'];
        }
        return cur.id < 0 ? (cur.id = id++) : cur.id;
    }

    private Node root = new Node();
    private int id = 0;

    public long minimumCost(String source, String target, String[] original, String[] changed, int[] cost) {
        int n = source.length(), m = original.length, inf = 0x3f3f3f3f, dis[][] = new int[m << 1][m << 1], x = 0, y = 0;
        for (int i = 0, p = dis.length; i < p; i++) Arrays.fill(dis[i], inf);
        for (int i = 0, p = dis.length; i < p; i++) dis[i][i] = 0;
        for (int i = 0; i < m; i++) dis[x = build(original[i])][y = build(changed[i])] = Math.min(dis[x][y], cost[i]);
        for (int k = 0; k < id; k++) for (int i = 0; i < id; i++) if (dis[i][k] < inf) for (int j = 0; j < id; j++) dis[i][j] = Math.min(dis[i][j], dis[i][k] + dis[k][j]);
        long dp[] = new long[n + 1], inf2 = Long.MAX_VALUE >> 1L;
        for (int i = n - 1; i > -1; i--) {
            dp[i] = source.charAt(i) == target.charAt(i) ? dp[i + 1] : inf2;
            Node p = root, q = root;
            for (int j = i, d = 0; j < n; j++) {
                if ((p = p.children[source.charAt(j) - 'a']) == null || (q = q.children[target.charAt(j) - 'a']) == null) break;
                if (p.id < 0 || q.id < 0) continue;
                if ((d = dis[p.id][q.id]) < inf) dp[i] = Math.min(dp[i], d + dp[j + 1]);
            }
        }
        return dp[0] < inf2 ? dp[0] : -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumCost(self, source: str, target: str, original: List[str], changed: List[str], cost: List[int]) -> int:
        groups, dis, dp = defaultdict(set), defaultdict(lambda: defaultdict(lambda: inf)), [0] + [inf] * (n := len(source))
        for x, y, c in zip(original, changed, cost):
            groups[len(x)].add(x)
            groups[len(y)].add(y)
            dis[x][y], dis[x][x], dis[y][y] = min(dis[x][y], c), 0, 0
        for group in groups.values():
            for k in group:
                for i in group:
                    if dis[i][k] < inf:
                        for j in group:
                            dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j])
        for i in range(1, n + 1):
            if source[i - 1] == target[i - 1]:
                dp[i] = dp[i - 1]
            for size, group in groups.items():
                if i >= size and (s := source[i - size:i]) in group and (t := target[i - size:i]) in group:
                    dp[i] = min(dp[i], dp[i - size] + dis[s][t])
        return dp[n] if dp[n] < inf else -1
```
