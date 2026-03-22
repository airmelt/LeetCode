# 1927 Sum Game 求和游戏

__Description:__

Alice and Bob take turns playing a game, with Alice starting first.

You are given a string `num` of __even length__ consisting of digits and `'?'` characters. On each turn, a player will do the following if there is still at least one `'?'` in `num`:

The game ends when there are no more `'?'` characters in `num`.

For Bob to win, the sum of the digits in the first half of `num` must be __equal__ to the sum of the digits in the second half. For Alice to win, the sums must __not be equal__.

- For example, if the game ended with `num = "243801"`, then Bob wins because `2+4+3 = 8+0+1`. If the game ended with `num = "243803"`, then Alice wins because `2+4+3 != 8+0+3`.

Assuming Alice and Bob play __optimally__, return `true` _if Alice will win and_ `false` _if Bob will win_.

__Example:__

Example 1:

```text
Input: num = "5023"
Output: false
Explanation: There are no moves to be made.
The sum of the first half is equal to the sum of the second half: 5 + 0 = 2 + 3.
```

Example 2:

```text
Input: num = "25??"
Output: true
Explanation: Alice can replace one of the '?'s with '9' and it will be impossible for Bob to make the sums equal.
```

Example 3:

```text
Input: num = "?3295???"
Output: false
Explanation: It can be proven that Bob will always win. One possible outcome is:
- Alice replaces the first '?' with '9'. num = "93295???".
- Bob replaces one of the '?' in the right half with '9'. num = "932959??".
- Alice replaces one of the '?' in the right half with '2'. num = "9329592?".
- Bob replaces the last '?' in the right half with '7'. num = "93295927".
Bob wins because 9 + 3 + 2 + 9 = 5 + 9 + 2 + 7.
```

__Constraints:__

- `2 <= num.length <= 10 ^ 5`
- `num.length` is __even__.
- `num` consists of only digits and `'?'`.

__题目描述:__

Alice 和 Bob 玩一个游戏，两人轮流行动，Alice 先手 。

给你一个 __偶数长度__ 的字符串 `num` ，每一个字符为数字字符或者 `'?'` 。每一次操作中，如果 `num` 中至少有一个 `'?'` ，那么玩家可以执行以下操作:

当 `num` 中没有 `'?'` 时，游戏结束。

Bob 获胜的条件是 `num` 中前一半数字的和 __等于__ 后一半数字的和。Alice 获胜的条件是前一半的和与后一半的和 __不相等__ 。

- 比方说，游戏结束时 `num = "243801"` ，那么 Bob 获胜，因为 `2+4+3 = 8+0+1` 。如果游戏结束时 `num = "243803"` ，那么 Alice 获胜，因为 `2+4+3 != 8+0+3` 。

在 Alice 和 Bob 都采取 __最优__ 策略的前提下，如果 Alice 获胜，请返回 `true` ，如果 Bob 获胜，请返回 `false` 。

__示例:__

示例 1：

```text
输入：num = "5023"
输出：false
解释：num 中没有 '?' ，没法进行任何操作。
前一半的和等于后一半的和：5 + 0 = 2 + 3 。
```

示例 2：

```text
输入：num = "25??"
输出：true
解释：Alice 可以将两个 '?' 中的一个替换为 '9' ，Bob 无论如何都无法使前一半的和等于后一半的和。
```

示例 3：

```text
输入：num = "?3295???"
输出：false
解释：Bob 总是能赢。一种可能的结果是：
- Alice 将第一个 '?' 用 '9' 替换。num = "93295???" 。
- Bob 将后面一半中的一个 '?' 替换为 '9' 。num = "932959??" 。
- Alice 将后面一半中的一个 '?' 替换为 '2' 。num = "9329592?" 。
- Bob 将后面一半中最后一个 '?' 替换为 '7' 。num = "93295927" 。
Bob 获胜，因为 9 + 3 + 2 + 9 = 5 + 9 + 2 + 7 。
```

__提示：__

- `2 <= num.length <= 10 ^ 5`
- `num.length` 是 __偶数__ 。
- `num` 只包含数字字符和 `'?'` 。

__思路:__

```text
模拟
计数分别求前一半和后一半的 '?' 的个数 cnt1, cnt2 和数字的和 sum1, sum2
如果 '?' 的个数为奇数, 则 Alice 必胜
或者极端情况下把 Alice 把所有左边的 '?' 都替换为 '9', Bob 把右边的 '?' 都尽可能替换成 '9' 左右两侧相差 9 * (cnt1 - cnt2) / 2 + sum1 - sum2, 如果不等于 0 则 Alice 获胜
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool sumGame(string num) 
    {
        int n = num.size(), sum1 = 0, cnt1 = 0, sum2 = 0, cnt2 = 0;
        for (int i = 0; i < n >> 1; i++) sum1 += (num[i] == '?' ? 0 : (num[i] - '0')), cnt1 += (num[i] == '?'), sum2 += (num[i + (n >> 1)] == '?' ? 0 : (num[i + (n >> 1)] - '0')), cnt2 += (num[i + (n >> 1)] == '?');
        return (cnt1 + cnt2) & 1 or 9 * ((cnt1 - cnt2) >> 1) + sum1 - sum2 != 0;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean sumGame(String num) {
        int n = num.length(), sum1 = 0, cnt1 = 0, sum2 = 0, cnt2 = 0;
        for (int i = 0; i < n >> 1; i++) {
            sum1 += (num.charAt(i) == '?' ? 0 : (num.charAt(i) - '0'));
            cnt1 += (num.charAt(i) == '?' ? 1 : 0);
            sum2 += (num.charAt(i + (n >> 1)) == '?' ? 0 : (num.charAt(i + (n >> 1)) - '0'));
            cnt2 += (num.charAt(i + (n >> 1)) == '?' ? 1 : 0);
        }
        return (((cnt1 + cnt2) & 1) == 1) || 9 * ((cnt1 - cnt2) >> 1) + sum1 - sum2 != 0;
    }
}
```

__Python__:

```Python
class Solution:
    def sumGame(self, num: str) -> bool:
        return not not (sum(num[i] == '?' for i in range(len(num) >> 1)) + sum(num[i + (len(num) >> 1)] == '?' for i in range(len(num) >> 1))) & 1 or not not 9 * ((sum(num[i] == '?' for i in range(len(num) >> 1)) - sum(num[i + (len(num) >> 1)] == '?' for i in range(len(num) >> 1))) >> 1) + sum(int(num[i]) for i in range(len(num) >> 1) if num[i] != '?') - sum(int(num[i + (len(num) >> 1)]) for i in range(len(num) >> 1) if num[i + (len(num) >> 1)] != '?')
```
