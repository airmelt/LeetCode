# 943 Find the Shortest Superstring 最短超级串

__Description__:
Given an array of strings words, return the smallest string that contains each string in words as a substring. If there are multiple valid strings of the smallest length, return any of them.

You may assume that no string in words is a substring of another string in words.

__Example:__

Example 1:

Input: words = ["alex","loves","leetcode"]
Output: "alexlovesleetcode"
Explanation: All permutations of "alex","loves","leetcode" would also be accepted.

Example 2:

Input: words = ["catg","ctaagt","gcta","ttca","atgcatc"]
Output: "gctaagttcatgcatc"

__Constraints:__

1 <= words.length <= 12
1 <= words[i].length <= 20
words[i] consists of lowercase English letters.
All the strings of words are unique.

__题目描述__:
给定一个字符串数组 words，找到以 words 中每个字符串作为子字符串的最短字符串。如果有多个有效最短字符串满足题目条件，返回其中 任意一个 即可。

我们可以假设 words 中没有字符串是 words 中另一个字符串的子字符串。

__示例 :__

示例 1：

输入：words = ["alex","loves","leetcode"]
输出："alexlovesleetcode"
解释："alex"，"loves"，"leetcode" 的所有排列都会被接受。

示例 2：

输入：words = ["catg","ctaagt","gcta","ttca","atgcatc"]
输出："gctaagttcatgcatc"

__提示:__

1 <= words.length <= 12
1 <= words[i].length <= 20
words[i] 由小写英文字母组成
words 中的所有字符串 互不相同

__思路__:

