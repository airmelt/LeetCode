# 1023 Camelcase Matching 驼峰式匹配

__Description__:
Given an array of strings queries and a string pattern, return a boolean array answer where answer[i] is true if queries[i] matches pattern, and false otherwise.

A query word queries[i] matches pattern if you can insert lowercase English letters pattern so that it equals the query. You may insert each character at any position and you may not insert any characters.

__Example:__

Example 1:

Input: queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FB"
Output: [true,false,true,true,false]
Explanation: "FooBar" can be generated like this "F" + "oo" + "B" + "ar".
"FootBall" can be generated like this "F" + "oot" + "B" + "all".
"FrameBuffer" can be generated like this "F" + "rame" + "B" + "uffer".

Example 2:

Input: queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FoBa"
Output: [true,false,true,false,false]
Explanation: "FooBar" can be generated like this "Fo" + "o" + "Ba" + "r".
"FootBall" can be generated like this "Fo" + "ot" + "Ba" + "ll".

Example 3:

Input: queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FoBaT"
Output: [false,true,false,false,false]
Explanation: "FooBarTest" can be generated like this "Fo" + "o" + "Ba" + "r" + "T" + "est".

__Constraints:__

1 <= pattern.length, queries.length <= 100
1 <= queries[i].length <= 100
queries[i] and pattern consist of English letters.

__题目描述__:
如果我们可以将小写字母插入模式串 pattern 得到待查询项 query，那么待查询项与给定模式串匹配。（我们可以在任何位置插入每个字符，也可以插入 0 个字符。）

给定待查询列表 queries，和模式串 pattern，返回由布尔值组成的答案列表 answer。只有在待查项 queries[i] 与模式串 pattern 匹配时， answer[i] 才为 true，否则为 false。

__示例 :__

示例 1：

输入：queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FB"
输出：[true,false,true,true,false]
示例：
"FooBar" 可以这样生成："F" + "oo" + "B" + "ar"。
"FootBall" 可以这样生成："F" + "oot" + "B" + "all".
"FrameBuffer" 可以这样生成："F" + "rame" + "B" + "uffer".

示例 2：

输入：queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FoBa"
输出：[true,false,true,false,false]
解释：
"FooBar" 可以这样生成："Fo" + "o" + "Ba" + "r".
"FootBall" 可以这样生成："Fo" + "ot" + "Ba" + "ll".

示例 3：

输出：queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FoBaT"
输入：[false,true,false,false,false]
解释：
"FooBarTest" 可以这样生成："Fo" + "o" + "Ba" + "r" + "T" + "est".

__提示:__

1 <= queries.length <= 100
1 <= queries[i].length <= 100
1 <= pattern.length <= 100
所有字符串都仅由大写和小写英文字母组成。

__思路__:

双指针
index 指针指向 pattern 的位置
i 指针指向待查询的字符串 p 的位置
如果匹配, 两个指针都向后移动
如果不匹配且待查询的字符串 p 对应位置为大写字母返回 false
否则匹配 p 的下一个字符
时间复杂度为 O(mn), 空间复杂度为 O(1), 其中 n 表示 queries 的大小, m 表示待查询的字符串的大小

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<bool> camelMatch(vector<string>& queries, string pattern)
    {
        vector<bool> result;
        for (const auto& q : queries) result.emplace_back(helper(q, pattern));
        return result;
    }
private:
    bool helper(const string& q, const string& pattern) 
    {
        int m = q.size(), n = pattern.size(), index = 0;
        for (int i = 0; i < m; i++) 
        {
            if (index < n and q[i] == pattern[index]) ++index;
            else if (q[i] >= 'A' and q[i] <= 'Z') return false;
        }
        return index == n;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Boolean> camelMatch(String[] queries, String pattern) {
        List<Boolean> result = new LinkedList<>();
        for (String q : queries) result.add(helper(q, pattern));
        return result;
    }
    
    private boolean helper(String q, String pattern) {
        int m = q.length(), n = pattern.length(), index = 0;
        for (int i = 0; i < m; i++) {
            if (index < n && q.charAt(i) == pattern.charAt(index)) ++index;
            else if (q.charAt(i) >= 'A' && q.charAt(i) <= 'Z') return false;
        }
        return index == n;
    }
}
```

__Python__:

```Python
class Solution:
    def camelMatch(self, queries: List[str], pattern: str) -> List[bool]:
        return list(map(lambda q: bool(re.match((r := re.compile(f'{"[a-z]*".join(["", *pattern, ""])}$')), q)), queries))
```
