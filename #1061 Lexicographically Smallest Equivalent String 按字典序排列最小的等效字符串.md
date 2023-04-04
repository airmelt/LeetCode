# 1061 Lexicographically Smallest Equivalent String 按字典序排列最小的等效字符串

__Description:__

You are given two strings of the same length `s1` and `s2` and a string `baseStr`.

We say `s1[i]` and `s2[i]` are equivalent characters.

- For example, if `s1 = "abc"` and `s2 = "cde"`, then we have `'a' == 'c'`, `'b' == 'd'`, and `'c' == 'e'`.

Equivalent characters follow the usual rules of any equivalence relation:

- __Reflexivity:__ `'a' == 'a'`.
- __Symmetry:__ `'a' == 'b'` implies `'b' == 'a'`.
- __Transitivity:__ `'a' == 'b'` and `'b' == 'c'` implies `'a' == 'c'`.

For example, given the equivalency information from `s1 = "abc"` and `s2 = "cde"`, `"acd"` and `"aab"` are equivalent strings of `baseStr = "eed"`, and `"aab"` is the lexicographically smallest equivalent string of `baseStr`.

Return _the lexicographically smallest equivalent string of_ `baseStr` _by using the equivalency information from_ `s1` _and_ `s2`.

__Example:__

Example 1:

```text
Input: s1 = "parker", s2 = "morris", baseStr = "parser"
Output: "makkek"
Explanation: Based on the equivalency information in s1 and s2, we can group their characters as [m,p], [a,o], [k,r,s], [e,i].
The characters in each group are equivalent and sorted in lexicographical order.
So the answer is "makkek".
```

Example 2:

```text
Input: s1 = "hello", s2 = "world", baseStr = "hold"
Output: "hdld"
Explanation: Based on the equivalency information in s1 and s2, we can group their characters as [h,w], [d,e,o], [l,r].
So only the second letter 'o' in baseStr is changed to 'd', the answer is "hdld".
```

Example 3:

```text
Input: s1 = "leetcode", s2 = "programs", baseStr = "sourcecode"
Output: "aauaaaaada"
Explanation: We group the equivalent characters in s1 and s2 as [a,o,e,r,s,c], [l,p], [g,t] and [d,m], thus all letters in baseStr except 'u' and 'd' are transformed to 'a', the answer is "aauaaaaada".
```

__Constraints:__

- `1 <= s1.length, s2.length, baseStr <= 1000`
- `s1.length == s2.length`
- `s1`, `s2`, and `baseStr` consist of lowercase English letters.

__题目描述:__

给出长度相同的两个字符串 `s1` 和 `s2` ，还有一个字符串 `baseStr` 。

其中  `s1[i]` 和 `s2[i]` 是一组等价字符。

- 举个例子，如果 `s1 = "abc"` 且 `s2 = "cde"`，那么就有 `'a' == 'c', 'b' == 'd', 'c' == 'e'`。

等价字符遵循任何等价关系的一般规则：

- __自反性__ : `'a' == 'a'`
- __对称性__ : `'a' == 'b'` 则必定有 `'b' == 'a'`
- __传递性__ : `'a' == 'b'` 且 `'b' == 'c'` 就表明 `'a' == 'c'`

例如， `s1 = "abc"` 和 `s2 = "cde"` 的等价信息和之前的例子一样，那么 `baseStr = "eed"` , `"acd"` 或 `"aab"`，这三个字符串都是等价的，而 `"aab"` 是 `baseStr` 的按字典序最小的等价字符串

利用 `s1` 和 `s2` 的等价信息，找出并返回 `baseStr` 的按字典序排列最小的等价字符串。

__示例:__

示例 1：

```text
输入:s1 = "parker", s2 = "morris", baseStr = "parser"
输出:"makkek"
解释:根据 `A` 和 `B 中的等价信息，`我们可以将这些字符分为 `[m,p]`, `[a,o]`, `[k,r,s]`, `[e,i] 共 4 组`。每组中的字符都是等价的，并按字典序排列。所以答案是 `"makkek"`。
```

示例 2：

```text
输入:s1 = "hello", s2 = "world", baseStr = "hold"
输出:"hdld"
解释:根据 `A` 和 `B 中的等价信息，`我们可以将这些字符分为 `[h,w]`, `[d,e,o]`, `[l,r] 共 3 组`。所以只有 S 中的第二个字符 `'o'` 变成 `'d'，最后答案为` `"hdld"`。
```

