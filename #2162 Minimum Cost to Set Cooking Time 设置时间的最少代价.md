# 2162 Minimum Cost to Set Cooking Time 设置时间的最少代价

__Description:__

A generic microwave supports cooking times for:

- at least `1` second.
- at most `99` minutes and `99` seconds.

To set the cooking time, you push at most four digits. The microwave normalizes what you push as four digits by prepending zeroes. It interprets the first two digits as the minutes and the last two digits as the seconds. It then adds them up as the cooking time. For example,

- You push `9` `5` `4` (three digits). It is normalized as `0954` and interpreted as `9` minutes and `54` seconds.
- You push `0` `0` `0` `8` (four digits). It is interpreted as `0` minutes and `8` seconds.
- You push `8` `0` `9` `0`. It is interpreted as `80` minutes and `90` seconds.
- You push `8` `1` `3` `0`. It is interpreted as `81` minutes and `30` seconds.

You are given integers `startAt`, `moveCost`, `pushCost`, and `targetSeconds`. __Initially__, your finger is on the digit `startAt`. Moving the finger above __any specific digit__ costs `moveCost` units of fatigue. Pushing the digit below the finger __once__ costs `pushCost` units of fatigue.

There can be multiple ways to set the microwave to cook for `targetSeconds` seconds but you are interested in the way with the minimum cost.

Return _the __minimum cost__ to set_ `targetSeconds` _seconds of cooking time_.

Remember that one minute consists of `60` seconds.

__Example:__

Example 1:

