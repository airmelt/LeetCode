# 1040 Moving Stones Until Consecutive II 移动石子直到连续 II

__Description__:
There are some stones in different positions on the X-axis. You are given an integer array stones, the positions of the stones.

Call a stone an endpoint stone if it has the smallest or largest position. In one move, you pick up an endpoint stone and move it to an unoccupied position so that it is no longer an endpoint stone.

In particular, if the stones are at say, stones = [1,2,5], you cannot move the endpoint stone at position 5, since moving it to any position (such as 0, or 3) will still keep that stone as an endpoint stone.
The game ends when you cannot make any more moves (i.e., the stones are in three consecutive positions).

Return an integer array answer of length 2 where:

answer[0] is the minimum number of moves you can play, and
answer[1] is the maximum number of moves you can play.

__Example:__

Example 1:

Input: stones = [7,4,9]
Output: [1,2]
Explanation: We can move 4 -> 8 for one move to finish the game.
Or, we can move 9 -> 5, 4 -> 6 for two moves to finish the game.

Example 2:

Input: stones = [6,5,4,3,10]
Output: [2,3]
Explanation: We can move 3 -> 8 then 10 -> 7 to finish the game.
Or, we can move 3 -> 7, 4 -> 8, 5 -> 9 to finish the game.
Notice we cannot move 10 -> 2 to finish the game, because that would be an illegal move.

__Constraints:__

3 <= stones.length <= 10^4
1 <= stones[i] <= 10^9
All the values of stones are unique.

__题目描述__:
在一个长度 无限 的数轴上，第 i 颗石子的位置为 stones[i]。如果一颗石子的位置最小/最大，那么该石子被称作 端点石子 。

每个回合，你可以将一颗端点石子拿起并移动到一个未占用的位置，使得该石子不再是一颗端点石子。

值得注意的是，如果石子像 stones = [1,2,5] 这样，你将 无法 移动位于位置 5 的端点石子，因为无论将它移动到任何位置（例如 0 或 3），该石子都仍然会是端点石子。

当你无法进行任何移动时，即，这些石子的位置连续时，游戏结束。

要使游戏结束，你可以执行的最小和最大移动次数分别是多少？ 以长度为 2 的数组形式返回答案：answer = [minimum_moves, maximum_moves] 。

__示例 :__

示例 1：

输入：[7,4,9]
输出：[1,2]
解释：
我们可以移动一次，4 -> 8，游戏结束。
或者，我们可以移动两次 9 -> 5，4 -> 6，游戏结束。

示例 2：

输入：[6,5,4,3,10]
输出：[2,3]
解释：
我们可以移动 3 -> 8，接着是 10 -> 7，游戏结束。
或者，我们可以移动 3 -> 7, 4 -> 8, 5 -> 9，游戏结束。
注意，我们无法进行 10 -> 2 这样的移动来结束游戏，因为这是不合要求的移动。

示例 3：

输入：[100,101,104,102,103]
输出：[0,0]

__提示:__

3 <= stones.length <= 10^4
1 <= stones[i] <= 10^9
stones[i] 的值各不相同。

__思路__:

模拟
先排序
因为不能将端点的石头移动到端点, 最多移动次数取第一个石头到倒数第二个石头和第二个石头到最后一个石头中间的位置
比如 [6, 5, 4, 2, 10], 排序之后是 [2, 4, 5, 6, 10]
要么将 2 移动到 3, 要么将 10 移动到 5(不考虑占用), 能够获得最大的移动次数
maximum_moves = max(stones[n - 2] - stones[0], stones[n - 1] - stones[1]) + 2 - n
最小移动次数是找到一个窗口使得窗口中的石子越多越好, 需要注意 [3, 4, 5, 6, 10] 这种情况, 有 n - 1 个石头连续, 需要移动两步, 不能将 10 直接移动到 7, 而是需要先将 3 移动到 8 再将 10  移动到 7, 需要 2 步
时间复杂度O(nlgn), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> numMovesStonesII(vector<int>& stones) 
    {
        sort(stones.begin(), stones.end());
        int n = stones.size(), maximum_moves = max(stones[n - 2] - stones.front(), stones.back() - stones[1]) + 2 - n, minimum_moves = 1, end = 0;
        for (int i = 0; i < n; i++) 
        {
            while (end < n and stones[end] - stones[i] < n) ++end;
            minimum_moves = max(minimum_moves, end - i);
        }
        if (minimum_moves == n - 1 and ((stones[n - 2] - stones.front() == n - 2 and stones.back() - stones[n - 2] > 2) or (stones.back() - stones[1] == n - 2 and stones[1] - stones.front() > 2))) --minimum_moves;
        return vector<int>{n - minimum_moves, maximum_moves};
    }
};
```

__Java__:

```Java
class Solution {
    public int[] numMovesStonesII(int[] stones) {
        Arrays.sort(stones);
        int n = stones.length, maximumMoves = Math.max(stones[n - 2] - stones[0], stones[n - 1] - stones[1]) + 2 - n, minimumMoves = 1, end = 0;
        for (int i = 0; i < n; i++) {
            while (end < n && stones[end] - stones[i] < n) ++end;
            minimumMoves = Math.max(minimumMoves, end - i);
        }
        if (minimumMoves == n - 1 && ((stones[n - 2] - stones[0] == n - 2 && stones[n - 1] - stones[n - 2] > 2) || (stones[n - 1] - stones[1] == n - 2 && stones[1] - stones[0] > 2))) --minimumMoves;
        return new int[]{n - minimumMoves, maximumMoves};
    }
}
```

__Python__:

```Python
class Solution:
    def numMovesStonesII(self, stones: List[int]) -> List[int]:
        stones.sort()
        maximum_moves, minimum_moves, end = max(stones[-2] - stones[0], stones[-1] - stones[1]) + 2 - (n := len(stones)), 1, 0
        for i, v in enumerate(stones):
            while end < n and stones[end] - v < n: 
                end += 1
            minimum_moves = max(minimum_moves, end - i)
        if minimum_moves == n - 1 and ((stones[-2] - stones[0] == n - 2 and stones[-1] - stones[-2] > 2) or (stones[-1] - stones[1] == n - 2 and stones[1] - stones[0] > 2)): 
            minimum_moves -= 1
        return [n - minimum_moves, maximum_moves]
```
