# 839 Similar String Groups 相似字符串组

__Description__:
Two strings X and Y are similar if we can swap two letters (in different positions) of X, so that it equals Y. Also two strings X and Y are similar if they are equal.

For example, "tars" and "rats" are similar (swapping at positions 0 and 2), and "rats" and "arts" are similar, but "star" is not similar to "tars", "rats", or "arts".

Together, these form two connected groups by similarity: {"tars", "rats", "arts"} and {"star"}.  Notice that "tars" and "arts" are in the same group even though they are not similar.  Formally, each group is such that a word is in the group if and only if it is similar to at least one other word in the group.

We are given a list strs of strings where every string in strs is an anagram of every other string in strs. How many groups are there?

__Example:__

Example 1:

Input: strs = ["tars","rats","arts","star"]
Output: 2

Example 2:

Input: strs = ["omv","ovm"]
Output: 1

__Constraints:__

1 <= strs.length <= 300
1 <= strs[i].length <= 300
strs[i] consists of lowercase letters only.
All words in strs have the same length and are anagrams of each other.

__题目描述__:
如果交换字符串 X 中的两个不同位置的字母，使得它和字符串 Y 相等，那么称 X 和 Y 两个字符串相似。如果这两个字符串本身是相等的，那它们也是相似的。

例如，"tars" 和 "rats" 是相似的 (交换 0 与 2 的位置)； "rats" 和 "arts" 也是相似的，但是 "star" 不与 "tars"，"rats"，或 "arts" 相似。

总之，它们通过相似性形成了两个关联组：{"tars", "rats", "arts"} 和 {"star"}。注意，"tars" 和 "arts" 是在同一组中，即使它们并不相似。形式上，对每个组而言，要确定一个单词在组中，只需要这个词和该组中至少一个单词相似。

给你一个字符串列表 strs。列表中的每个字符串都是 strs 中其它所有字符串的一个字母异位词。请问 strs 中有多少个相似字符串组？

__示例 :__

示例 1：

输入：strs = ["tars","rats","arts","star"]
输出：2

示例 2：

输入：strs = ["omv","ovm"]
输出：1

__提示:__

1 <= strs.length <= 300
1 <= strs[i].length <= 300
strs[i] 只包含小写字母。
strs 中的所有单词都具有相同的长度，且是彼此的字母异位词。

__备注：__
字母异位词（anagram），一种把某个字符串的字母的位置（顺序）加以改换所形成的新词。

__思路__:

并查集
将相似的字符串放在并查集的同一组中
检查是否相似只需要比较两个字符串字符不同的数目是否不大于 2
时间复杂度为 O(mn ^ 2), 空间复杂度为 O(n), n 为字符串的数量, m 为字符串的长度

__代码__:
__C++__:

```C++
class Solution 
{
private:
    vector<int> f;

    int find(int x) 
    {
        return f[x] == x ? x : f[x] = find(f[x]);
    }

    bool check(const string &a, const string &b, int len) 
    {
        int num = 0;
        for (int i = 0; i < len; i++) 
        {
            num += (a[i] != b[i]);
            if (num > 2) return false;
        }
        return true;
    }
public:
    int numSimilarGroups(vector<string> &strs) 
    {
        int n = strs.size(), m = strs[0].length(), result = 0;
        f.resize(n);
        for (int i = 0; i < n; i++) f[i] = i;
        for (int i = 0; i < n; i++) 
        {
            for (int j = i + 1; j < n; j++) 
            {
                int fi = find(i), fj = find(j);
                if (fi == fj) continue;
                if (check(strs[i], strs[j], m)) f[fi] = fj;
            }
        }
        for (int i = 0; i < n; i++) result += (f[i] == i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    int f[], m;
    public int numSimilarGroups(String[] strs) {
        int n = strs.length, result = 0;
        f = new int[n];
        m = strs[0].length();
        for (int i = 1; i < n; i++) f[i] = i;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int fi = find(i), fj = find(j);
                if (fi == fj) continue;
                if (check(strs[i], strs[j])) f[fi] = fj;
            }
        }
        for (int i = 0; i < n; i++) result += (f[i] == i ? 1 : 0);
        return result;
    }
    
    private int find(int x) {
        return (f[x] == x) ? x : (f[x] = find(f[x]));
    }
    
    private boolean check(String a, String b) {
        int num = 0;
        for (int i = 0; i < m; i++) {
            num += (a.charAt(i) == b.charAt(i) ? 0 : 1);
            if (num > 2) return false;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def numSimilarGroups(self, strs: List[str]) -> int:
        f, n, m = list(range(len(strs))), len(strs), len(strs[0])
        
        def find(x: int) -> int:
            while x != f[x]:
                x, f[x] = f[x], find(f[x])
            return x
        
        for i in range(n):
            for j in range(i + 1, n):
                if (fi := find(i)) == (fj := find(j)):
                    continue
                if sum(a != b for a, b in zip(strs[i], strs[j])) < 3:
                    f[fj] = fi
        return sum(f[i] == i for i in range(n))
```
