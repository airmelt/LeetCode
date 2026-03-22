# 2266 Count Number of Texts 统计打字方案数

__Description:__

Alice is texting Bob using her phone. The mapping of digits to letters is shown in the figure below.

![2266-1](https://assets.leetcode.com/uploads/2022/03/15/1200px-telephone-keypad2svg.png)

In order to __add__ a letter, Alice has to __press__ the key of the corresponding digit `i` times, where `i` is the position of the letter in the key.

- For example, to add the letter `'s'`, Alice has to press `'7'` four times. Similarly, to add the letter `'k'`, Alice has to press `'5'` twice.
- Note that the digits `'0'` and `'1'` do not map to any letters, so Alice __does not__ use them.

However, due to an error in transmission, Bob did not receive Alice's text message but received a string of pressed keys instead.

- For example, when Alice sent the message `"bob"`, Bob received the string `"2266622"`.

Given a string `pressedKeys` representing the string received by Bob, return _the __total number of possible text messages__ Alice could have sent_.

Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: pressedKeys = "22233"
Output: 8
Explanation:
The possible text messages Alice could have sent are:
"aaadd", "abdd", "badd", "cdd", "aaae", "abe", "bae", and "ce".
Since there are 8 possible messages, we return 8.
```

Example 2:

```text
Input: pressedKeys = "222222222222222222222222222222222222"
Output: 82876089
Explanation:
There are 2082876103 possible text messages Alice could have sent.
Since we need to return the answer modulo 10 ^ 9 + 7, we return 2082876103 % (109 + 7) = 82876089.
```

__Constraints:__

- `1 <= pressedKeys.length <= 10 ^ 5`
- `pressedKeys` only consists of digits from `'2'` - `'9'`.

__题目描述:__

Alice 在给 Bob 用手机打字。数字到字母的 对应 如下图所示。

![2266-2](https://pic.leetcode.cn/1722224025-gsUAIv-image.png)

为了 __打出__ 一个字母，Alice 需要 __按__ 对应字母 `i` 次， `i` 是该字母在这个按键上所处的位置。

- 比方说，为了按出字母 `'s'` ，Alice 需要按 `'7'` 四次。类似的， Alice 需要按 `'5'` 两次得到字母  `'k'` 。
- 注意，数字 `'0'` 和 `'1'` 不映射到任何字母，所以 Alice __不__ 使用它们。

但是，由于传输的错误，Bob 没有收到 Alice 打字的字母信息，反而收到了 按键的字符串信息 。

- 比方说，Alice 发出的信息为 `"bob"` ，Bob 将收到字符串 `"2266622"` 。

给你一个字符串 `pressedKeys` ，表示 Bob 收到的字符串，请你返回 Alice __总共可能发出多少种文字信息__ 。

由于答案可能很大，将它对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

```text
输入：pressedKeys = "22233"
输出：8
解释：
Alice 可能发出的文字信息包括：
"aaadd", "abdd", "badd", "cdd", "aaae", "abe", "bae" 和 "ce" 。
由于总共有 8 种可能的信息，所以我们返回 8 。
```

示例 2：

```text
输入：pressedKeys = "222222222222222222222222222222222222"
输出：82876089
解释：
总共有 2082876103 种 Alice 可能发出的文字信息。
由于我们需要将答案对 10 ^ 9 + 7 取余，所以我们返回 2082876103 % (109 + 7) = 82876089 。
```

__提示：__

- `1 <= pressedKeys.length <= 10 ^ 5`
- `pressedKeys` 只包含数字 `'2'` 到 `'9'` 。

__思路:__

```text
动态规划
可以看出来需要按按键类型进行分组
各组之间的方案数是独立的
预处理出按键类型为 i 的方案数
7 和 9 需要特殊处理
f[i] 表示按键类型为 i 的方案数
g[i] 表示按键类型为 i 的方案数，且按键类型为 7 和 9 的方案数
f[i] = f[i - 1] + f[i - 2] + f[i - 3]
g[i] = g[i - 1] + g[i - 2] + g[i - 3] + g[i - 4]
遍历 pressedKeys，统计每个按键类型的方案数
如果按键类型不是 7 和 9，则方案数为 f[i]
否则方案数为 g[i]
将所有方案数相乘即为答案
时间复杂度为 O(N), 空间复杂度为 O(M), 其中 M = 100001, 即预处理的空间, N 为 pressedKeys 的长度
```

__代码:__

__C++__:

```C++
class Solution {
    private static final int MOD = 1_000_000_007;
    private static final int M = 100_001;
    private static final long[] f = new long[M];
    private static final long[] g = new long[M];

    static {
        f[0] = g[0] = 1L;
        f[1] = g[1] = 1L;
        f[2] = g[2] = 2L;
        f[3] = g[3] = 4L;
        for (int i = 4; i < M; i++) {
            f[i] = (f[i - 1] + f[i - 2] + f[i - 3]) % MOD;
            g[i] = (g[i - 1] + g[i - 2] + g[i - 3] + g[i - 4]) % MOD;
        }
    }
    public int countTexts(String pressedKeys) {
        long result = 1L;
        for (int i = 0, cur = 1, n = pressedKeys.length(); i < n; i++, cur++) {
            if (i == n - 1 || pressedKeys.charAt(i) != pressedKeys.charAt(i + 1)) {
                result = result * (pressedKeys.charAt(i) != '7' && pressedKeys.charAt(i) != '9' ? f[cur] : g[cur]) % MOD;
                cur = 0;
            }
        }
        return (int)result;
    }
}
```

__Java__:

```Java
class Solution {
    private static final int MOD = 1_000_000_007;
    private static final int M = 100_001;
    private static final long[] f = new long[M];
    private static final long[] g = new long[M];

    static {
        f[0] = g[0] = 1L;
        f[1] = g[1] = 1L;
        f[2] = g[2] = 2L;
        f[3] = g[3] = 4L;
        for (int i = 4; i < M; i++) {
            f[i] = (f[i - 1] + f[i - 2] + f[i - 3]) % MOD;
            g[i] = (g[i - 1] + g[i - 2] + g[i - 3] + g[i - 4]) % MOD;
        }
    }
    public int countTexts(String pressedKeys) {
        long result = 1L;
        for (int i = 0, cur = 1, n = pressedKeys.length(); i < n; i++, cur++) {
            if (i == n - 1 || pressedKeys.charAt(i) != pressedKeys.charAt(i + 1)) {
                result = result * (pressedKeys.charAt(i) != '7' && pressedKeys.charAt(i) != '9' ? f[cur] : g[cur]) % MOD;
                cur = 0;
            }
        }
        return (int)result;
    }
}
```

__Python__:

```Python
MOD, f, g = 10 ** 9 + 7, [1, 1, 2, 4, 7], [1, 1, 2, 4, 8]
for _ in range(5, 100001):
    f.append((f[-1] + f[-1] - f[-4]) % MOD)
    g.append((g[-1] + g[-1] - g[-5]) % MOD)
class Solution:
    def countTexts(self, pressedKeys: str) -> int:
        return reduce(lambda r, t: r * [f, g][t[0] in '79'][len([*t[1]])] % MOD, groupby(pressedKeys), 1)
```
