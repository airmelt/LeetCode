# 2213 Longest Substring of One Repeating Character 由单个字符重复的最长子字符串

__Description:__

You are given a __0-indexed__ string `s`. You are also given a __0-indexed__ string `queryCharacters` of length `k` and a __0-indexed__ array of integer __indices__ `queryIndices` of length `k`, both of which are used to describe `k` queries.

The `i ^ th` query updates the character in `s` at index `queryIndices[i]` to the character `queryCharacters[i]`.

Return _an array_ `lengths` _of length_ `k` _where_ `lengths[i]` _is the __length__ of the __longest substring__ of_ `s` _consisting of __only one repeating__ character __after__ the_ `i ^ th` _query_ _is performed._

__Example:__

Example 1:

```text
Input: s = "babacc", queryCharacters = "bcb", queryIndices = [1,3,3]
Output: [3,3,4]
Explanation: 
```

- 1st query updates s = "bbbacc". The longest substring consisting of one repeating character is "bbb" with length 3.
- 2nd query updates s = "bbbccc".
  The longest substring consisting of one repeating character can be "bbb" or "ccc" with length 3.
- 3rd query updates s = "bbbbcc". The longest substring consisting of one repeating character is "bbbb" with length 4.

Thus, we return [3,3,4].

Example 2:

```text
Input: s = "abyzz", queryCharacters = "aa", queryIndices = [2,1]
Output: [2,3]
Explanation:
```

- 1st query updates s = "abazz". The longest substring consisting of one repeating character is "zz" with length 2.
- 2nd query updates s = "aaazz". The longest substring consisting of one repeating character is "aaa" with length 3.

Thus, we return [2,3].

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists of lowercase English letters.
- `k == queryCharacters.length == queryIndices.length`
- `1 <= k <= 10 ^ 5`
- `queryCharacters` consists of lowercase English letters.
- `0 <= queryIndices[i] < s.length`

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `s` 。另给你一个下标从 __0__ 开始、长度为 `k` 的字符串 `queryCharacters` ，一个下标从 `0` 开始、长度也是 `k` 的整数 __下标__ 数组 `queryIndices` ，这两个都用来描述 `k` 个查询。

第 `i` 个查询会将 `s` 中位于下标 `queryIndices[i]` 的字符更新为 `queryCharacters[i]` 。

返回一个长度为 `k` 的数组 `lengths` ，其中 `lengths[i]` 是在执行第 `i` 个查询 __之后__ `s` 中仅由 __单个字符重复__ 组成的 __最长子字符串__ 的 __长度__ _。_

__示例:__

示例 1：

```text
输入：s = "babacc", queryCharacters = "bcb", queryIndices = [1,3,3]
输出：[3,3,4]
解释：
```

- 第 1 次查询更新后 s = "bbbacc" 。由单个字符重复组成的最长子字符串是 "bbb" ，长度为 3 。
- 第 2 次查询更新后 s = "bbbccc" 。由单个字符重复组成的最长子字符串是 "bbb" 或 "ccc"，长度为 3 。
- 第 3 次查询更新后 s = "bbbbcc" 。由单个字符重复组成的最长子字符串是 "bbbb" ，长度为 4 。

因此，返回 [3,3,4] 。

示例 2：

```text
输入：s = "abyzz", queryCharacters = "aa", queryIndices = [2,1]
输出：[2,3]
解释：
```

- 第 1 次查询更新后 s = "abazz" 。由单个字符重复组成的最长子字符串是 "zz" ，长度为 2 。
- 第 2 次查询更新后 s = "aaazz" 。由单个字符重复组成的最长子字符串是 "aaa" ，长度为 3 。

因此，返回 [2,3] 。

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 由小写英文字母组成
- `k == queryCharacters.length == queryIndices.length`
- `1 <= k <= 10 ^ 5`
- `queryCharacters` 由小写英文字母组成
- `0 <= queryIndices[i] < s.length`

__思路:__

