# 2983 Palindrome Rearrangement Queries 回文串重新排列查询

__Description:__

You are given a __0-indexed__ string `s` having an __even__ length `n`.

You are also given a __0-indexed__ 2D integer array, `queries`, where `queries[i] = [ai, bi, ci, di]`.

For each query `i`, you are allowed to perform the following operations:

- Rearrange the characters within the __substring__ `s[ai:bi]`, where `0 <= ai <= bi < n / 2`.
- Rearrange the characters within the __substring__ `s[ci:di]`, where `n / 2 <= ci <= di < n`.

For each query, your task is to determine whether it is possible to make `s` a __palindrome__ by performing the operations.

Each query is answered independently of the others.

Return _a __0-indexed__ array_ `answer`_, where_ `answer[i] == true` _if it is possible to make_ `s` _a palindrome by performing operations specified by the_ `i ^ th` _query, and_ `false` _otherwise._

- A __substring__ is a contiguous sequence of characters within a string.
- `s[x:y]` represents the substring consisting of characters from the index `x` to index `y` in `s`, __both inclusive__.

__Example:__

Example 1:

```text
Input: s = "abcabc", queries = [[1,1,3,5],[0,2,5,5]]
Output: [true,true]
Explanation: In this example, there are two queries:
In the first query:
```

- a0 = 1, b0 = 1, c0 = 3, d0 = 5.
- So, you are allowed to rearrange s[1:1] => abcabc and s[3:5] => abcabc.
- To make s a palindrome, s[3:5] can be rearranged to become => abccba.
- Now, s is a palindrome. So, answer[0] = true.

In the second query:

- a1 = 0, b1 = 2, c1 = 5, d1 = 5.
- So, you are allowed to rearrange s[0:2] => abcabc and s[5:5] => abcabc.
- To make s a palindrome, s[0:2] can be rearranged to become => cbaabc.
- Now, s is a palindrome. So, answer[1] = true.

Example 2:

```text
Input: s = "abbcdecbba", queries = [[0,2,7,9]]
Output: [false]
Explanation: In this example, there is only one query.
a0 = 0, b0 = 2, c0 = 7, d0 = 9.
So, you are allowed to rearrange s[0:2] => abbcdecbba and s[7:9] => abbcdecbba.
It is not possible to make s a palindrome by rearranging these substrings because s[3:6] is not a palindrome.
So, answer[0] = false.
```

Example 3:

```text
Input: s = "acbcab", queries = [[1,2,4,5]]
Output: [true]
Explanation: In this example, there is only one query.
a0 = 1, b0 = 2, c0 = 4, d0 = 5.
So, you are allowed to rearrange s[1:2] => acbcab and s[4:5] => acbcab.
To make s a palindrome s[1:2] can be rearranged to become abccab.
Then, s[4:5] can be rearranged to become abccba.
Now, s is a palindrome. So, answer[0] = true.
```

__Constraints:__

- `2 <= n == s.length <= 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length == 4`
- `ai == queries[i][0], bi == queries[i][1]`
- `ci == queries[i][2], di == queries[i][3]`
- `0 <= ai <= bi < n / 2`
- `n / 2 <= ci <= di < n`
- `n` is even.
- `s` consists of only lowercase English letters.

__题目描述:__

给你一个长度为 __偶数__ `n` ，下标从 __0__ 开始的字符串 `s` 。

同时给你一个下标从 __0__ 开始的二维整数数组 `queries` ，其中 `queries[i] = [ai, bi, ci, di]` 。

对于每个查询 `i` ，你需要执行以下操作:

- 将下标在范围 `0 <= ai <= bi < n / 2` 内的 __子字符串__ `s[ai:bi]` 中的字符重新排列。
- 将下标在范围 `n / 2 <= ci <= di < n` 内的 __子字符串__ `s[ci:di]` 中的字符重新排列。

对于每个查询，你的任务是判断执行操作后能否让 `s` 变成一个 __回文串__ 。