![2162-1](https://assets.leetcode.com/uploads/2021/12/30/1.png)

```text
Input: startAt = 1, moveCost = 2, pushCost = 1, targetSeconds = 600
Output: 6
Explanation: The following are the possible ways to set the cooking time.
```

- 1 0 0 0, interpreted as 10 minutes and 0 seconds.
  The finger is already on digit 1, pushes 1 (with cost 1), moves to 0 (with cost 2), pushes 0 (with cost 1), pushes 0 (with cost 1), and pushes 0 (with cost 1).
  The cost is: 1 + 2 + 1 + 1 + 1 = 6. This is the minimum cost.
- 0 9 6 0, interpreted as 9 minutes and 60 seconds. That is also 600 seconds.
  The finger moves to 0 (with cost 2), pushes 0 (with cost 1), moves to 9 (with cost 2), pushes 9 (with cost 1), moves to 6 (with cost 2), pushes 6 (with cost 1), moves to 0 (with cost 2), and pushes 0 (with cost 1).
  The cost is: 2 + 1 + 2 + 1 + 2 + 1 + 2 + 1 = 12.
- 9 6 0, normalized as 0960 and interpreted as 9 minutes and 60 seconds.
  The finger moves to 9 (with cost 2), pushes 9 (with cost 1), moves to 6 (with cost 2), pushes 6 (with cost 1), moves to 0 (with cost 2), and pushes 0 (with cost 1).
  The cost is: 2 + 1 + 2 + 1 + 2 + 1 = 9.

Example 2:

![2162-2](https://assets.leetcode.com/uploads/2021/12/30/2.png)

```text
Input: startAt = 0, moveCost = 1, pushCost = 2, targetSeconds = 76
Output: 6
Explanation: The optimal way is to push two digits: 7 6, interpreted as 76 seconds.
The finger moves to 7 (with cost 1), pushes 7 (with cost 2), moves to 6 (with cost 1), and pushes 6 (with cost 2). The total cost is: 1 + 2 + 1 + 2 = 6
Note other possible ways are 0076, 076, 0116, and 116, but none of them produces the minimum cost.
```

__Constraints:__

- `0 <= startAt <= 9`
- `1 <= moveCost, pushCost <= 10 ^ 5`
- `1 <= targetSeconds <= 6039`

__题目描述:__

常见的微波炉可以设置加热时间，且加热时间满足以下条件：

- 至少为 `1` 秒钟。
- 至多为 `99` 分 `99` 秒。

你可以 最多 输入 4 个数字 来设置加热时间。如果你输入的位数不足 4 位，微波炉会自动加 前缀 0 来补足 4 位。微波炉会将设置好的四位数中，前 两位当作分钟数，后 两位当作秒数。它们所表示的总时间就是加热时间。比方说：

- 你输入 `9` `5` `4` （三个数字），被自动补足为 `0954` ，并表示 `9` 分 `54` 秒。
- 你输入 `0` `0` `0` `8` （四个数字），表示 `0` 分 `8` 秒。
- 你输入 `8` `0` `9` `0` ，表示 `80` 分 `90` 秒。
- 你输入 `8` `1` `3` `0` ，表示 `81` 分 `30` 秒。

给你整数 `startAt` ， `moveCost` ， `pushCost` 和 `targetSeconds` 。__一开始__，你的手指在数字 `startAt` 处。将手指移到 __任何其他数字__ ，需要花费 `moveCost` 的单位代价。__每__ 输入你手指所在位置的数字一次，需要花费 `pushCost` 的单位代价。

要设置 `targetSeconds` 秒的加热时间，可能会有多种设置方法。你想要知道这些方法中，总代价最小为多少。

请你能返回设置 `targetSeconds` 秒钟加热时间需要花费的最少代价。

请记住，虽然微波炉的秒数最多可以设置到 `99` 秒，但一分钟等于 `60` 秒。

__示例:__

示例 1：

![2162-3](https://assets.leetcode.com/uploads/2021/12/30/1.png)

```text
输入：startAt = 1, moveCost = 2, pushCost = 1, targetSeconds = 600
输出：6
解释：以下为设置加热时间的所有方法。
```

- 1 0 0 0 ，表示 10 分 0 秒。
  手指一开始就在数字 1 处，输入 1 （代价为 1），移到 0 处（代价为 2），输入 0（代价为 1），输入 0（代价为 1），输入 0（代价为 1）。
  总代价为：1 + 2 + 1 + 1 + 1 = 6 。这是所有方案中的最小代价。
- 0 9 6 0，表示 9 分 60 秒。它也表示 600 秒。
  手指移到 0 处（代价为 2），输入 0 （代价为 1），移到 9 处（代价为 2），输入 9（代价为 1），移到 6 处（代价为 2），输入 6（代价为 1），移到 0 处（代价为 2），输入 0（代价为 1）。
  总代价为：2 + 1 + 2 + 1 + 2 + 1 + 2 + 1 = 12 。
- 9 6 0，微波炉自动补全为 0960 ，表示 9 分 60 秒。
  手指移到 9 处（代价为 2），输入 9 （代价为 1），移到 6 处（代价为 2），输入 6（代价为 1），移到 0 处（代价为 2），输入 0（代价为 1）。
  总代价为：2 + 1 + 2 + 1 + 2 + 1 = 9 。

示例 2：

![2162-4](https://assets.leetcode.com/uploads/2021/12/30/2.png)

```text
输入：startAt = 0, moveCost = 1, pushCost = 2, targetSeconds = 76
输出：6
解释：最优方案为输入两个数字 7 6，表示 76 秒。
手指移到 7 处（代价为 1），输入 7 （代价为 2），移到 6 处（代价为 1），输入 6（代价为 2）。总代价为：1 + 2 + 1 + 2 = 6
其他可行方案为 0076 ，076 ，0116 和 116 ，但是它们的代价都比 6 大。
```

__提示：__

- `0 <= startAt <= 9`
- `1 <= moveCost, pushCost <= 10 ^ 5`
- `1 <= targetSeconds <= 6039`

__思路:__

```text
模拟
只有两种方式来输入
要么秒数 / 60 为分钟数 m, 秒数 % 60 为剩下的秒数 s
要么秒数 / 60 - 1 为分钟数 m, 秒数 % 60 + 60 为剩下的秒数 s, 即借用 1 分钟给秒数
遍历两种方式, 计算代价, 取最小值
m 和 s 需要在 0 ~ 99 之间
如果不合法, 返回 INT_MAX
否则找到第一个不为 0 的位置开始计算
如果和前一个位置不相等加上 moveCost
每次遍历加上 pushCost
并更新 pre
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minCostSetTime(int startAt, int moveCost, int pushCost, int targetSeconds) 
    {
        return min(helper(targetSeconds / 60, targetSeconds % 60, startAt, moveCost, pushCost), helper(targetSeconds / 60 - 1, targetSeconds % 60 + 60, startAt, moveCost, pushCost));
    }
private:
    int helper(int m, int s, int pre, int moveCost, int pushCost) 
    {
        if (m < 0 or m > 99 or s < 0 or s > 99) return INT_MAX;
        vector<int> arr{m / 10, m % 10, s / 10, s % 10};
        int i = m / 10 ? 0 : (m % 10 ? 1 : (s / 10 ? 2 : (s % 10 ? 3 : 4))), cost = 0;
        for (; i < 4; ++i) 
        {
            cost += (arr[i] != pre) * moveCost + pushCost;
            pre = arr[i];
        }
        return cost;
    }
};
```

__Java__:

```Java
class Solution {
    public int minCostSetTime(int startAt, int moveCost, int pushCost, int targetSeconds) {
        return Math.min(helper(targetSeconds / 60, targetSeconds % 60, startAt, moveCost, pushCost), helper(targetSeconds / 60 - 1, targetSeconds % 60 + 60, startAt, moveCost, pushCost));
    }

    private int helper(int m, int s, int pre, int moveCost, int pushCost) {
        if (m < 0 || m > 99 || s < 0 || s > 99) return Integer.MAX_VALUE;
        int arr[] = new int[] {m / 10, m % 10, s / 10, s % 10}, i = m / 10 != 0 ? 0 : (m % 10 != 0 ? 1 : (s / 10 != 0 ? 2 : (s % 10 != 0 ? 3 : 4))), cost = 0;
        for (; i < 4; i++) {
            cost += (arr[i] != pre ? 1 : 0) * moveCost + pushCost;
            pre = arr[i];
        }
        return cost;
    }
}
```

__Python__:

```Python
class Solution:
    def minCostSetTime(self, startAt: int, moveCost: int, pushCost: int, targetSeconds: int) -> int:
        def helper(m: int, s: int, pre: int) -> int:
            if m < 0 or m > 99 or s < 0 or s > 99:
                return inf
            arr, i, cost = [m // 10, m % 10, s // 10, s % 10], 0 if m // 10 else 1 if m % 10 else 2 if s // 10 else 3 if s % 10 else 4, 0
            while i < 4:
                cost += (arr[i] != pre) * moveCost + pushCost
                pre = arr[i]
                i += 1
            return cost
        return min(helper(targetSeconds // 60, targetSeconds % 60, startAt), helper(targetSeconds // 60 - 1, targetSeconds % 60 + 60, startAt))
```