示例 3：

```text
输入:s1 = "leetcode", s2 = "programs", baseStr = "sourcecode"
输出:"aauaaaaada"
解释:我们可以把 A 和 B 中的等价字符分为 `[a,o,e,r,s,c]`, `[l,p]`, `[g,t]` 和 `[d,m] 共 4 组`，因此 `S` 中除了 `'u'` 和 `'d'` 之外的所有字母都转化成了 `'a'`，最后答案为 `"aauaaaaada"`。
```

__提示：__

- `1 <= s1.length, s2.length, baseStr <= 1000`
- `s1.length == s2.length`
- 字符串 `s1`, `s2`, and `baseStr` 仅由从 `'a'` 到 `'z'` 的小写英文字母组成。

__思路:__

```text
并查集
因为用到了等价关系可以尝试使用并查集
将字符映射到 26 的整数上
然后使用并查集将 s1 和 s2 的所有字符合并
需要把当前较小的字符作为 parent
最后遍历一次 baseStr 的字符找到所有 parent 即可
时间复杂度为 O(N), 空间复杂度为 O(1), 只需要长度为 26 的额外数组存放 parent 信息
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string smallestEquivalentString(string s1, string s2, string baseStr) 
    {
        vector<int> parent(26);
        iota(parent.begin(), parent.end(), 0);
        auto find = [&](int x) -> int
        {
            while (x != parent[x])
            {
                parent[x] = parent[parent[x]];
                x = parent[x];
            }
            return x;
        };
        auto merge = [&](int x, int y)
        {
            x = find(x);
            y = find(y);
            if (x == y) return;
            if (x > y) parent[x] = y;
            else parent[y] = x;
        };
        for (int i = 0, n = s1.size(); i < n; i++) merge(s1[i] - 'a', s2[i] - 'a');
        string result;
        for (const auto& c: baseStr) result.push_back(find(c - 'a') + 'a');
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String smallestEquivalentString(String s1, String s2, String baseStr) {
        int n = s1.length();
        UF uf = new UF(26);
        for (int i = 0; i < n; i++) uf.union(s1.charAt(i) - 'a', s2.charAt(i) - 'a');
        StringBuilder sb = new StringBuilder();
        for (char c : baseStr.toCharArray()) sb.append((char) (uf.find(c - 'a') + 'a'));
        return sb.toString();
    }

    class UF {
        private int[] parent;

        public UF(int n) {
            parent = new int[n];
            for (int i = 0; i < n; i++) parent[i] = i;
        }
        
        public int find(int x) {
            return x == parent[x] ? x : (parent[x] = find(parent[x]));
        }

        public void union(int p, int q) {
            p = find(p);
            q = find(q);
            if (p == q) return;
            if (p < q) parent[q] = p;
            else parent[p] = q;
        }
    }
}
```

__Python__:

```Python
class UF:
    def __init__(self, n: int) -> None:
        self.parent = [i for i in range(n)]
        self.weight = [1] * n
        self.count = n

    def union(self, p: int, q: int) -> None:
        """
        连接两个点
        :param p: 一个节点
        :param q: 另一个节点
        :return:
        """
        p = self.find(p)
        q = self.find(q)
        p, q = max(p, q), min(p, q)
        self.parent[p] = q
        self.count -= 1

    def connected(self, p: int, q: int) -> bool:
        """
        检查两个点是否在同一分量
        :param p: 一个节点
        :param q: 另一个节点
        :return: 返回两个点是否在同一个分量
        """
        return self.find(p) == self.find(q)

    def find(self, p: int) -> int:
        """
        查找根节点, 并进行路径压缩
        :param p: 一个节点
        :return: 根节点
        """
        while self.parent[p] != p:
            self.parent[p] = self.parent[self.parent[p]]
            p = self.parent[p]
        return p
    
class Solution:
    def smallestEquivalentString(self, s1: str, s2: str, baseStr: str) -> str:
        uf = UF(26)
        for a, b in zip(s1, s2):
            uf.union(ord(a) - ord('a'), ord(b) - ord('a'))
        return ''.join(chr(uf.find(ord(x) - ord('a')) + ord('a')) for x in baseStr)
```