每个查询与其他查询都是 独立的 。

请你返回一个下标从 __0__ 开始的数组 `answer` ，如果第 `i` 个查询执行操作后，可以将 `s` 变为一个回文串，那么 `answer[i] = true`，否则为 `false` 。

- __子字符串__ 指的是一个字符串中一段连续的字符序列。
- `s[x:y]` 表示 `s` 中从下标 `x` 到 `y` 且两个端点 __都包含__ 的子字符串。

__示例:__

示例 1：

```text
输入：s = "abcabc", queries = [[1,1,3,5],[0,2,5,5]]
输出：[true,true]
解释：这个例子中，有 2 个查询：
第一个查询：
```

- a0 = 1, b0 = 1, c0 = 3, d0 = 5
- 你可以重新排列 s[1:1] => abcabc 和 s[3:5] => abcabc 。
- 为了让 s 变为回文串，s[3:5] 可以重新排列得到 => abccba 。
- 现在 s 是一个回文串。所以 answer[0] = true 。

第二个查询：

- a1 = 0, b1 = 2, c1 = 5, d1 = 5.
- 你可以重新排列 s[0:2] => abcabc 和 s[5:5] => abcabc 。
- 为了让 s 变为回文串，s[0:2] 可以重新排列得到 => cbaabc 。
- 现在 s 是一个回文串，所以 answer[1] = true 。

示例 2：

```text
输入：s = "abbcdecbba", queries = [[0,2,7,9]]
输出：[false]
解释：这个示例中，只有一个查询。
a0 = 0, b0 = 2, c0 = 7, d0 = 9.
你可以重新排列 s[0:2] => abbcdecbba 和 s[7:9] => abbcdecbba 。
无法通过重新排列这些子字符串使 s 变为一个回文串，因为 s[3:6] 不是一个回文串。
所以 answer[0] = false 。
```

示例 3：

```text
输入: s = "acbcab", queries = [[1,2,4,5]]
输出: [true]
解释:
这个示例中，只有一个查询。
a0 = 1, b0 = 2, c0 = 4, d0 = 5.
```

你可以重新排列 s[1:2] => a __cb__ cab 和 s[4:5] => acbc __ab__ 。
为了让 s 变为回文串，s[1:2] 可以重新排列得到 => a __bc__ cab 。
然后 s[4:5] 重新排列得到 abcc __ba__ 。
现在 s 是一个回文串，所以 answer[0] = true 。

__提示：__

- `2 <= n == s.length <= 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length == 4`
- `ai == queries[i][0], bi == queries[i][1]`
- `ci == queries[i][2], di == queries[i][3]`
- `0 <= ai <= bi < n / 2`
- `n / 2 <= ci <= di < n`
- `n` 是一个偶数。
- `s` 只包含小写英文字母。

__思路:__

