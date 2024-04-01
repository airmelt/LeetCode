# 2055 Plates Between Candles 蜡烛之间的盘子

__Description:__

There is a long table with a line of plates and candles arranged on top of it. You are given a __0-indexed__ string `s` consisting of characters `'*'` and `'|'` only, where a `'*'` represents a __plate__ and a `'|'` represents a __candle__.

You are also given a __0-indexed__ 2D integer array `queries` where `queries[i] = [lefti, righti]` denotes the __substring__ `s[lefti...righti]` (__inclusive__). For each query, you need to find the __number__ of plates __between candles__ that are __in the substring__. A plate is considered __between candles__ if there is at least one candle to its left __and__ at least one candle to its right __in the substring__.

- For example, `s = "||**||**|*"`, and a query `[3, 8]` denotes the substring `"*||**|"`. The number of plates between candles in this substring is `2`, as each of the two plates has at least one candle __in the substring__ to its left __and__ right.

Return _an integer array_ `answer` _where_ `answer[i]` _is the answer to the_ `i ^ th` _query_.

__Example:__

Example 1:

![2055-1](https://assets.leetcode.com/uploads/2021/10/04/ex-1.png)

```text
Input: s = "**|**|***|", queries = [[2,5],[5,9]]
Output: [2,3]
Explanation:
```

- queries[0] has two plates between candles.
- queries[1] has three plates between candles.

Example 2:

![2055-2](https://assets.leetcode.com/uploads/2021/10/04/ex-2.png)

```text
Input: s = "***|**|*****|**||**|*", queries = [[1,17],[4,5],[14,17],[5,11],[15,16]]
Output: [9,0,0,0,0]
Explanation:
```

- queries[0] has nine plates between candles.
- The other queries have zero plates between candles.

__Constraints:__

- `3 <= s.length <= 10 ^ 5`
- `s` consists of `'*'` and `'|'` characters.
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length == 2`
- `0 <= lefti <= righti < s.length`

__题目描述:__

给你一个长桌子，桌子上盘子和蜡烛排成一列。给你一个下标从 __0__ 开始的字符串 `s` ，它只包含字符 `'*'` 和 `'|'` ，其中 `'*'` 表示一个 __盘子__ ， `'|'` 表示一支 __蜡烛__ 。

同时给你一个下标从 __0__ 开始的二维整数数组 `queries` ，其中 `queries[i] = [lefti, righti]` 表示 __子字符串__ `s[lefti...righti]` （__包含左右端点的字符__）。对于每个查询，你需要找到 __子字符串中__ 在 __两支蜡烛之间__ 的盘子的 _数目_ 。如果一个盘子在 __子字符串中__ 左边和右边 __都__ 至少有一支蜡烛，那么这个盘子满足在 __两支蜡烛之间__ 。

- 比方说， `s = "||**||**|*"` ，查询 `[3, 8]` ，表示的是子字符串 `"*||**|"` 。子字符串中在两支蜡烛之间的盘子数目为 `2` ，子字符串中右边两个盘子在它们左边和右边 __都__ 至少有一支蜡烛。

请你返回一个整数数组 `answer` ，其中 `answer[i]` 是第 `i` 个查询的答案。

__示例:__

示例 1:

![2055-3](https://assets.leetcode.com/uploads/2021/10/04/ex-1.png)

```text
输入：s = "**|**|***|", queries = [[2,5],[5,9]]
输出：[2,3]
解释：
```

- queries[0] 有两个盘子在蜡烛之间。
- queries[1] 有三个盘子在蜡烛之间。

示例 2:

![2055-4](https://assets.leetcode.com/uploads/2021/10/04/ex-2.png)

```text
输入：s = "***|**|*****|**||**|*", queries = [[1,17],[4,5],[14,17],[5,11],[15,16]]
输出：[9,0,0,0,0]
解释：
```

- queries[0] 有 9 个盘子在蜡烛之间。
- 另一个查询没有盘子在蜡烛之间。

__提示：__

- `3 <= s.length <= 10 ^ 5`
- `s` 只包含字符 `'*'` 和 `'|'` 。
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length == 2`
- `0 <= lefti <= righti < s.length`

__思路:__

```text
前缀和
用前缀和统计蜡烛的个数
用两个数组 left 和 right 记录当前位置左边/右边的盘子的位置
如果当前位置的左右两边都有盘子
则将前缀和的差值记录为结果
时间复杂度为 O(N + M), 空间复杂度为 O(N), 其中 N 为字符串 s 的长度, M 为查询数组 queries 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> platesBetweenCandles(string s, vector<vector<int>>& queries) 
    {
        int n = s.size(), m = queries.size();
        vector<int> left(n), right(n), result(m), pre(n + 1);
        for (int i = 0, j = n - 1, p = -1, q = -1; i < n; i++, j--) 
        {
            if (s[i] == '|') p = i;
            if (s[j] == '|') q = j;
            left[i] = p;
            right[j] = q;
            pre[i + 1] = pre[i] + (s[i] == '*');
        }
        for (int i = 0; i < m; i++) if (right[queries[i][0]] != -1 and right[queries[i][0]] <= left[queries[i][1]]) result[i] = pre[left[queries[i][1]] + 1] - pre[right[queries[i][0]]];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] platesBetweenCandles(String s, int[][] queries) {
        int n = s.length(), m = queries.length, left[] = new int[n], right[] = new int[n], pre[] = new int[n + 1], result[] = new int[m];
        for (int i = 0, j = n - 1, p = -1, q = -1; i < n; j--) {
            if (s.charAt(i) == '|') p = i;
            if (s.charAt(j) == '|') q = j;
            left[i] = p;
            right[j] = q;
            pre[i + 1] = pre[i] + (s.charAt(i++) == '*' ? 1 : 0);
        }
        for (int i = 0; i < m; i++) if (right[queries[i][0]] != -1 && right[queries[i][0]] <= left[queries[i][1]]) result[i] = pre[left[queries[i][1]] + 1] - pre[right[queries[i][0]]];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def platesBetweenCandles(self, s: str, queries: List[List[int]]) -> List[int]:
        n, result = len(s), []
        left, right, pre = [-1 if s[0] != '|' else 0] + [-1] * (n - 1), [-1] * (n - 1) + [-1 if s[n - 1] != '|' else n - 1], [0] * n
        for i in range(1, n):
            left[i], right[n - i - 1], pre[i] = i if s[i] == '|' else left[i - 1], n - i - 1 if s[n - i - 1] == '|' else right[n - i], pre[i - 1] + 1 if s[i - 1] == '*' else pre[i - 1]
        for q in queries:
            result.append(0 if (right_index := left[(r := q[1])]) <= (l := q[0]) + 1 or (left_index := right[l]) >= r - 1 else pre[right_index] - pre[left_index])
        return result
```
