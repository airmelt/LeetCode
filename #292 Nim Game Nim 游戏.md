# 292 Nim Game Nim 游戏

__Description__:
You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.

Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.

**Example :**

Input: 4
Output: false
Explanation: If there are 4 stones in the heap, then you will never win the game;
             No matter 1, 2, or 3 stones you remove, the last stone will always be
             removed by your friend.

__题目描述__:
你和你的朋友，两个人一起玩 Nim 游戏：桌子上有一堆石头，每次你们轮流拿掉 1 - 3 块石头。 拿掉最后一块石头的人就是获胜者。你作为先手。

你们是聪明人，每一步都是最优解。 编写一个函数，来判断你是否可以在给定石头数量的情况下赢得游戏。

**示例 :**

输入: 4
输出: false
解释: 如果堆中有 4 块石头，那么你永远不会赢得比赛；
     因为无论你拿走 1 块、2 块 还是 3 块石头，最后一块石头总是会被你的朋友拿走。

__思路__:

实际上就是输出 n % 4 != 0, 当 n是 4的倍数时必败, 否则每次取到 4的倍数即可胜利
可以转换为求 n & 3(0b11) != 0
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool canWinNim(int n) 
    {
        return (n & (1 << 2) - 1) != 0;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canWinNim(int n) {
        return (n & (1 << 2) - 1) != 0;
    }
}
```

__Python__:

```Python
class Solution:
    def canWinNim(self, n: int) -> bool:
        return (n & (1 << 2) - 1) != 0
```