```text
线段树
维护一个线段树
pre 表示前缀最长重复的长度
suf 表示后缀最长重复的长度
mx 表示区间内最长重复的长度
如果左右子节点的最长重复长度相等, 则合并
对于区间 a, b
如果 a 的后缀长度等于 a 的长度, 将 b 的前缀长度加到 a 的前缀长度上
如果 b 的前缀长度等于 b 的长度, 将 a 的后缀长度加到 b 的后缀长度上
mx = suf[a] + pre[b]
时间复杂度为 O(N + MlogN), 空间复杂度为 O(N), 其中 N 为字符串的长度, M 为查询的次数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> longestRepeating(string &s, string &queryCharacters, vector<int> &queryIndices) 
    {
        this -> s = s;
        int n = s.size(), m = queryIndices.size();
        pre.resize(n << 2);
        suf.resize(n << 2);
        mx.resize(n << 2);
        build(1, 1, n);
        vector<int> result(m);
        for (int i = 0; i < m; i++) 
        {
            this -> s[queryIndices[i]] = queryCharacters[i];
            update(1, 1, n, queryIndices[i] + 1);
            result[i] = mx[1];
        }
        return result;
    }
private:
    string s;
    vector<int> pre, suf, mx;

    void maintain(int o, int l, int r) 
    {
        pre[o] = pre[o << 1];
        suf[o] = suf[o << 1 | 1];
        mx[o] = max(mx[o << 1], mx[o << 1 | 1]);
        int m = (l + r) >> 1;
        if (s[m - 1] == s[m]) 
        {
            if (suf[o << 1] == m - l + 1) pre[o] += pre[o << 1 | 1];
            if (pre[o << 1 | 1] == r - m) suf[o] += suf[o << 1];
            mx[o] = max(mx[o], suf[o << 1] + pre[o << 1 | 1]);
        }
    }

    void build(int o, int l, int r) 
    {
        if (l == r) 
        {
            pre[o] = suf[o] = mx[o] = 1;
            return;
        }
        int m = (l + r) >> 1;
        build(o << 1, l, m);
        build(o << 1 | 1, m + 1, r);
        maintain(o, l, r);
    }

    void update(int o, int l, int r, int i) 
    {
        if (l == r) return;
        int m = (l + r) >> 1;
        if (i <= m) update(o << 1, l, m, i);
        else update(o << 1 | 1, m + 1, r, i);
        maintain(o, l, r);
    }
};
```

__Java__:

```Java
class Solution {
    private char[] s;
    private int[] pre, suf, max;

    private void maintain(int o, int l, int r) {
        pre[o] = pre[o << 1];
        suf[o] = suf[o << 1 | 1];
        max[o] = Math.max(max[o << 1], max[o << 1 | 1]);
        var m = (l + r) >>> 1;
        if (s[m - 1] == s[m]) {
            if (suf[o << 1] == m - l + 1) pre[o] += pre[o << 1 | 1];
            if (pre[o << 1 | 1] == r - m) suf[o] += suf[o << 1];
            max[o] = Math.max(max[o], suf[o << 1] + pre[o << 1 | 1]);
        }
    }

    private void build(int o, int l, int r) {
        if (l == r) {
            pre[o] = suf[o] = max[o] = 1;
            return;
        }
        var m = (l + r) >>> 1;
        build(o << 1, l, m);
        build(o << 1 | 1, m + 1, r);
        maintain(o, l, r);
    }

    private void update(int o, int l, int r, int i) {
        if (l == r) return;
        var m = (l + r) >>> 1;
        if (i <= m) update(o << 1, l, m, i);
        else update(o << 1 | 1, m + 1, r, i);
        maintain(o, l, r);
    }

    public int[] longestRepeating(String s, String queryCharacters, int[] queryIndices) {
        this.s = s.toCharArray();
        int n = this.s.length, m = queryIndices.length, result[] = new int[m];
        pre = new int[n << 2];
        suf = new int[n << 2];
        max = new int[n << 2];
        build(1, 1, n);
        for (var i = 0; i < m; i++) {
            this.s[queryIndices[i]] = queryCharacters.charAt(i);
            update(1, 1, n, queryIndices[i] + 1);
            result[i] = max[1];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestRepeating(self, s: str, queryCharacters: str, queryIndices: List[int]) -> List[int]:
        s, pre, suf, mx, result = list(s), [0] * ((n := len(s)) << 2), [0] * (n << 2), [0] * (n << 2), []

        def maintain(o: int, l: int, r: int) -> NoReturn:
            pre[o] = pre[o << 1]
            suf[o] = suf[o << 1 | 1]
            mx[o] = max(mx[o << 1], mx[o << 1 | 1])
            m = (l + r) >> 1
            if s[m - 1] == s[m]:
                if suf[o << 1] == m - l + 1:
                    pre[o] += pre[o << 1 | 1]
                if pre[o << 1 | 1] == r - m:
                    suf[o] += suf[o << 1]
                mx[o] = max(mx[o], suf[o << 1] + pre[o << 1 | 1])

        def build(o: int, l: int, r: int) -> NoReturn:
            if l == r:
                pre[o] = suf[o] = mx[o] = 1
                return
            m = (l + r) >> 1
            build(o << 1, l, m)
            build(o << 1 | 1, m + 1, r)
            maintain(o, l, r)

        def update(o: int, l: int, r: int, i: int) -> NoReturn:
            if l == r: 
                return
            m = (l + r) >> 1
            if i <= m:
                update(o << 1, l, m, i)
            else:
                update(o << 1 | 1, m + 1, r, i)
            maintain(o, l, r)

        build(1, 1, n)
        for c, i in zip(queryCharacters, queryIndices):
            s[i] = c
            update(1, 1, n, i + 1)
            result.append(mx[1])
        return result
```