动态规划状态压缩
用 state 表示使用的字符串, 位对应 1 表示使用了 words[i]
设 dp[state][i] 表示第 i 个单词放在首位的单词长度
比如 dp[0x1111][0] 表示 words[0] 放在首位, 使用了 words[:4] 的所有单词
那么 dp[0x1111][0] = len(words[0] + min(dp[0x1110][1] - overlap[0][1], dp[0x1110][2] - overlap[0][2], dp[0x1110][3] - overlap[0][3]
就是将 words[0] 放在首位, 那么找到 words[1], words[2], words[3] 依次放在首位的最短单词长度即可
overlap 为预先计算的两个单词的前后缀的最大长度
另外因为要输出单词所以还要用 pre 数组记录下转移的位置
时间复杂度为 O(n ^ 2 \* 2 ^ n), 空间复杂度为 O(n \* 2 ^ n), n 为 words 的长度

__代码__:
__C++__:

```C++
class Solution {
public:
    string shortestSuperstring(vector<string>& words) 
    {
        const int INF = 1e9, n = words.size();
        int min_len = INF, start = -1, mask = (1 << n) - 1, cur = 0;
        vector<vector<int>> overlaps(n, vector<int>(n)), dp(1 << n, vector<int>(n, INF)), pre(1 << n, vector<int>(n));
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) if (i != j) for (int k = min(words[i].size(), words[j].size()); k > -1; k--) if (words[i].substr((int)words[i].size() - k) == words[j].substr(0, k)) 
        {
            overlaps[i][j] = k;
            break;
        }
        for (int state = 1; state <= mask; state++) for (int i = 0; i < n; i++)
        {
            if ((state >> i) & 1) 
            {
                if (!(state & (state - 1))) 
                {
                    dp[state][i] = words[i].size();
                    pre[state][i] = i;
                } 
                else 
                {
                    for (int j = 0; j < n; j++) if (i != j) if ((state >> j) & 1) 
                    {
                        cur = dp[state ^ (1 << i)][j] + words[i].size() - overlaps[j][i];
                        if (cur < dp[state][i]) 
                        {
                            dp[state][i] = cur;
                            pre[state][i] = j;
                        }
                    }    
                }
            }
        }
        for (int i = 0; i < n; i++) if (dp[mask][i] < min_len) 
        {
            min_len = dp[mask][i];
            start = i;
        }
        vector<int> path{ start };
        int state = mask, pre_id = start;
        for (int i = 0; i < n - 1; i++) 
        {
            int cur_id = pre[state][pre_id];
            path.emplace_back(cur_id);
            state ^= (1 << pre_id);
            pre_id = cur_id;
        }
        reverse(path.begin(), path.end());
        string result = words[path.front()];
        for (int k = 1; k < n; k++) result += words[path[k]].substr(overlaps[path[k - 1]][path[k]]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String shortestSuperstring(String[] words) {
        final int INF = 1_000_000_000;
        int n = words.length, minLen = INF, start = -1, mask = (1 << n) - 1, overlaps[][] = new int[n][n], dp[][] = new int[1 << n][n], pre[][] = new int[1 << n][n], cur = 0;
        for (int i = 0; i < (1 << n); i++) Arrays.fill(dp[i], INF);
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) if (i != j) for (int k = Math.min(words[i].length(), words[j].length()); k > -1; k--) if (words[i].endsWith(words[j].substring(0, k))) {
            overlaps[i][j] = k;
            break;
        }
        for (int state = 1; state <= mask; state++) for (int i = 0; i < n; i++) {
            if (((state >> i) & 1) != 0) {
                if ((state & (state - 1)) == 0) {
                    dp[state][i] = words[i].length();
                    pre[state][i] = i;
                } else {
                    for (int j = 0; j < n; j++) if (i != j) if (((state >> j) & 1) != 0) {
                        cur = dp[state ^ (1 << i)][j] + words[i].length() - overlaps[j][i];
                        if (cur < dp[state][i]) {
                            dp[state][i] = cur;
                            pre[state][i] = j;
                        }
                    }    
                }
            }
        }
        for (int i = 0; i < n; i++) if (dp[mask][i] < minLen) {
            minLen = dp[mask][i];
            start = i;
        }
        List<Integer> path = new ArrayList<>();
        path.add(start);
        int state = mask, preId = start;
        for (int i = 0; i < n - 1; i++) {
            int curId = pre[state][preId];
            path.add(curId);
            state ^= (1 << preId);
            preId = curId;
        }
        Collections.reverse(path);
        StringBuilder result = new StringBuilder(words[path.get(0)]);
        for (int k = 1; k < n; k++) result.append(words[path.get(k)].substring(overlaps[path.get(k - 1)][path.get(k)]));
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def shortestSuperstring(self, words: List[str]) -> str:
        INF, min_len, start, mask = 10 ** 9, 10 ** 9, -1, (1 << (n := len(words))) - 1
        overlap, dp, pre = [[0] * n for _ in range(n)], [[INF] * n for _ in range((1 << n))], [[-1] * n for _ in range((1 << n))] 
        for i in range(n):
            for j in range(n):
                if i != j:
                    for k in range(min(len(words[i]), len(words[j])), 0, -1):
                        if words[i][-k:] == words[j][:k]:
                            overlap[i][j] = k 
                            break
        for state in range(1, 1 << n):
            for i in range(n):
                if (state >> i) & 1:
                    if bin(state).count('1') == 1:
                        dp[state][i], pre[state][i] = len(words[i]), i
                    else:
                        for j in range(n):
                            if i != j:
                                if (state >> j) & 1:
                                    cur = dp[state ^ (1 << i)][j] + len(words[i]) - overlap[j][i]
                                    if cur < dp[state][i]:
                                        dp[state][i] = cur
                                        pre[state][i] = j
        for i in range(n):
            if dp[mask][i] < min_len:
                min_len = dp[mask][i]
                start = i
        path, state, pre_id = [start], mask, start
        for _ in range(n - 1):
            path.append((cur_id := pre[state][pre_id]))
            state ^= (1 << pre_id)
            pre_id = cur_id
        path.reverse()
        result = words[path[0]]
        for k in range(1, n):
            result += words[(i := path[k])][overlap[path[k - 1]][i]:]
        return result
```
