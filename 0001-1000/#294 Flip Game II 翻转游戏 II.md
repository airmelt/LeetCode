# 294 Flip Game II 翻转游戏 II

__Description:__

You are playing a Flip Game with your friend.

You are given a string `currentState` that contains only `'+'` and `'-'`. You and your friend take turns to flip two consecutive `"++"` into `"--"`. The game ends when a person can no longer make a move, and therefore the other person will be the winner.

Return `true` _if the starting player can __guarantee a win__,_ and `false` otherwise.

__Example:__

Example 1:

```text
Input: currentState = "++++"
Output: true
Explanation: The starting player can guarantee a win by flipping the middle "++" to become "+--+".
```

Example 2:

```text
Input: currentState = "+"
Output: false
```

__Constraints:__

- `1 <= currentState.length <= 60`
- `currentState[i]` is either `'+'` or `'-'`.

__Follow up:__

Derive your algorithm's runtime complexity.

__题目描述:__

你和朋友玩一个叫做「翻转游戏」的游戏。游戏规则如下：

给你一个字符串 currentState ，其中只含 `'+'` 和 `'-'` 。你和朋友轮流将 连续 的两个 `"++"` 反转成 `"--"` 。当一方无法进行有效的翻转时便意味着游戏结束，则另一方获胜。默认每个人都会采取最优策略。

请你写出一个函数来判定起始玩家 __是否存在必胜的方案__ ：如果存在，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：currentState = "++++"
输出：true
解释：起始玩家可将中间的 "++" 翻转变为 "+--+" 从而得胜。
```

示例 2：

```text
输入：currentState = "+"
输出：false
```

__提示：__

- `1 <= currentState.length <= 60`
- `currentState[i]` 不是 `'+'` 就是 `'-'`

__思路:__

```text
动态规划
如果当前状态的下一个状态中存在必败状态, 则当前状态为必胜状态
即如果下一个状态中没有 "++" 则当前状态为必胜状态
所以如果没有出现 "++" 则返回 false
在当前状态检查将 "++" 反转为 "--" 的所有可能状态, 如果有一个状态为必败状态, 则当前状态为必胜状态
由于会有很多重复的状态, 所以使用哈希表进行记忆化搜索
时间复杂度为 O(2 ^ N * N), 空间复杂度为 O(2 ^ N), 其中 N 为字符串长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canWin(string currentState) 
    {
        long long state = 0LL;
        n = currentState.size();
        for (int i = 0; i < n; i++) state |= (long long)(currentState[i] == '+') << i;
        return dfs(state);    
    }
private:
    unordered_map<long long, bool> memo;
    int n;

    bool dfs(long long state)
    {
        if (!memo.count(state)) for (int i = 1, flag = 0; i < n and !flag; i++) if ((state & (1LL << (i - 1))) and (state & (1LL << i))) if (!dfs(state ^ (1LL << (i - 1)) + (1LL << i))) return true;
        return memo[state] = false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canWin(String currentState) {
        return canWin(currentState.toCharArray());
    }

    public boolean canWin(char[] arr) {
        boolean flag = false;
        for (int i = 1, n = arr.length; i < n && !flag; i++) {
            if (arr[i - 1] == '+' && arr[i] == '+') {
                arr[i] = arr[i - 1] = '-';
                if (!canWin(arr)) flag = true;
                arr[i] = arr[i - 1] = '+';
            }
        }
        return flag;
    }
}
```

__Python__:

```Python
class Solution:
    @lru_cache(None)
    def canWin(self, currentState: str) -> bool:
        return any(not self.canWin(currentState[:i] + '--' + currentState[i + 2:]) for i in range(len(currentState) - 1) if currentState[i:i + 2] == '++')
```
