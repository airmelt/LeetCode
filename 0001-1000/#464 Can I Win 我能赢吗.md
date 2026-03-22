# 464 Can I Win 我能赢吗

__Description__:
In the "100 game" two players take turns adding, to a running total, any integer from 1 to 10. The player who first causes the running total to reach or exceed 100 wins.

What if we change the game so that players cannot re-use integers?

For example, two players might take turns drawing from a common pool of numbers from 1 to 15 without replacement until they reach a total >= 100.

Given two integers maxChoosableInteger and desiredTotal, return true if the first player to move can force a win, otherwise return false. Assume both players play optimally.

__Example:__

Example 1:

Input: maxChoosableInteger = 10, desiredTotal = 11
Output: false
Explanation:
No matter which integer the first player choose, the first player will lose.
The first player can choose an integer from 1 up to 10.
If the first player choose 1, the second player can only choose integers from 2 up to 10.
The second player will win by choosing 10 and get a total = 11, which is >= desiredTotal.
Same with other integers chosen by the first player, the second player will always win.

Example 2:

Input: maxChoosableInteger = 10, desiredTotal = 0
Output: true

Example 3:

Input: maxChoosableInteger = 10, desiredTotal = 1
Output: true

__Constraints:__

1 <= maxChoosableInteger <= 20
0 <= desiredTotal <= 300

__题目描述__:
在 "100 game" 这个游戏中，两名玩家轮流选择从 1 到 10 的任意整数，累计整数和，先使得累计整数和达到或超过 100 的玩家，即为胜者。

如果我们将游戏规则改为 “玩家不能重复使用整数” 呢？

例如，两个玩家可以轮流从公共整数池中抽取从 1 到 15 的整数（不放回），直到累计整数和 >= 100。

给定一个整数 maxChoosableInteger （整数池中可选择的最大数）和另一个整数 desiredTotal（累计和），判断先出手的玩家是否能稳赢（假设两位玩家游戏时都表现最佳）？

你可以假设 maxChoosableInteger 不会大于 20， desiredTotal 不会大于 300。

__示例 :__

输入：
maxChoosableInteger = 10
desiredTotal = 11

输出：
false

解释：
无论第一个玩家选择哪个整数，他都会失败。
第一个玩家可以选择从 1 到 10 的整数。
如果第一个玩家选择 1，那么第二个玩家只能选择从 2 到 10 的整数。
第二个玩家可以通过选择整数 10（那么累积和为 11 >= desiredTotal），从而取得胜利.
同样地，第一个玩家选择任意其他整数，第二个玩家都会赢。

__思路__:

回溯➕记忆化搜索剪枝

1. 如果第一个人选择的范围包括目标直接获胜
2. 如果选择的范围和都达不到目标肯定失败
3. 注意到选择的数字最大为 20, 那么可以用一个 int记录选择的数字, 比如选择了1, 2, 4, 可以记为 1011, 即 11, 用 map记录之前搜索过的直接返回, 回溯搜索, 从 0开始搜索到 maxChoosableInteger结束, 每次选择 i + 1, 搜索到desiredTotal <= i + 1, 表示这次已经能到达目标值, 或者对方输就代表自己赢
时间复杂度O(2 ^ n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool canIWin(int maxChoosableInteger, int desiredTotal) 
    {
        if (maxChoosableInteger >= desiredTotal) return true;
        if ((maxChoosableInteger * (maxChoosableInteger + 1) >> 1) < desiredTotal) return false;
        unordered_map<int, bool> record;
        return helper(maxChoosableInteger, desiredTotal, 0, record);
    }
private:
    bool helper(int maxChoosableInteger, int desiredTotal, int used, unordered_map<int, bool> &record)
    {
        if (record.count(used)) return record[used];
        for (int i = 0; i < maxChoosableInteger; i++) if ((!((1 << i) & used)) and (desiredTotal <= i + 1 or !helper(maxChoosableInteger, desiredTotal - i - 1, used | (1 << i), record))) return record[used] = true;
        return record[used] = false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canIWin(int maxChoosableInteger, int desiredTotal) {
        if (maxChoosableInteger >= desiredTotal) return true;
        if ((maxChoosableInteger * (maxChoosableInteger + 1)) >> 1 < desiredTotal) return false;
        Map<Integer, Boolean> record = new HashMap<>();
        return helper(maxChoosableInteger, desiredTotal, 0, record);
    }
    
    private boolean helper(int maxChoosableInteger, int desiredTotal, int used, Map<Integer, Boolean> record) {
        if (record.containsKey(used)) return record.get(used);
        for (int i = 0; i < maxChoosableInteger; i++) {
            if ((((1 << i) & used) == 0) && (desiredTotal < i + 2 || !helper(maxChoosableInteger, desiredTotal - i - 1, used | (1 << i), record))) {
                record.put(used, true);
                return true;
            }
        }
        record.put(used, false);
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def canIWin(self, maxChoosableInteger: int, desiredTotal: int) -> bool:
        if maxChoosableInteger >= desiredTotal:
            return True
        if (maxChoosableInteger * (maxChoosableInteger + 1)) >> 1 < desiredTotal:
            return False
        record = dict()
        def helper(maxChoosableInteger: int, desiredTotal: int, used: int) -> bool:
            if used in record:
                return record[used]
            for i in range(maxChoosableInteger):
                if not ((cur := (1 << i)) & used) and (desiredTotal < i + 2 or not helper(maxChoosableInteger, desiredTotal - i - 1, used | cur)):
                    record[used] = True
                    return True
            record[used] = False
            return False
        return helper(maxChoosableInteger, desiredTotal, 0)
```
