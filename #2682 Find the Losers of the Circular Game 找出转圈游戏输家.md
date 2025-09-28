# 2682 Find the Losers of the Circular Game 找出转圈游戏输家

__Description:__

There are `n` friends that are playing a game. The friends are sitting in a circle and are numbered from `1` to `n` in __clockwise order__. More formally, moving clockwise from the `i ^ th` friend brings you to the `(i+1) ^ th` friend for `1 <= i < n`, and moving clockwise from the `n ^ th` friend brings you to the `1 ^ st` friend.

The rules of the game are as follows:

`1 ^ st` friend receives the ball.

- After that, `1 ^ st` friend passes it to the friend who is `k` steps away from them in the __clockwise__ direction.
- After that, the friend who receives the ball should pass it to the friend who is `2 * k` steps away from them in the __clockwise__ direction.
- After that, the friend who receives the ball should pass it to the friend who is `3 * k` steps away from them in the __clockwise__ direction, and so on and so forth.

In other words, on the `i ^ th` turn, the friend holding the ball should pass it to the friend who is `i * k` steps away from them in the __clockwise__ direction.

The game is finished when some friend receives the ball for the second time.

The losers of the game are friends who did not receive the ball in the entire game.

Given the number of friends, `n`, and an integer `k`, return _the array answer, which contains the losers of the game in the __ascending__ order_.

__Example:__

Example 1:

```text
Input: n = 5, k = 2
Output: [4,5]
Explanation: The game goes as follows:
1) Start at 1st friend and pass the ball to the friend who is 2 steps away from them - 3rd friend.
2) 3rd friend passes the ball to the friend who is 4 steps away from them - 2nd friend.
3) 2nd friend passes the ball to the friend who is 6 steps away from them  - 3rd friend.
4) The game ends as 3rd friend receives the ball for the second time.
```

Example 2:

```text
Input: n = 4, k = 4
Output: [2,3,4]
Explanation: The game goes as follows:
1) Start at the 1st friend and pass the ball to the friend who is 4 steps away from them - 1st friend.
2) The game ends as 1st friend receives the ball for the second time.
```

__Constraints:__

- `1 <= k <= n <= 50`

__题目描述:__

`n` 个朋友在玩游戏。这些朋友坐成一个圈，按 __顺时针方向__ 从 `1` 到 `n` 编号。准确的说，从第 `i` 个朋友的位置开始顺时针移动 `1` 步会到达第 `(i + 1)` 个朋友的位置（ `1 <= i < n`），而从第 `n` 个朋友的位置开始顺时针移动 `1` 步会回到第 `1` 个朋友的位置。

游戏规则如下：

第 `1` 个朋友接球。

- 接着，第 `1` 个朋友将球传给距离他顺时针方向 `k` 步的朋友。
- 然后，接球的朋友应该把球传给距离他顺时针方向 `2 * k` 步的朋友。
- 接着，接球的朋友应该把球传给距离他顺时针方向 `3 * k` 步的朋友，以此类推。

换句话说，在第 `i` 轮中持有球的那位朋友需要将球传递给距离他顺时针方向 `i * k` 步的朋友。

当某个朋友第 2 次接到球时，游戏结束。

在整场游戏中没有接到过球的朋友是 输家 。

给你参与游戏的朋友数量 `n` 和一个整数 `k` ，请按升序排列返回包含所有输家编号的数组 `answer` 作为答案。

__示例:__

示例 1：

```text
输入：n = 5, k = 2
输出：[4,5]
解释：以下为游戏进行情况：
1）第 1 个朋友接球，第 1 个朋友将球传给距离他顺时针方向 2 步的玩家 —— 第 3 个朋友。
2）第 3 个朋友将球传给距离他顺时针方向 4 步的玩家 —— 第 2 个朋友。
3）第 2 个朋友将球传给距离他顺时针方向 6 步的玩家 —— 第 3 个朋友。
4）第 3 个朋友接到两次球，游戏结束。
```

示例 2：

```text
输入：n = 4, k = 4
输出：[2,3,4]
解释：以下为游戏进行情况：
1）第 1 个朋友接球，第 1 个朋友将球传给距离他顺时针方向 4 步的玩家 —— 第 1 个朋友。
2）第 1 个朋友接到两次球，游戏结束。
```

__提示：__

- `1 <= k <= n <= 50`

__思路:__

```text
模拟
按照题意模拟
使用布尔数组记录已经接到球的人
统计未接到球的人
为了方便可以最后返回的时候再修改下标
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> circularGameLosers(int n, int k) 
    {
        bitset<50> visited;
        for (int i = 0, p = k; !visited[i]; i = (i + p) % n, p += k) visited.set(i);
        vector<int> result;
        for (int i = 0; i < n; i++) if (!visited[i]) result.emplace_back(i + 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] circularGameLosers(int n, int k) {
        boolean[] visited = new boolean[n];
        for (int i = 0, p = k; !visited[i]; i = (i + p) % n, p += k) visited[i] = true;
        var result = new ArrayList<Integer>();
        for (int i = 0; i < n; i++) if (!visited[i]) result.add(i + 1);
        return result.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

__Python__:

```Python
class Solution:
    def circularGameLosers(self, n: int, k: int) -> List[int]:
        i, p, visited = 0, 1, [False] * n
        while not visited[i]:
            visited[i] = True
            i = (i + ((p := p + 1) - 1) * k) % n
        return [i + 1 for i, v in enumerate(visited) if not v]
```
