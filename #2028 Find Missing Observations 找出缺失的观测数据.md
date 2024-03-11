# 2028 Find Missing Observations 找出缺失的观测数据

__Description:__

You have observations of `n + m` __6-sided__ dice rolls with each face numbered from `1` to `6`. `n` of the observations went missing, and you only have the observations of `m` rolls. Fortunately, you have also calculated the __average value__ of the `n + m` rolls.

You are given an integer array `rolls` of length `m` where `rolls[i]` is the value of the `i ^ th` observation. You are also given the two integers `mean` and `n`.

Return _an array of length_ `n` _containing the missing observations such that the __average value__ of the_ `n + m` _rolls is __exactly___ `mean`. If there are multiple valid answers, return _any of them_. If no such array exists, return _an empty array_.

The __average value__ of a set of `k` numbers is the sum of the numbers divided by `k`.

Note that `mean` is an integer, so the sum of the `n + m` rolls should be divisible by `n + m`.

__Example:__

Example 1:

```text
Input: rolls = [3,2,4,3], mean = 4, n = 2
Output: [6,6]
Explanation: The mean of all n + m rolls is (3 + 2 + 4 + 3 + 6 + 6) / 6 = 4.
```

Example 2:

```text
Input: rolls = [1,5,6], mean = 3, n = 4
Output: [2,3,2,2]
Explanation: The mean of all n + m rolls is (1 + 5 + 6 + 2 + 3 + 2 + 2) / 7 = 3.
```

Example 3:

```text
Input: rolls = [1,2,3,4], mean = 6, n = 4
Output: []
Explanation: It is impossible for the mean to be 6 no matter what the 4 missing rolls are.
```

__Constraints:__

- `m == rolls.length`
- `1 <= n, m <= 10 ^ 5`
- `1 <= rolls[i], mean <= 6`

__题目描述:__

现有一份 `n + m` 次投掷单个 __六面__ 骰子的观测数据，骰子的每个面从 `1` 到 `6` 编号。观测数据中缺失了 `n` 份，你手上只拿到剩余 `m` 次投掷的数据。幸好你有之前计算过的这 `n + m` 次投掷数据的 __平均值__ 。

给你一个长度为 `m` 的整数数组 `rolls` ，其中 `rolls[i]` 是第 `i` 次观测的值。同时给你两个整数 `mean` 和 `n` 。

返回一个长度为 `n` 的数组，包含所有缺失的观测数据，且满足这 `n + m` 次投掷的 __平均值__ 是 `mean` 。如果存在多组符合要求的答案，只需要返回其中任意一组即可。如果不存在答案，返回一个空数组。

`k` 个数字的 __平均值__ 为这些数字求和后再除以 `k` 。

注意 `mean` 是一个整数，所以 `n + m` 次投掷的总和需要被 `n + m` 整除。

__示例:__

示例 1：

```text
输入：rolls = [3,2,4,3], mean = 4, n = 2
输出：[6,6]
解释：所有 n + m 次投掷的平均值是 (3 + 2 + 4 + 3 + 6 + 6) / 6 = 4 。
```

示例 2：

```text
输入：rolls = [1,5,6], mean = 3, n = 4
输出：[2,3,2,2]
解释：所有 n + m 次投掷的平均值是 (1 + 5 + 6 + 2 + 3 + 2 + 2) / 7 = 3 。
```

示例 3：

```text
输入：rolls = [1,2,3,4], mean = 6, n = 4
输出：[]
解释：无论丢失的 4 次数据是什么，平均值都不可能是 6 。
```

示例 4：

```text
输入：rolls = [1], mean = 3, n = 1
输出：[5]
解释：所有 n + m 次投掷的平均值是 (1 + 5) / 2 = 3 。
```

__提示：__

- `m == rolls.length`
- `1 <= n, m <= 10 ^ 5`
- `1 <= rolls[i], mean <= 6`

__思路:__

```text
贪心
先计算出总和 s = mean * (n + m) - total
如果 s < n 或者 s > 6 * n, 返回空数组
否则，返回 s // n + 1 的个数为 s % n, 返回 s // n 的个数为 n - s % n
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> missingRolls(vector<int>& rolls, int mean, int n) 
    {
        int m = rolls.size(), total = accumulate(rolls.begin(), rolls.end(), 0), s = mean * (m + n) - total;
        vector<int> result;
        if (s < n or s > 6 * n) return result;
        result.resize(n);
        for (int i = 0; i < s % n; i++) result[i] = s / n + 1;
        for (int i = s % n; i < n; i++) result[i] = s / n;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] missingRolls(int[] rolls, int mean, int n) {
        int m = rolls.length, total = Arrays.stream(rolls).sum(), s = mean * (m + n) - total, result[] = new int[n];
        if (s < n || s > 6 * n) return new int[0];
        for (int i = 0; i < s % n; i++) result[i] = s / n + 1;
        for (int i = s % n; i < n; i++) result[i] = s / n;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def missingRolls(self, rolls: List[int], mean: int, n: int) -> List[int]:
        return [] if (s := mean * (n + len(rolls)) - sum(rolls)) < n or s > 6 * n else [s // n + 1] * (s % n) + [s // n] * (n - s % n)
```
