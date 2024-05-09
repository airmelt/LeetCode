# 2100 Find Good Days to Rob the Bank 适合野炊的日子

__Description:__

You and a gang of thieves are planning on robbing a bank. You are given a __0-indexed__ integer array `security`, where `security[i]` is the number of guards on duty on the `i ^ th` day. The days are numbered starting from `0`. You are also given an integer `time`.

The `i ^ th` day is a good day to rob the bank if:

- There are at least `time` days before and after the `i ^ th` day,
- The number of guards at the bank for the `time` days __before__ `i` are __non-increasing__, and
- The number of guards at the bank for the `time` days __after__ `i` are __non-decreasing__.

More formally, this means day `i` is a good day to rob the bank if and only if `security[i - time] >= security[i - time + 1] >= ... >= security[i] <= ... <= security[i + time - 1] <= security[i + time]`.

Return a list of all days (0-indexed) that are good days to rob the bank. The order that the days are returned in does not matter.

__Example:__

Example 1:

```text
Input: security = [5,3,3,3,5,6,2], time = 2
Output: [2,3]
Explanation:
On day 2, we have security[0] >= security[1] >= security[2] <= security[3] <= security[4].
On day 3, we have security[1] >= security[2] >= security[3] <= security[4] <= security[5].
No other days satisfy this condition, so days 2 and 3 are the only good days to rob the bank.
```

Example 2:

```text
Input: security = [1,1,1,1,1], time = 0
Output: [0,1,2,3,4]
Explanation:
Since time equals 0, every day is a good day to rob the bank, so return every day.
```

Example 3:

```text
Input: security = [1,2,3,4,5,6], time = 2
Output: []
Explanation:
No day has 2 days before it that have a non-increasing number of guards.
Thus, no day is a good day to rob the bank, so return an empty list.
```

__Constraints:__

- `1 <= security.length <= 10 ^ 5`
- `0 <= security[i], time <= 10 ^ 5`

__题目描述:__

你和朋友们准备去野炊。给你一个下标从 __0__ 开始的整数数组 `security` ，其中 `security[i]` 是第 `i` 天的建议出行指数。日子从 `0` 开始编号。同时给你一个整数 `time` 。

如果第 `i` 天满足以下所有条件，我们称它为一个适合野炊的日子:

- 第 `i` 天前和后都分别至少有 `time` 天。
- 第 `i` 天前连续 `time` 天建议出行指数都是非递增的。
- 第 `i` 天后连续 `time` 天建议出行指数都是非递减的。

更正式的，第 `i` 天是一个适合野炊的日子当且仅当: `security[i - time] >= security[i - time + 1] >= ... >= security[i] <= ... <= security[i + time - 1] <= security[i + time]`.

请你返回一个数组，包含 所有 适合野炊的日子（下标从 0 开始）。返回的日子可以 任意 顺序排列。

__示例:__

示例 1：

```text
输入：security = [5,3,3,3,5,6,2], time = 2
输出：[2,3]
解释：
第 2 天，我们有 security[0] >= security[1] >= security[2] <= security[3] <= security[4] 。
第 3 天，我们有 security[1] >= security[2] >= security[3] <= security[4] <= security[5] 。
没有其他日子符合这个条件，所以日子 2 和 3 是适合野炊的日子。
```

示例 2：

```text
输入：security = [1,1,1,1,1], time = 0
输出：[0,1,2,3,4]
解释：
因为 time 等于 0 ，所以每一天都是适合野炊的日子，所以返回每一天。
```

示例 3：

```text
输入：security = [1,2,3,4,5,6], time = 2
输出：[]
解释：
没有任何一天的前 2 天建议出行指数是非递增的。
所以没有适合野炊的日子，返回空数组。
```

__提示：__

- `1 <= security.length <= 10 ^ 5`
- `0 <= security[i], time <= 10 ^ 5`

__思路:__

```text
前缀和
left 数组记录 i 前累计比 i 小的连续的个数
right 数组记录 i 后累计比 i 小的连续的个数
最后从 [time, n - time) 区间内找到 left[i] >= time and right[i] >= time 的 i 即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> goodDaysToRobBank(vector<int>& security, int time) 
    {
        int n = security.size();
        vector<int> left(n), right(n), result;
        for (int i = 1; i < n; i++) 
        {
            if (security[i] <= security[i - 1]) left[i] = left[i - 1] + 1;
            if (security[n - i - 1] <= security[n - i]) right[n - i - 1] = right[n - i] + 1;
        }
        for (int i = time; i < n - time; i++) if (left[i] >= time and right[i] >= time) result.emplace_back(i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> goodDaysToRobBank(int[] security, int time) {
        int n = security.length, left[] = new int[n], right[] = new int[n];
        for (int i = 1; i < n; i++) {
            if (security[i] <= security[i - 1]) left[i] = left[i - 1] + 1;
            if (security[n - i - 1] <= security[n - i]) right[n - i - 1] = right[n - i] + 1;
        }
        List<Integer> result = new ArrayList<>();
        for (int i = time; i < n - time; i++) if (left[i] >= time && right[i] >= time) result.add(i);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def goodDaysToRobBank(self, security: List[int], time: int) -> List[int]:
        left, right = [0] * (n := len(security)), [0] * n
        for i in range(1, n):
            if security[i] <= security[i - 1]:
                left[i] = left[i - 1] + 1
            if security[n - i - 1] <= security[n - i]:
                right[n - i - 1] = right[n - i] + 1
        return [i for i in range(time, n - time) if left[i] >= time and right[i] >= time]
```
