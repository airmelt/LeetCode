# 2327 Number of People Aware of a Secret 知道秘密的人数

__Description:__

On day `1`, one person discovers a secret.

You are given an integer `delay`, which means that each person will __share__ the secret with a new person __every day__, starting from `delay` days after discovering the secret. You are also given an integer `forget`, which means that each person will __forget__ the secret `forget` days after discovering it. A person __cannot__ share the secret on the same day they forgot it, or on any day afterwards.

Given an integer `n`, return _the number of people who know the secret at the end of day_ `n`. Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: n = 6, delay = 2, forget = 4
Output: 5
Explanation:
Day 1: Suppose the first person is named A. (1 person)
Day 2: A is the only person who knows the secret. (1 person)
Day 3: A shares the secret with a new person, B. (2 people)
Day 4: A shares the secret with a new person, C. (3 people)
Day 5: A forgets the secret, and B shares the secret with a new person, D. (3 people)
Day 6: B shares the secret with E, and C shares the secret with F. (5 people)
```

Example 2:

```text
Input: n = 4, delay = 1, forget = 3
Output: 6
Explanation:
Day 1: The first person is named A. (1 person)
Day 2: A shares the secret with B. (2 people)
Day 3: A and B share the secret with 2 new people, C and D. (4 people)
Day 4: A forgets the secret. B, C, and D share the secret with 3 new people. (6 people)
```

__Constraints:__

- `2 <= n <= 1000`
- `1 <= delay < forget <= n`

__题目描述:__

在第 `1` 天，有一个人发现了一个秘密。

给你一个整数 `delay` ，表示每个人会在发现秘密后的 `delay` 天之后，__每天__ 给一个新的人 __分享__ 秘密。同时给你一个整数 `forget` ，表示每个人在发现秘密 `forget` 天之后会 __忘记__ 这个秘密。一个人 __不能__ 在忘记秘密那一天及之后的日子里分享秘密。

给你一个整数 `n` ，请你返回在第 `n` 天结束时，知道秘密的人数。由于答案可能会很大，请你将结果对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

```text
输入：n = 6, delay = 2, forget = 4
输出：5
解释：
第 1 天：假设第一个人叫 A 。（一个人知道秘密）
第 2 天：A 是唯一一个知道秘密的人。（一个人知道秘密）
第 3 天：A 把秘密分享给 B 。（两个人知道秘密）
第 4 天：A 把秘密分享给一个新的人 C 。（三个人知道秘密）
第 5 天：A 忘记了秘密，B 把秘密分享给一个新的人 D 。（三个人知道秘密）
第 6 天：B 把秘密分享给 E，C 把秘密分享给 F 。（五个人知道秘密）
```

示例 2：

```text
输入：n = 4, delay = 1, forget = 3
输出：6
解释：
第 1 天：第一个知道秘密的人为 A 。（一个人知道秘密）
第 2 天：A 把秘密分享给 B 。（两个人知道秘密）
第 3 天：A 和 B 把秘密分享给 2 个新的人 C 和 D 。（四个人知道秘密）
第 4 天：A 忘记了秘密，B、C、D 分别分享给 3 个新的人。（六个人知道秘密）
```

__提示：__

- `2 <= n <= 1000`
- `1 <= delay < forget <= n`

__思路:__

```text
动态规划
设 f[i] 表示第 i 天新知道秘密的人数
f[i] = sum(f[j]), i - forget + 1 <= j <= i - delay
令 dp[i] = sum(f[i]), 即 f[i] 的前缀和
dp[i] = dp[i - 1] + f[i]
f[i] = dp[i - delay] - dp[i - forget], i >= delay, i >= forget, 否则取 dp[0]
初始化 dp[1] = 1
由于 f[i] 只与 dp[i - delay] 和 dp[i - forget] 有关，可以使用滚动数组优化空间
最终结果为 dp[n] - dp[n - forget]
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int peopleAwareOfSecret(int n, int delay, int forget) 
    {
        vector<int> dp(n + 1);
        dp[1] = 1;
        for (int i = 2, f = 0, MOD = 1e9 + 7; i <= n; i++) dp[i] = (dp[i - 1] + (f = (dp[max(i - delay, 0)] - dp[max(i - forget, 0)]) % MOD)) % MOD;
        return ((dp[n] - dp[max(n - forget, 0)]) % ((int)(1e9 + 7)) + ((int)(1e9 + 7))) % ((int)(1e9 + 7));
    }
};
```

__Java__:

```Java
class Solution {
    public int peopleAwareOfSecret(int n, int delay, int forget) {
        int dp[] = new int[n + 1], f = 0, MOD = 1_000_000_007;
        dp[1] = 1;
        for (int i = 2; i <= n; i++) dp[i] = (dp[i - 1] + (f = (dp[Math.max(i - delay, 0)] - dp[Math.max(i - forget, 0)]) % MOD)) % MOD;
        return ((dp[n] - dp[Math.max(n - forget, 0)]) % MOD + MOD) % MOD;
    }
}
```

__Python__:

```Python
class Solution:
    def peopleAwareOfSecret(self, n: int, delay: int, forget: int) -> int:
        dp, MOD = [0, 1] + [0] * (n - 1), 10 ** 9 + 7
        for i in range(2, n + 1):
            dp[i] = dp[i - 1] + (f := dp[max(0, i - delay)] - dp[max(0, i - forget)])
        return (dp[-1] - dp[max(0, n - forget)]) % MOD
```
