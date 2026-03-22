# 1202 Smallest String With Swaps  交换字符串中的元素

__Description__:
You are given a string s, and an array of pairs of indices in the string pairs where pairs[i] = [a, b] indicates 2 indices(0-indexed) of the string.

You can swap the characters at any pair of indices in the given pairs any number of times.

Return the lexicographically smallest string that s can be changed to after using the swaps.

__Example:__

Example 1:

Input: s = "dcab", pairs = [[0,3],[1,2]]
Output: "bacd"
Explaination:
Swap s[0] and s[3], s = "bcad"
Swap s[1] and s[2], s = "bacd"

Example 2:

Input: s = "dcab", pairs = [[0,3],[1,2],[0,2]]
Output: "abcd"
Explaination:
Swap s[0] and s[3], s = "bcad"
Swap s[0] and s[2], s = "acbd"
Swap s[1] and s[2], s = "abcd"

Example 3:

Input: s = "cba", pairs = [[0,1],[1,2]]
Output: "abc"
Explaination:
Swap s[0] and s[1], s = "bca"
Swap s[1] and s[2], s = "bac"
Swap s[0] and s[1], s = "abc"

__Constraints:__

1 <= s.length <= 10^5
0 <= pairs.length <= 10^5
0 <= pairs[i][0], pairs[i][1] < s.length
s only contains lower case English letters.

__题目描述__:
给你一个字符串 s，以及该字符串中的一些「索引对」数组 pairs，其中 pairs[i] = [a, b] 表示字符串中的两个索引（编号从 0 开始）。

你可以 任意多次交换 在 pairs 中任意一对索引处的字符。

返回在经过若干次交换后，s 可以变成的按字典序最小的字符串。

__示例 :__

示例 1:

输入：s = "dcab", pairs = [[0,3],[1,2]]
输出："bacd"
解释：
交换 s[0] 和 s[3], s = "bcad"
交换 s[1] 和 s[2], s = "bacd"

示例 2：

输入：s = "dcab", pairs = [[0,3],[1,2],[0,2]]
输出："abcd"
解释：
交换 s[0] 和 s[3], s = "bcad"
交换 s[0] 和 s[2], s = "acbd"
交换 s[1] 和 s[2], s = "abcd"

示例 3：

输入：s = "cba", pairs = [[0,1],[1,2]]
输出："abc"
解释：
交换 s[0] 和 s[1], s = "bca"
交换 s[1] 和 s[2], s = "bac"
交换 s[0] 和 s[1], s = "abc"

__提示:__

1 <= s.length <= 10^5
0 <= pairs.length <= 10^5
0 <= pairs[i][0], pairs[i][1] < s.length
s 中只含有小写英文字母

__思路__:

并查集
两个可交换的字符可以归并到一个并查集中
将相同并查集的字符存放到哈希表中并排序, 也可以使用优先队列
然后按照同一并查集弹出字符串到结果中
时间复杂度为 O(m + n + nlgn), 空间复杂度为 O(n), m 是 pairs 数组的长度, n 为 s 的长度

__代码__:
__C++__:

```C++
class Solution 
{
private:
    int father[100010];
    
    int find(int x)
    {
        return x == father[x] ? x : (father[x] = find(father[x]));
    }
    
    void merge(int x,int y)
    {
        father[find(x)] = find(y);
    }
public:
    string smallestStringWithSwaps(string s, vector<vector<int>>& pairs)
    {
        int n = s.size();
        unordered_map<int, vector<char>> m;
        string result;
        for (int i = 0; i < n; i++) father[i] = i;
        for (const auto& pair: pairs) merge(pair.front(), pair.back());
        for (int i = 0; i < n; i++) m[find(i)].emplace_back(s[i]);
        for (auto& [k, v] : m) sort(v.begin(), v.end(), greater<char>());
        for (int i = 0; i < n; i++) 
        {
            int x = find(i);
            result += m[x].back();
            m[x].pop_back();
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String smallestStringWithSwaps(String s, List<List<Integer>> pairs) {
        int n = s.length(), p[] = new int[n];
        for (int i = 0; i < n; i++) p[i] = i;
        for (List<Integer> pair : pairs) union(pair.get(0), pair.get(1), p);
        Map<Integer, PriorityQueue<Character>> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            int cur = find(i, p);
            if (!map.containsKey(cur)) map.put(cur, new PriorityQueue<>());
            map.get(cur).offer(s.charAt(i));
        }
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < n; i++) result.append(map.get(find(i, p)).poll());
        return result.toString();
    }
    
    private int find(int x, int[] p) {
        return p[x] == x ? x : (p[x] = find(p[x], p));
    }
    
    private void union(int x, int y, int[] p) {
        p[find(x, p)] = find(y, p);
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
    def smallestStringWithSwaps(self, s: str, pairs: List[List[int]]) -> str:
        uf, d, result = UF((n := len(s))), defaultdict(list), []
        for x, y in pairs:
            uf.union(x, y)
        for i, c in enumerate(s):
            d[uf.find(i)].append(c)
        for vec in d.values():
            vec.sort(reverse=True)
        for i in range(n):
            result.append(d[x := uf.find(i)][-1])
            d[x].pop()
        return "".join(result)
```
