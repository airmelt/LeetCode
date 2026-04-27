# 1583 Count Unhappy Friends 统计不开心的朋友

__Description:__

You are given a list of `preferences` for `n` friends, where `n` is always __even__.

For each person `i`, `preferences[i]` contains a list of friends __sorted__ in the __order of preference__. In other words, a friend earlier in the list is more preferred than a friend later in the list. Friends in each list are denoted by integers from `0` to `n-1`.

All the friends are divided into pairs. The pairings are given in a list `pairs`, where `pairs[i] = [xi, yi]` denotes `xi` is paired with `yi` and `yi` is paired with `xi`.

However, this pairing may cause some of the friends to be unhappy. A friend `x` is unhappy if `x` is paired with `y` and there exists a friend `u` who is paired with `v` but:

- `x` prefers `u` over `y`, and
- `u` prefers `x` over `v`.

Return the number of unhappy friends.

__Example:__

Example 1:

```text
Input: n = 4, preferences = [[1, 2, 3], [3, 2, 0], [3, 1, 0], [1, 2, 0]], pairs = [[0, 1], [2, 3]]
Output: 2
Explanation:
Friend 1 is unhappy because:
- 1 is paired with 0 but prefers 3 over 0, and
- 3 prefers 1 over 2.
Friend 3 is unhappy because:
- 3 is paired with 2 but prefers 1 over 2, and
- 1 prefers 3 over 0.
Friends 0 and 2 are happy.
```

Example 2:

```text
Input: n = 2, preferences = [[1], [0]], pairs = [[1, 0]]
Output: 0
Explanation: Both friends 0 and 1 are happy.
```

Example 3:

```text
Input: n = 4, preferences = [[1, 3, 2], [2, 3, 0], [1, 3, 0], [0, 2, 1]], pairs = [[1, 3], [0, 2]]
Output: 4
```

__Constraints:__

- `2 <= n <= 500`
- `n` is even.
- `preferences.length == n`
- `preferences[i].length == n - 1`
- `0 <= preferences[i][j] <= n - 1`
- `preferences[i]` does not contain `i`.
- All values in `preferences[i]` are unique.
- `pairs.length == n/2`
- `pairs[i].length == 2`
- `xi != yi`
- `0 <= xi, yi <= n - 1`
- Each person is contained in __exactly one__ pair.

__题目描述:__

给你一份 `n` 位朋友的亲近程度列表，其中 `n` 总是 __偶数__ 。

对每位朋友 `i`， `preferences[i]` 包含一份 __按亲近程度从高____到低排列__ 的朋友列表。换句话说，排在列表前面的朋友与 `i` 的亲近程度比排在列表后面的朋友更高。每个列表中的朋友均以 `0` 到 `n-1` 之间的整数表示。

所有的朋友被分成几对，配对情况以列表 `pairs` 给出，其中 `pairs[i] = [xi, yi]` 表示 `xi` 与 `yi` 配对，且 `yi` 与 `xi` 配对。

但是，这样的配对情况可能会使其中部分朋友感到不开心。在 `x` 与 `y` 配对且 `u` 与 `v` 配对的情况下，如果同时满足下述两个条件， `x` 就会不开心:

- `x` 与 `u` 的亲近程度胜过 `x` 与 `y`，且
- `u` 与 `x` 的亲近程度胜过 `u` 与 `v`

返回 不开心的朋友的数目 。

__示例:__

示例 1：

```text
输入：n = 4, preferences = [[1, 2, 3], [3, 2, 0], [3, 1, 0], [1, 2, 0]], pairs = [[0, 1], [2, 3]]
输出：2
解释：
朋友 1 不开心，因为：
- 1 与 0 配对，但 1 与 3 的亲近程度比 1 与 0 高，且
- 3 与 1 的亲近程度比 3 与 2 高。
朋友 3 不开心，因为：
- 3 与 2 配对，但 3 与 1 的亲近程度比 3 与 2 高，且
- 1 与 3 的亲近程度比 1 与 0 高。
朋友 0 和 2 都是开心的。
```

示例 2：

```text
输入：n = 2, preferences = [[1], [0]], pairs = [[1, 0]]
输出：0
解释：朋友 0 和 1 都开心。
```

示例 3：

```text
输入：n = 4, preferences = [[1, 3, 2], [2, 3, 0], [1, 3, 0], [0, 2, 1]], pairs = [[1, 3], [0, 2]]
输出：4
```

__提示：__

- `2 <= n <= 500`
- `n` 是偶数
- `preferences.length == n`
- `preferences[i].length == n - 1`
- `0 <= preferences[i][j] <= n - 1`
- `preferences[i]` 不包含 `i`
- `preferences[i]` 中的所有值都是独一无二的
- `pairs.length == n/2`
- `pairs[i].length == 2`
- `xi != yi`
- `0 <= xi, yi <= n - 1`
- 每位朋友都 __恰好__ 被包含在一对中

__思路:__

```text
模拟
由于 pairs 恰好包含所有人各一次
可以将 pairs 转化为一维数组 match, 分别记录每个人匹配的对象
然后搜索匹配的对象, 如果搜索到的匹配对象有别人更优就直接返回
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    bool helper(int x, vector<int>& match, vector<vector<int>>& preferences) 
    {
        int y = match[x];
        for (int u : preferences[x]) 
        {
            if (u == y) break;
            for (int v : preferences[u]) 
            {
                if (v == match[u]) break;
                if (v == x) return true;
            }
        }
        return false;
    }
public:
    int unhappyFriends(int n, vector<vector<int>>& preferences, vector<vector<int>>& pairs) 
    {
        int result = 0;
        vector<int> match(n);
        for (const auto& pair : pairs) 
        {
            match[pair.front()] = pair.back();
            match[pair.back()] = pair.front();
        }
        for (int i = 0; i < n; i++) result += helper(i, match, preferences);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int unhappyFriends(int n, int[][] preferences, int[][] pairs) {
        int result = 0, match[] = new int[n];
        for (int[] pair : pairs) {
            match[pair[0]] = pair[1];
            match[pair[1]] = pair[0];
        }
        for (int i = 0; i < n; i++) if (helper(i, match, preferences)) ++result;
        return result;
    }
    
    private boolean helper(int x, int[] match, int[][] preferences) {
        int y = match[x];
        for (int u : preferences[x]) {
            if (u == y) break;
            for (int v : preferences[u]) {
                if (v == match[u]) break;
                if (v == x) return true;
            }
        }
        return false;
    }
}
```

__Python__:

```Python
import numpy as np
class Solution:
    def unhappyFriends(self, n: int, preferences: List[List[int]], pairs: List[List[int]]) -> int:
        prefer = np.zeros((n, n), dtype=np.bool_)
        for x, y in map(tuple,pairs):
            prefer[x][preferences[x][:preferences[x].index(y)]] = prefer[y][preferences[y][:preferences[y].index(x)]] = True
        return sum(True in l for l in np.logical_and(prefer, prefer.transpose()))
```
