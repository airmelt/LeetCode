# 3185 Count Pairs That Form a Complete Day II 构成整天的下标对数目 II

__Description:__

Given an integer array `hours` representing times in __hours__, return an integer denoting the number of pairs `i`, `j` where `i < j` and `hours[i] + hours[j]` forms a __complete day__.

A __complete day__ is defined as a time duration that is an __exact multiple__ of 24 hours.

For example, 1 day is 24 hours, 2 days is 48 hours, 3 days is 72 hours, and so on.

__Example:__

Example 1:

```text
Input: hours = [12,12,30,24,24]

Output: 2

Explanation: The pairs of indices that form a complete day are (0, 1) and (3, 4).
```

Example 2:

```text
Input: hours = [72,48,24,3]

Output: 3

Explanation: The pairs of indices that form a complete day are (0, 1), (0, 2), and (1, 2).
```

__Constraints:__

- `1 <= hours.length <= 5 * 10 ^ 5`
- `1 <= hours[i] <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `hours`，表示以 __小时__ 为单位的时间，返回一个整数，表示满足 `i < j` 且 `hours[i] + hours[j]` 构成 __整天__ 的下标对 `i`, `j` 的数目。

__整天__ 定义为时间持续时间是 24 小时的 __整数倍__ 。

例如，1 天是 24 小时，2 天是 48 小时，3 天是 72 小时，以此类推。

__示例:__

示例 1：

```text
输入： hours = [12,12,30,24,24]

输出： 2

解释：

构成整天的下标对分别是 (0, 1) 和 (3, 4)。
```

示例 2：

```text
输入： hours = [72,48,24,3]

输出： 3

解释：

构成整天的下标对分别是 (0, 1)、(0, 2) 和 (1, 2)。
```

__提示：__

- `1 <= hours.length <= 5 * 10 ^ 5`
- `1 <= hours[i] <= 10 ^ 9`

__思路:__

```text
数学
按照对 24 的余数分组统计
统计之前先将 24 - 当前 hour 对 24 的余数累加到结果
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countCompleteDayPairs(vector<int>& hours) 
    {
        long long result = 0LL, c[24]{ 0LL };
        for (const auto& hour : hours) 
        {
            result += c[(24 - hour % 24) % 24];
            ++c[hour % 24];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long countCompleteDayPairs(int[] hours) {
        long result = 0, c[] = new long[24];
        for (int hour : hours) {
            result += c[(24 - hour % 24) % 24];
            ++c[hour % 24];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countCompleteDayPairs(self, hours: List[int]) -> int:
        d, result = defaultdict(int), 0
        for i, hour in enumerate(hours):
            result += d[(24 - (hour % 24)) % 24]
            d[hour % 24] += 1
        return result
```
