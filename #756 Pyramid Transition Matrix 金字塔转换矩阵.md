# 756 Pyramid Transition Matrix 金字塔转换矩阵

__Description__:
We are stacking blocks to form a pyramid. Each block has a color which is a one-letter string.

We are allowed to place any color block C on top of two adjacent blocks of colors A and B, if and only if ABC is an allowed triple.

We start with a bottom row of bottom, represented as a single string. We also start with a list of allowed triples allowed. Each allowed triple is represented as a string of length 3.

Return true if we can build the pyramid all the way to the top, otherwise false.

__Example:__

Example 1:

Input: bottom = "BCD", allowed = ["BCG","CDE","GEA","FFF"]
Output: true
Explanation:
We can stack the pyramid like this:

```text
    A
   / \
  G   E
 / \ / \
B   C   D
```

We are allowed to place G on top of B and C because BCG is an allowed triple.  Similarly, we can place E on top of C and D, then A on top of G and E.

Example 2:

Input: bottom = "AABA", allowed = ["AAA","AAB","ABA","ABB","BAC"]
Output: false
Explanation:
We cannot stack the pyramid to the top.
Note that there could be allowed triples (A, B, C) and (A, B, D) with C != D.

__Constraints:__

2 <= bottom.length <= 8
0 <= allowed.length <= 200
allowed[i].length == 3
The letters in all input strings are from the set {'A', 'B', 'C', 'D', 'E', 'F', 'G'}.

__题目描述__:
现在，我们用一些方块来堆砌一个金字塔。 每个方块用仅包含一个字母的字符串表示。

使用三元组表示金字塔的堆砌规则如下：

对于三元组 ABC ，C 为顶层方块，方块 A 、B 分别作为方块 C 下一层的的左、右子块。当且仅当 ABC 是被允许的三元组，我们才可以将其堆砌上。

初始时，给定金字塔的基层 bottom，用一个字符串表示。一个允许的三元组列表 allowed，每个三元组用一个长度为 3 的字符串表示。

如果可以由基层一直堆到塔尖就返回 true ，否则返回 false 。

__示例 :__

示例 1：

输入：bottom = "BCD", allowed = ["BCG", "CDE", "GEA", "FFF"]
输出：true
解释：
可以堆砌成这样的金字塔:

```text
    A
   / \
  G   E
 / \ / \
B   C   D
```

因为符合 BCG、CDE 和 GEA 三种规则。

示例 2：

输入：bottom = "AABA", allowed = ["AAA", "AAB", "ABA", "ABB", "BAC"]
输出：false
解释：
无法一直堆到塔尖。
注意, 允许存在像 ABC 和 ABD 这样的三元组，其中 C != D。

__提示:__

bottom 的长度范围在 [2, 8]。
allowed 的长度范围在[0, 200]。
方块的标记字母范围为{'A', 'B', 'C', 'D', 'E', 'F', 'G'}。

__思路__:

DFS
先将 allowed 数组转化为 map 方便快速查找
dfs(last, cur) 中 last 表示下一行, cur 表示这一行
每次搜索一行, 比如: BCD -> GE -> A
当最后一行是长度为 1 的字符串的时候返回 true
对每层字符串, 每次取出长度为 2 的字符串 child
去 map 中查找 child 对应所有的字符加到 cur 中, map 中不存在 child 的 key 时, 直接返回 false
当该行的字符串 cur 的长度增加比和下一行的字符串 last 的长度少 1 时, 说明这一行已经生成完成, 需要查看上一行, 这时应该调用 dfs(cur, "")
只要有一条路径返回 true, 直接返回 true
时间复杂度为 O(m ^ n), 空间复杂度为 O(n), n 为 allowed 数组的长度, m 为 bottom 的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool pyramidTransition(string bottom, vector<string>& allowed) 
    {
        for (const auto& word : allowed) m[word.substr(0, 2)].insert(word.back());
        return dfs(bottom, "");
    }
private:
    unordered_map<string, unordered_set<char>> m;
    bool dfs(string& last, string cur) 
    {
        if (last.size() == 1) return true;
        if (last.size() - cur.size() == 1) return dfs(cur, "");
        string child = last.substr(cur.size(), 2);
        if (!m.count(child)) return false;
        for (const auto& c : m[child]) if (dfs(last, cur + c)) return true;
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    private Map<String, Set<Character>> map;
    public boolean pyramidTransition(String bottom, List<String> allowed) {
        map = new HashMap<>();
        for (String word : allowed) {
            String child = word.substring(0, 2);
            if (!map.containsKey(child)) map.put(child, new HashSet<>());
            map.get(child).add(word.charAt(2));
        }
        return dfs(bottom, "");
    }
    
    private boolean dfs(String last, String cur) {
        if (last.length() == 1) return true;
        if (last.length() - cur.length() == 1) return dfs(cur, "");
        String child = last.substring(cur.length(), cur.length() + 2);
        if (!map.containsKey(child)) return false;
        for (Character c : map.get(child)) if (dfs(last, cur + c)) return true;
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def pyramidTransition(self, bottom: str, allowed: List[str]) -> bool:
        d = defaultdict(set)
        for u, v, w in allowed:
            d[u, v].add(w)

        def solve(a: str) -> bool:
            return len(a) == 1 or any(solve(tri) for tri in build(a, []))

        def build(a: str, cur: List[str], i: int=0) -> None:
            if i + 1 == len(a):
                yield "".join(cur)
            else:
                for w in d[a[i], a[i + 1]]:
                    cur.append(w)
                    for result in build(a, cur, i + 1):
                        yield result
                    cur.pop()
        return solve(bottom)
```