```text
前缀和
设 s 和 t 分别为 s 的前一半字符以及 s 的后一半字符的反转
记 pre_s 和 pre_t 分别为 s 和 t 的前缀和
前缀和中记录 26 的字符出现的次数
记 pre_ne 为 s 和 t 不相等的前缀和
对于查询 [a, b, c, d]
先将 c 和 d 转换为 m - 1 - c 以及 m - 1 - d
转换之后为 [left1, right1, left2, right2]
则只需要比较 s[left1:right1] 和 t[left2:right2] 重新排列之后是否相等
首先在区间外的都必须逐个字符相等, 因为无法排序
所以如果 pre_ne[left1] 或者 pre_ne.back() - pre_ne[max(right1, right2) + 1] 不为 0 直接返回 false
接下来分类讨论
如果 right2 <= right1 说明区间 [left2, right2] 被 [left1, right1] 包含, 这时只需要检查 s 和 t 在 [left1, right1] 上是否相等
若 right1 < left2 说明两个区间完全不相交则首先位于中间的元素不能排序需要逐个相等, 即 pre_ne[left2] - pre_ne[right1 + 1] 需要等于 0, 其次 s[left1:right1] == t[left1:right1] 且 s[left2:right2] == t[left2:right2] 后者只需要数量相等即可
最后是相交的情况
此时 s[left1:right1] - t[left1:left2 - 1] 和 t[left2:right2] - s[right1 + 1:right2] 必须有意义(即非负)且相等
时间复杂度为 O(CN + CQ), 空间复杂度为 O(CN), 其中 N 为字符串 s 的长度, Q 为查询数组长度, C 为字符种类, 本题为 26
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<bool> canMakePalindromeQueries(string s, vector<vector<int>> &queries) 
    {
        int m = s.size(), n = m >> 1, q = queries.size();
        vector<vector<int>> pre_s(n + 1, vector<int>(26)), pre_t(n + 1, vector<int>(26));
        for (int i = 0; i < n; i++)
        {
            pre_s[i + 1] = pre_s[i];
            ++pre_s[i + 1][s[i] - 'a'];
            pre_t[i + 1] = pre_t[i];
            ++pre_t[i + 1][s[m - i - 1] - 'a'];
        }
        vector<int> pre_ne(n + 1);
        for (int i = 0; i < n; i++) pre_ne[i + 1] = pre_ne[i] + (s[i] != s[m - i - 1]);
        auto sum_count = [](const auto& pre, int left, int right) -> vector<int>
        {
            vector<int> result = pre[right + 1];
            for (int i = 0; i < 26; i++) result[i] -= pre[left][i];
            return result;
        };
        auto subtract = [](auto s1, const auto& s2) -> vector<int>
        {
            for (int i = 0; i < 26; i++)
            {
                s1[i] -= s2[i];
                if (s1[i] < 0) return {};
            }
            return s1;
        };
        auto check = [&](int left1, int right1, int left2, int right2, const auto& sum_s, const auto& sum_t) -> bool 
        {
            if (pre_ne[left1] or pre_ne.back() - pre_ne[max(right1, right2) + 1]) return false;
            if (right2 <= right1) return sum_count(sum_s, left1, right1) == sum_count(sum_t, left1, right1);
            if (right1 < left2) return !(pre_ne[left2] - pre_ne[right1 + 1]) and sum_count(sum_s, left1, right1) == sum_count(sum_t, left1, right1) and sum_count(sum_s, left2, right2) == sum_count(sum_t, left2, right2);
            auto s1 = subtract(sum_count(sum_s, left1, right1), sum_count(sum_t, left1, left2 - 1)), s2 = subtract(sum_count(sum_t, left2, right2), sum_count(sum_s, right1 + 1, right2));
            return !s1.empty() and !s2.empty() and s1 == s2;
        };
        vector<bool> result(q);
        for (int i = 0; i < q; i++) result[i] = queries[i][0] <= (m - 1 - queries[i][3]) ? check(queries[i][0], queries[i][1], m - 1 - queries[i][3], m - 1 - queries[i][2], pre_s, pre_t) : check(m - 1 - queries[i][3], m - 1 - queries[i][2], queries[i][0], queries[i][1], pre_t, pre_s);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean[] canMakePalindromeQueries(String s, int[][] queries) {
        int m = s.length(), n = m >>> 1, preS[][] = new int[n + 1][26], preT[][] = new int[n + 1][26], preNe[] = new int[n + 1], q = queries.length;
        for (int i = 0; i < n; i++) {
            preS[i + 1] = preS[i].clone();
            ++preS[i + 1][s.charAt(i) - 'a'];
            preT[i + 1] = preT[i].clone();
            ++preT[i + 1][s.charAt(m - 1 - i) - 'a'];
        }
        for (int i = 0; i < n; i++) preNe[i + 1] = preNe[i] + (s.charAt(i) != s.charAt(m - 1 - i) ? 1 : 0);
        boolean[] result = new boolean[q];
        for (int i = 0; i < q; i++) result[i] = queries[i][0] <= (m - 1 - queries[i][3]) ? check(queries[i][0], queries[i][1], m - 1 - queries[i][3], m - 1 - queries[i][2], preS, preT, preNe) : check(m - 1 - queries[i][3], m - 1 - queries[i][2], queries[i][0], queries[i][1], preT, preS, preNe);
        return result;
    }

    private boolean check(int left1, int right1, int left2, int right2, int[][] preS, int[][] preT, int[] preNe) {
        if (preNe[left1] > 0 || preNe[preNe.length - 1] - preNe[Math.max(right1, right2) + 1] > 0) return false;
        if (right2 <= right1) return Arrays.equals(count(preS, left1, right1), count(preT, left1, right1));
        if (right1 < left2) return preNe[left2] - preNe[right1 + 1] == 0 && Arrays.equals(count(preS, left1, right1), count(preT, left1, right1)) && Arrays.equals(count(preS, left2, right2), count(preT, left2, right2));
        int[] s1 = subtract(count(preS, left1, right1), count(preT, left1, left2 - 1)), s2 = subtract(count(preT, left2, right2), count(preS, right1 + 1, right2));
        return s1 != null && s2 != null && Arrays.equals(s1, s2);
    }

    private int[] count(int[][] pre, int left, int right) {
        int[] result = pre[right + 1].clone();
        for (int i = 0; i < 26; i++) result[i] -= pre[left][i];
        return result;
    }

    private int[] subtract(int[] s1, int[] s2) {
        for (int i = 0; i < 26; i++) {
            s1[i] -= s2[i];
            if (s1[i] < 0) return null;
        }
        return s1;
    }
}
```

