# 936 Stamping The Sequence 戳印序列

__Description__:
You are given two strings stamp and target. Initially, there is a string s of length target.length with all s[i] == '?'.

In one turn, you can place stamp over s and replace every letter in the s with the corresponding letter from stamp.

For example, if stamp = "abc" and target = "abcba", then s is "?????" initially. In one turn you can:
place stamp at index 0 of s to obtain "abc??",
place stamp at index 1 of s to obtain "?abc?", or
place stamp at index 2 of s to obtain "??abc".
Note that stamp must be fully contained in the boundaries of s in order to stamp (i.e., you cannot place stamp at index 3 of s).
We want to convert s to target using at most 10 * target.length turns.

Return an array of the index of the left-most letter being stamped at each turn. If we cannot obtain target from s within 10 * target.length turns, return an empty array.

__Example:__

Example 1:

Input: stamp = "abc", target = "ababc"
Output: [0,2]
Explanation: Initially s = "?????".

- Place stamp at index 0 to get "abc??".
- Place stamp at index 2 to get "ababc".
[1,0,2] would also be accepted as an answer, as well as some other answers.

Example 2:

Input: stamp = "abca", target = "aabcaca"
Output: [3,0,1]
Explanation: Initially s = "???????".

- Place stamp at index 3 to get "???abca".
- Place stamp at index 0 to get "abcabca".
- Place stamp at index 1 to get "aabcaca".

__Constraints:__

1 <= stamp.length <= target.length <= 1000
stamp and target consist of lowercase English letters.

__题目描述__:
你想要用小写字母组成一个目标字符串 target。

开始的时候，序列由 target.length 个 '?' 记号组成。而你有一个小写字母印章 stamp。

在每个回合，你可以将印章放在序列上，并将序列中的每个字母替换为印章上的相应字母。你最多可以进行 10 * target.length  个回合。

举个例子，如果初始序列为 "?????"，而你的印章 stamp 是 "abc"，那么在第一回合，你可以得到 "abc??"、"?abc?"、"??abc"。（请注意，印章必须完全包含在序列的边界内才能盖下去。）

如果可以印出序列，那么返回一个数组，该数组由每个回合中被印下的最左边字母的索引组成。如果不能印出序列，就返回一个空数组。

例如，如果序列是 "ababc"，印章是 "abc"，那么我们就可以返回与操作 "?????" -> "abc??" -> "ababc" 相对应的答案 [0, 2]；

另外，如果可以印出序列，那么需要保证可以在 10 * target.length 个回合内完成。任何超过此数字的答案将不被接受。

__示例 :__

示例 1：

输入：stamp = "abc", target = "ababc"
输出：[0,2]
（[1,0,2] 以及其他一些可能的结果也将作为答案被接受）

示例 2：

输入：stamp = "abca", target = "aabcaca"
输出：[3,0,1]

__提示:__

1 <= stamp.length <= target.length <= 1000
stamp 和 target 只包含小写字母。

__思路__:

逆向模拟
因为题意正向模拟会覆盖之前的字符, 难以记录, 可以将 target 与 stamp 对应位置相同的字符用 "?" 代替, 最后如果全是 "?" 说明可以替换成功
每次从一个位置开始以 stamp 的大小作为一个窗口遍历 target, 并将 target 对应位置上的字符修改为 "?", 直到没有修改 target 退出循环
如果能够修改成功, 将当前位置加入结果列表
最后如果 target 全部字符被修改为 "?" 说明可以修改, 返回结果的逆序, 因为加入时是逆序
否则返回空数组
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2), n 为 target 长度, m 为 stamp 长度, n >= m, 时间复杂度为 O(n(n - m))

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> movesToStamp(string stamp, string target) 
    {
        int m = stamp.size(), n = target.size(), count = 0;
        vector<int> result, empty_list;
        bool change = true;
        while (change)
        {
            change = false;
            for (int i = 0; i < n - m + 1; i++) change |= check(i, m, stamp, target, result);
        }
        for (const auto& c : target) if (c == '?') ++count;
        reverse(result.begin(), result.end());
        return count == n ? result : empty_list;
    }
private:
    bool check(int i, int m, string& s, string& t, vector<int>& result) 
    {
        bool change = false;
        for (int j = 0; j < m; j++) 
        {
            if (t[i + j] == '?') continue;
            if (t[i + j] != s[j]) return false;
            change = true;
        }
        if (change) 
        {
            for (int j = 0; j < m; j++) t[i + j] = '?';
            result.emplace_back(i);
        }
        return change;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] movesToStamp(String stamp, String target) {
        int m = stamp.length(), n = target.length(), count = 0;
        char[] s = stamp.toCharArray(), t = target.toCharArray();
        List<Integer> list = new ArrayList<>();
        boolean change = true;
        while (change) {
            change = false;
            for (int i = 0; i < n - m + 1; i++) change |= check(i, m, s, t, list);
        }
        for (char c : t) if (c == '?') ++count;
        if (count < n) return new int[0];
        int p = list.size(), result[] = new int[p];
        for (int i = p - 1; i > -1; i--) result[p - i - 1] = list.get(i);
        return result;
    }
    
    private boolean check(int i, int m, char[] s, char[] t, List<Integer> list) {
        boolean result = false;
        for (int j = 0; j < m; j++) {
            if (t[i + j] == '?') continue;
            if (t[i + j] != s[j]) return false;
            result = true;
        }
        if (result) {
            for (int j = 0; j < m; j++) t[i + j] = '?';
            list.add(i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def movesToStamp(self, stamp: str, target: str) -> List[int]:
        m, n, s, t, result, change = len(stamp), len(target), list(stamp), list(target), [], True

        def check(i: int) -> bool:
            change = False
            for j in range(m):
                if t[i + j] == "?": 
                    continue
                if t[i + j] != s[j]: 
                    return False
                change = True
            if change:
                t[i:i + m] = ["?"] * m
                result.append(i)
            return change

        while change:
            change = False
            for i in range(n - m + 1):
                change |= check(i)
        return result[::-1] if t.count("?") == n else []
```
