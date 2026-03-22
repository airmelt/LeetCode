# 990 Satisfiability of Equality Equations 等式方程的可满足性

__Description__:
You are given an array of strings equations that represent relationships between variables where each string equations[i] is of length 4 and takes one of two different forms: "xi==yi" or "xi!=yi".Here, xi and yi are lowercase letters (not necessarily different) that represent one-letter variable names.

Return true if it is possible to assign integers to variable names so as to satisfy all the given equations, or false otherwise.

__Example:__

Example 1:

Input: equations = ["a==b","b!=a"]
Output: false
Explanation: If we assign say, a = 1 and b = 1, then the first equation is satisfied, but not the second.
There is no way to assign the variables to satisfy both equations.

Example 2:

Input: equations = ["b==a","a==b"]
Output: true
Explanation: We could assign a = 1 and b = 1 to satisfy both equations.

__Constraints:__

1 <= equations.length <= 500
equations[i].length == 4
equations[i][0] is a lowercase letter.
equations[i][1] is either '=' or '!'.
equations[i][2] is '='.
equations[i][3] is a lowercase letter.

__题目描述__:
给定一个由表示变量之间关系的字符串方程组成的数组，每个字符串方程 equations[i] 的长度为 4，并采用两种不同的形式之一："a==b" 或 "a!=b"。在这里，a 和 b 是小写字母（不一定不同），表示单字母变量名。

只有当可以将整数分配给变量名，以便满足所有给定的方程时才返回 true，否则返回 false。

__示例 :__

示例 1：

输入：["a==b","b!=a"]
输出：false
解释：如果我们指定，a = 1 且 b = 1，那么可以满足第一个方程，但无法满足第二个方程。没有办法分配变量同时满足这两个方程。

示例 2：

输入：["b==a","a==b"]
输出：true
解释：我们可以指定 a = 1 且 b = 1 以满足满足这两个方程。

示例 3：

输入：["a==b","b==c","a==c"]
输出：true

示例 4：

输入：["a==b","b!=c","c==a"]
输出：false

示例 5：

输入：["c==c","b==d","x!=z"]
输出：true

__提示:__

1 <= equations.length <= 500
equations[i].length == 4
equations[i][0] 和 equations[i][3] 是小写字母
equations[i][1] 要么是 '='，要么是 '!'
equations[i][2] 是 '='

__思路__:

并查集
将相等的元素放在同一个集合中
查找不相等的元素不能在一个集合中
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool equationsPossible(vector<string>& equations) 
    {
        vector<int> parent(26);
        for (int i = 0; i < 26; i++) parent[i] = i;
        for (const auto& e : equations) if (e[1] == '=') parent[find(parent, e[0] - 'a')] = find(parent, e[3] - 'a');
        for (const auto& e : equations) if (e[1] == '!') if (find(parent, e[0] - 'a') == find(parent, e[3] - 'a')) return false;
        return true;
    }
private:
    int find(vector<int>& parent, int p) 
    {
        return parent[p] == p ? p : (find(parent, parent[p] = parent[parent[p]]));
    }
};
```

__Java__:

```Java
class Solution {
    public boolean equationsPossible(String[] equations) {
        int[] parent = new int[26];
        for (int i = 0; i < 26; i++) parent[i] = i;
        for (String e : equations) if (e.charAt(1) == '=') parent[find(parent, e.charAt(0) - 'a')] = find(parent, e.charAt(3) - 'a');
        for (String e : equations) if (e.charAt(1) == '!') if (find(parent, e.charAt(0) - 'a') == find(parent, e.charAt(3) - 'a')) return false;
        return true;
    }
    
    private int find(int[] parent, int p) {
        return parent[p] == p ? p : (find(parent, parent[p] = parent[parent[p]]));
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
        if p == q:
            return
        if self.weight[p] > self.weight[q]:
            self.parent[q] = p
            self.weight[p] += self.weight[q]
        else:
            self.parent[p] = q
            self.weight[q] += self.weight[p]
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
    def equationsPossible(self, equations: List[str]) -> bool:
        uf = UF(26)
        for e in equations:
            if e[1] == '=':
                if (p := uf.find(ord(e[0]) - ord('a'))) != (q := uf.find(ord(e[3]) - ord('a'))):
                    uf.union(p, q)
        return all(uf.find(ord(e[0]) - ord('a')) != uf.find(ord(e[3]) - ord('a')) for e in equations if e[1] == '!')
```