__Python__:

```Python
class Solution:
    def canMakePalindromeQueries(self, s: str, queries: List[List[int]]) -> List[bool]:
        s, t, pre_s, pre_t = s[:(n := len(s) >> 1)], s[n:][::-1], [[0] * 26 for _ in range(n + 1)], [[0] * 26 for _ in range(n + 1)]
        for i, (x, y) in enumerate(zip(s, t)):
            pre_s[i + 1], pre_t[i + 1] = pre_s[i][:], pre_t[i][:]
            pre_s[i + 1][ord(x) - ord('a')] += 1
            pre_t[i + 1][ord(y) - ord('a')] += 1
        pre_ne = list(accumulate((x != y for x, y in zip(s, t)), initial=0))

        def count(pre: List[List[int]], left: int, right: int) -> List[int]:
            return [x - y for x, y in zip(pre[right + 1], pre[left])]
        
        def subtract(s1: List[int], s2: List[int]) -> List[int]:
            return [] if not all(x >= y for x, y in zip(s1, s2)) else [x - y for x, y in zip(s1, s2)]
        
        def check(left1: int, right1: int, left2: int, right2: int, pre1: List[List[int]], pre2: List[List[int]]) -> bool:
            return False if pre_ne[left1] > 0 or pre_ne[n] - pre_ne[max(right1, right2) + 1] > 0 else count(pre1, left1, right1) == count(pre2, left1, right1) if right2 <= right1 else pre_ne[left2] - pre_ne[right1 + 1] == 0 and count(pre1, left1, right1) == count(pre2, left1, right1) and count(pre1, left2, right2) == count(pre2, left2, right2) if right1 < left2 else bool((s1 := subtract(count(pre1, left1, right1), count(pre2, left1, left2 - 1))) and (s2 := subtract(count(pre2, left2, right2), count(pre1, right1 + 1, right2))) and (s1 == s2))
        return [check(left1, right1, left2, (right2 := (n << 1) - 1 - c), pre_s, pre_t) if left1 <= (left2 := (n << 1) - 1 - d) else check(left2, (right2 := (n << 1) - 1 - c), left1, right1, pre_t, pre_s) for i, (left1, right1, c, d) in enumerate(queries)]
```
