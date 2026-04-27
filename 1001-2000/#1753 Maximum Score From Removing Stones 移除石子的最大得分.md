# 1753 Maximum Score From Removing Stones 移除石子的最大得分

__Description:__

You are playing a solitaire game with __three piles__ of stones of sizes `a`​​​​​​, `b`,​​​​​​ and `c`​​​​​​ respectively. Each turn you choose two __different non-empty__ piles, take one stone from each, and add `1` point to your score. The game stops when there are __fewer than two non-empty__ piles (meaning there are no more available moves).

Given three integers `a`​​​​​, `b`,​​​​​ and `c`​​​​​, return _the_ ___maximum___ ___score__ you can get._

__Example:__

Example 1:

```text
Input: a = 2, b = 4, c = 6
Output: 6
Explanation: The starting state is (2, 4, 6). One optimal set of moves is:
- Take from 1st and 3rd piles, state is now (1, 4, 5)
- Take from 1st and 3rd piles, state is now (0, 4, 4)
- Take from 2nd and 3rd piles, state is now (0, 3, 3)
- Take from 2nd and 3rd piles, state is now (0, 2, 2)
- Take from 2nd and 3rd piles, state is now (0, 1, 1)
- Take from 2nd and 3rd piles, state is now (0, 0, 0)
There are fewer than two non-empty piles, so the game ends. Total: 6 points.
```

Example 2:

```text
Input: a = 4, b = 4, c = 6
Output: 7
Explanation: The starting state is (4, 4, 6). One optimal set of moves is:
- Take from 1st and 2nd piles, state is now (3, 3, 6)
- Take from 1st and 3rd piles, state is now (2, 3, 5)
- Take from 1st and 3rd piles, state is now (1, 3, 4)
- Take from 1st and 3rd piles, state is now (0, 3, 3)
- Take from 2nd and 3rd piles, state is now (0, 2, 2)
- Take from 2nd and 3rd piles, state is now (0, 1, 1)
- Take from 2nd and 3rd piles, state is now (0, 0, 0)
There are fewer than two non-empty piles, so the game ends. Total: 7 points.
```

Example 3:

```text
Input: a = 1, b = 8, c = 8
Output: 8
Explanation: One optimal set of moves is to take from the 2nd and 3rd piles for 8 turns until they are empty.
After that, there are fewer than two non-empty piles, so the game ends.
```

__Constraints:__

- `1 <= a, b, c <= 10 ^ 5`

__题目描述:__

你正在玩一个单人游戏，面前放置着大小分别为 `a`​​​​​​、 `b` 和 `c`​​​​​​ 的 __三堆__ 石子。

每回合你都要从两个 __不同的非空堆__ 中取出一颗石子，并在得分上加 `1` 分。当存在 __两个或更多__ 的空堆时，游戏停止。

给你三个整数 `a` 、 `b` 和 `c` ，返回可以得到的 __最大分数__ 。

__示例:__

示例 1：

```text
输入：a = 2, b = 4, c = 6
输出：6
解释：石子起始状态是 (2, 4, 6) ，最优的一组操作是：
- 从第一和第三堆取，石子状态现在是 (1, 4, 5)
- 从第一和第三堆取，石子状态现在是 (0, 4, 4)
- 从第二和第三堆取，石子状态现在是 (0, 3, 3)
- 从第二和第三堆取，石子状态现在是 (0, 2, 2)
- 从第二和第三堆取，石子状态现在是 (0, 1, 1)
- 从第二和第三堆取，石子状态现在是 (0, 0, 0)
总分：6 分 。
```

示例 2：

```text
输入：a = 4, b = 4, c = 6
输出：7
解释：石子起始状态是 (4, 4, 6) ，最优的一组操作是：
- 从第一和第二堆取，石子状态现在是 (3, 3, 6)
- 从第一和第三堆取，石子状态现在是 (2, 3, 5)
- 从第一和第三堆取，石子状态现在是 (1, 3, 4)
- 从第一和第三堆取，石子状态现在是 (0, 3, 3)
- 从第二和第三堆取，石子状态现在是 (0, 2, 2)
- 从第二和第三堆取，石子状态现在是 (0, 1, 1)
- 从第二和第三堆取，石子状态现在是 (0, 0, 0)
总分：7 分 。
```

示例 3：

```text
输入：a = 1, b = 8, c = 8
输出：8
解释：最优的一组操作是连续从第二和第三堆取 8 回合，直到将它们取空。
注意，由于第二和第三堆已经空了，游戏结束，不能继续从第一堆中取石子。
```

__提示：__

- `1 <= a, b, c <= 10 ^ 5`

__思路:__

```text
贪心
不妨设 a <= b <= c
如果 a + b <= c, 则每次从 c 中取一个石子, 直到 a + b 中的石子取完
如果 a + b > c, 每次匹配 c 的时候, 匹配 a, b 较大的, 直到 c 中的石子取完, 这时 a 和 b 相等或者相差 1, 最后匹配 a 和 b, 总共可以取 a + b + c / 2 次
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumScore(int a, int b, int c) 
    {
        return a + b < c ? a + b : (b + c < a ? b + c : (a + c < b ? a + c : ((a + b + c) >> 1)));
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumScore(int a, int b, int c) {
        return a + b < c ? a + b : (b + c < a ? b + c : (a + c < b ? a + c : ((a + b + c) >> 1)));
    }
}
```

__Python__:

```Python
class Solution:
    def maximumScore(self, a: int, b: int, c: int) -> int:
        return a + b if a + b < c else b + c if b + c < a else a + c if a + c < b else sum([a, b, c]) >> 1
```
