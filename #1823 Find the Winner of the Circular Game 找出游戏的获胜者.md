# 1823 Find the Winner of the Circular Game 找出游戏的获胜者

__Description:__

There are `n` friends that are playing a game. The friends are sitting in a circle and are numbered from `1` to `n` in __clockwise order__. More formally, moving clockwise from the `i ^ th` friend brings you to the `(i+1) ^ th` friend for `1 <= i < n`, and moving clockwise from the `n ^ th` friend brings you to the `1 ^ st` friend.

The rules of the game are as follows:

Given the number of friends, `n`, and an integer `k`, return _the winner of the game_.

__Example:__

Example 1:

![1823-1](https://assets.leetcode.com/uploads/2021/03/25/ic234-q2-ex11.png)

```text
Input: n = 5, k = 2
Output: 3
Explanation: Here are the steps of the game:
1) Start at friend 1.
2) Count 2 friends clockwise, which are friends 1 and 2.
3) Friend 2 leaves the circle. Next start is friend 3.
4) Count 2 friends clockwise, which are friends 3 and 4.
5) Friend 4 leaves the circle. Next start is friend 5.
6) Count 2 friends clockwise, which are friends 5 and 1.
7) Friend 1 leaves the circle. Next start is friend 3.
8) Count 2 friends clockwise, which are friends 3 and 5.
9) Friend 5 leaves the circle. Only friend 3 is left, so they are the winner.
```

Example 2:

```text
Input: n = 6, k = 5
Output: 1
Explanation: The friends leave in this order: 5, 4, 6, 2, 3. The winner is friend 1.
```

__Constraints:__

- `1 <= k <= n <= 500`

Follow up:

Could you solve this problem in linear time with constant space?

__题目描述:__

共有 `n` 名小伙伴一起做游戏。小伙伴们围成一圈，按 __顺时针顺序__ 从 `1` 到 `n` 编号。确切地说，从第 `i` 名小伙伴顺时针移动一位会到达第 `(i+1)` 名小伙伴的位置，其中 `1 <= i < n` ，从第 `n` 名小伙伴顺时针移动一位会回到第 `1` 名小伙伴的位置。

游戏遵循如下规则：

给你参与游戏的小伙伴总数 `n` ，和一个整数 `k` ，返回游戏的获胜者。

__示例:__

示例 1：

![1823-2](https://assets.leetcode.com/uploads/2021/03/25/ic234-q2-ex11.png)

```text
输入：n = 5, k = 2
输出：3
解释：游戏运行步骤如下：
1) 从小伙伴 1 开始。
2) 顺时针数 2 名小伙伴，也就是小伙伴 1 和 2 。
3) 小伙伴 2 离开圈子。下一次从小伙伴 3 开始。
4) 顺时针数 2 名小伙伴，也就是小伙伴 3 和 4 。
5) 小伙伴 4 离开圈子。下一次从小伙伴 5 开始。
6) 顺时针数 2 名小伙伴，也就是小伙伴 5 和 1 。
7) 小伙伴 1 离开圈子。下一次从小伙伴 3 开始。
8) 顺时针数 2 名小伙伴，也就是小伙伴 3 和 5 。
9) 小伙伴 5 离开圈子。只剩下小伙伴 3 。所以小伙伴 3 是游戏的获胜者。
```

示例 2：

```text
输入：n = 6, k = 5
输出：1
解释：小伙伴离开圈子的顺序：5、4、6、2、3 。小伙伴 1 是游戏的获胜者。
```

__提示：__

- `1 <= k <= n <= 500`

进阶：你能否使用线性时间复杂度和常数空间复杂度解决此问题？

__思路:__

```text
数学
约瑟夫环
f(1, k) = 1
f(n, k) = (f(n - 1, k) + k) % n
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findTheWinner(int n, int k) 
    {
        int p = 0;
        for (int i = 1; i < n; i++) p = (p + k) % (i + 1);
        return p + 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int findTheWinner(int n, int k) {
        return n == 1 ? 1 : (k + findTheWinner(n - 1, k) - 1) % n + 1;
    }
}
```

__Python__:

```Python
class Solution:
    def findTheWinner(self, n: int, k: int) -> int:
        p = 0
        for i in range(2, n + 1):
            p = (p + k) % i
        return p + 1
```
