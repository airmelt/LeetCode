# 488 Zuma Game 祖玛游戏

__Description__:
Think about Zuma Game. You have a row of balls on the table, colored red(R), yellow(Y), blue(B), green(G), and white(W). You also have several balls in your hand.

Each time, you may choose a ball in your hand, and insert it into the row (including the leftmost place and rightmost place). Then, if there is a group of 3 or more balls in the same color touching, remove these balls. Keep doing this until no more balls can be removed.

Find the minimal balls you have to insert to remove all the balls on the table. If you cannot remove all the balls, output -1.

__Example:__

Example 1:

Input: board = "WRRBBW", hand = "RB"
Output: -1
Explanation: WRRBBW -> WRR[R]BBW -> WBBW -> WBB[B]W -> WW

Example 2:

Input: board = "WWRRBBWW", hand = "WRBRW"
Output: 2
Explanation: WWRRBBWW -> WWRR[R]BBWW -> WWBBWW -> WWBB[B]WW -> WWWW -> empty

Example 3:

Input: board = "G", hand = "GGGGG"
Output: 2
Explanation: G -> G[G] -> GG[G] -> empty

Example 4:

Input: board = "RBYYBBRRB", hand = "YRBGB"
Output: 3
Explanation: RBYYBBRRB -> RBYY[Y]BBRRB -> RBBBRRB -> RRRB -> B -> B[B] -> BB[B] -> empty

__Constraints:__

You may assume that the initial row of balls on the table won’t have any 3 or more consecutive balls with the same color.
1 <= board.length <= 16
1 <= hand.length <= 5
Both input strings will be non-empty and only contain characters 'R','Y','B','G','W'.

__题目描述__:
回忆一下祖玛游戏。现在桌上有一串球，颜色有红色(R)，黄色(Y)，蓝色(B)，绿色(G)，还有白色(W)。 现在你手里也有几个球。

每一次，你可以从手里的球选一个，然后把这个球插入到一串球中的某个位置上（包括最左端，最右端）。接着，如果有出现三个或者三个以上颜色相同的球相连的话，就把它们移除掉。重复这一步骤直到桌上所有的球都被移除。

找到插入并可以移除掉桌上所有球所需的最少的球数。如果不能移除桌上所有的球，输出 -1 。

__示例 :__

示例 1：

输入：board = "WRRBBW", hand = "RB"
输出：-1
解释：WRRBBW -> WRR[R]BBW -> WBBW -> WBB[B]W -> WW

示例 2：

输入：board = "WWRRBBWW", hand = "WRBRW"
输出：2
解释：WWRRBBWW -> WWRR[R]BBWW -> WWBBWW -> WWBB[B]WW -> WWWW -> empty

示例 3：

输入：board = "G", hand = "GGGGG"
输出：2
解释：G -> G[G] -> GG[G] -> empty

示例 4：

输入：board = "RBYYBBRRB", hand = "YRBGB"
输出：3
解释：RBYYBBRRB -> RBYY[Y]BBRRB -> RBBBRRB -> RRRB -> B -> B[B] -> BB[B] -> empty

__提示:__

你可以假设桌上一开始的球中，不会有三个及三个以上颜色相同且连着的球。
1 <= board.length <= 16
1 <= hand.length <= 5
输入的两个字符串均为非空字符串，且只包含字符 'R','Y','B','G','W'。

__思路__:

用一个 map记录场上的球, 用一个 count数组记录手上的球的数量及种类
消除需要递归进行
每次从手上拿出一个球来尝试是否能清空球
因为手上的球最多为 5个, 设置 map的默认值为 6
如果 map[""] < 6, 说明手上的球可以使得 board清空, 否则返回 -1表示不能清空
时间复杂度O(2 ^ n), 空间复杂度O(m), n为 hand的长度, m为 board的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findMinStep(string board, string hand) 
    {
        m[board] = 0;
        m[""] = 6;
        for (auto &c : hand) ++count[c - 'A'];
        dfs(board);
        return m[""] < 6 ? m[""] : -1;
    }
private:
    unordered_map<string, int> m;
    int count[26];
    
    string eliminate(string &s) 
    {
        if (s.size() < 3) return s;
        for (int i = 0, n = s.size(); i < n; i++) 
        {
            int j = i;
            while (j < n and s[j] == s[i]) ++j;
            if (j - i > 2) 
            {
                string other = s.substr(0, i) + s.substr(j);
                return eliminate(other);
            }
        }
        return s;
    }
    
    void dfs(string &s) 
    {
        if (s.empty()) return;
        char list[]{'R', 'Y', 'B', 'G', 'W'};
        for (auto &i : list) 
        {
            if (count[i - 'A'] > 0) 
            {
                --count[i - 'A'];
                for (int j = 0; j < s.size() + 1; j++) 
                {
                    string temp = s.substr(0, j) + i + s.substr(j);
                    string other = eliminate(temp);
                    if (!m.count(s)) m[s] = 6;
                    if (!m.count(other)) m[other] = 6;
                    if (m[other] > m[s] + 1) 
                    {
                        m[other] = m[s] + 1;
                        dfs(other);
                    }
                }
                ++count[i - 'A'];
            }
        }
    }
};
```

__Java__:

```Java
class Solution {
    public int findMinStep(String board, String hand) {
        map = new HashMap<String, Integer>();
        count = new int[26];
        map.put(board, 0);
        map.put("", 6);
        for (char c : hand.toCharArray()) ++count[c - 'A'];
        dfs(board);
        return map.get("") < 6 ? map.get("") : -1;
    }
    
    private Map<String, Integer> map;
    
    private int[] count;
    
    private String eliminate(String s) {
        if (s.length() < 3) return s;
        for (int i = 0, n = s.length(); i < n; i++) {
            int j = i;
            while (j < n && s.charAt(j) == s.charAt(i)) ++j;
            if (j - i > 2) return eliminate(s.substring(0, i) + s.substring(j));
        }
        return s;
    }
    
    private void dfs(String s) {
        if (s.isEmpty()) return;
        char[] list = new char[]{'R', 'Y', 'B', 'G', 'W'};
        for (char i : list) {
            if (count[i - 'A'] > 0) {
                --count[i - 'A'];
                for (int j = 0; j < s.length() + 1; j++) {
                    String other = eliminate(s.substring(0, j) + i + s.substring(j));
                    if (map.get(s) == null) map.put(s, 6);
                    if (map.get(other) == null) map.put(other, 6);
                    if (map.get(other) > map.get(s) + 1) {
                        map.put(other, map.get(s) + 1);
                        dfs(other);
                    }
                }
                ++count[i - 'A'];
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def findMinStep(self, board: str, hand: str) -> int:
        def eliminate(s: str) -> str:
            if len(s) < 3:
                return s
            for i in range(len(s)):
                j = i
                while j < len(s) and s[j] == s[i]:
                    j += 1
                if j - i >= 3: 
                    return eliminate(s[:i] + s[j:])
            return s
        
        d, c = collections.defaultdict(lambda: 6), collections.Counter(hand)
        d[board] = 0
        
        def dfs(s: str) -> None:
            if not s: 
                return
            for i in 'RYBGW':
                if c[i] > 0:
                    c[i] -= 1
                    for j in range(len(s) + 1):
                        other = eliminate(s[:j] + i + s[j:])
                        if d[other] > d[s] + 1:
                            d[other] = d[s] + 1
                            dfs(other)
                    c[i] += 1
        
        dfs(board)
        return d[''] if d[''] < 6 else -1
```
