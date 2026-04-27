# 2139 Minimum Moves to Reach Target Score 得到目标值的最少行动次数

__Description:__

You are playing a game with integers. You start with the integer `1` and you want to reach the integer `target`.

In one move, you can either:

- __Increment__ the current integer by one (i.e., `x = x + 1`).
- __Double__ the current integer (i.e., `x = 2 * x`).

You can use the __increment__ operation __any__ number of times, however, you can only use the __double__ operation __at most__ `maxDoubles` times.

Given the two integers `target` and `maxDoubles`, return _the minimum number of moves needed to reach_ `target` _starting with_ `1`.

__Example:__

Example 1:

```text
Input: target = 5, maxDoubles = 0
Output: 4
Explanation: Keep incrementing by 1 until you reach target.
```

Example 2:

```text
Input: target = 19, maxDoubles = 2
Output: 7
Explanation: Initially, x = 1
Increment 3 times so x = 4
Double once so x = 8
Increment once so x = 9
Double again so x = 18
Increment once so x = 19
```

Example 3:

```text
Input: target = 10, maxDoubles = 4
Output: 4
Explanation: Initially, x = 1
Increment once so x = 2
Double once so x = 4
Increment once so x = 5
Double again so x = 10
```

__Constraints:__

- `1 <= target <= 10 ^ 9`
- `0 <= maxDoubles <= 100`

__题目描述:__

你正在玩一个整数游戏。从整数 `1` 开始，期望得到整数 `target` 。

在一次行动中，你可以做下述两种操作之一：

- __递增__，将当前整数的值加 1（即， `x = x + 1`）。
- __加倍__，使当前整数的值翻倍（即， `x = 2 * x`）。

在整个游戏过程中，你可以使用 __递增__ 操作 __任意__ 次数。但是只能使用 __加倍__ 操作 __至多__ `maxDoubles` 次。

给你两个整数 `target` 和 `maxDoubles` ，返回从 1 开始得到 `target` 需要的最少行动次数。

__示例:__

示例 1：

```text
输入：target = 5, maxDoubles = 0
输出：4
解释：一直递增 1 直到得到 target 。
```

示例 2：

```text
输入：target = 19, maxDoubles = 2
输出：7
解释：最初，x = 1 。
递增 3 次，x = 4 。
加倍 1 次，x = 8 。
递增 1 次，x = 9 。
加倍 1 次，x = 18 。
递增 1 次，x = 19 。
```

示例 3：

```text
输入：target = 10, maxDoubles = 4
输出：4
解释：
最初，x = 1 。 
递增 1 次，x = 2 。 
加倍 1 次，x = 4 。 
递增 1 次，x = 5 。 
加倍 1 次，x = 10 。
```

__提示：__

- `1 <= target <= 10 ^ 9`
- `0 <= maxDoubles <= 100`

__思路:__

```text
贪心
类似用快速幂的方式
尽快从 target 减少到 1
如果 target 是奇数，那么只能递减
如果 maxDoubles 为 0，那么只能递减, 返回结果加上 target - 1
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minMoves(int target, int maxDoubles) 
    {
        int result = 0;
        while (target > 1) 
        {
            if (!maxDoubles) return result + target - 1;
            if (target & 1) 
            {
                --target;
                ++result;
            }
            --maxDoubles;
            target >>= 1;
            ++result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minMoves(int target, int maxDoubles) {
        int result = 0;
        while (target > 1) {
            if (maxDoubles == 0) return result + target - 1;
            if ((target & 1) == 1) {
                --target;
                ++result;
            }
            --maxDoubles;
            target >>>= 1;
            ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minMoves(self, target: int, maxDoubles: int) -> int:
        result = 0
        while target > 1:
            if not maxDoubles:
                return result + target - 1
            if target & 1 > 0:
                target -= 1
                result += 1
            maxDoubles -= 1
            target >>= 1
            result += 1
        return result
```
