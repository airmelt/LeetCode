# 2178 Maximum Split of Positive Even Integers 拆分成最多数目的正偶数之和

__Description:__

You are given an integer `finalSum`. Split it into a sum of a __maximum__ number of __unique__ positive even integers.

- For example, given `finalSum = 12`, the following splits are __valid__ (unique positive even integers summing up to `finalSum`): `(12)`, `(2 + 10)`, `(2 + 4 + 6)`, and `(4 + 8)`. Among them, `(2 + 4 + 6)` contains the maximum number of integers. Note that `finalSum` cannot be split into `(2 + 2 + 4 + 4)` as all the numbers should be unique.

Return _a list of integers that represent a valid split containing a __maximum__ number of integers_. If no valid split exists for `finalSum`, return _an __empty__ list_. You may return the integers in __any__ order.

__Example:__

Example 1:

```text
Input:  finalSum = 12
Output:  [2,4,6]
```

Explanation:  The following are valid splits: `(12)`, `(2 + 10)`, `(2 + 4 + 6)`, and `(4 + 8)`.
(2 + 4 + 6) has the maximum number of integers, which is 3. Thus, we return [2,4,6].
Note that [2,6,4], [6,2,4], etc. are also accepted.

Example 2:

```text
Input: finalSum = 7
Output: []
Explanation: There are no valid splits for the given finalSum.
Thus, we return an empty array.
```

Example 3:

```text
Input:  finalSum = 28
Output:  [6,8,2,12]
```

Explanation:  The following are valid splits: `(2 + 26)`, `(6 + 8 + 2 + 12)`, and `(4 + 24)`.
 `(6 + 8 + 2 + 12)` has the maximum number of integers, which is 4. Thus, we return [6,8,2,12].
Note that [10,2,4,12], [6,2,4,16], etc. are also accepted.

__Constraints:__

- `1 <= finalSum <= 10 ^ 10`

__题目描述:__

给你一个整数 `finalSum` 。请你将它拆分成若干个 __互不相同__ 的正偶数之和，且拆分出来的正偶数数目 __最多__ 。

- 比方说，给你 `finalSum = 12` ，那么这些拆分是 __符合要求__ 的（互不相同的正偶数且和为 `finalSum`）: `(2 + 10)` ， `(2 + 4 + 6)` 和 `(4 + 8)` 。它们中， `(2 + 4 + 6)` 包含最多数目的整数。注意 `finalSum` 不能拆分成 `(2 + 2 + 4 + 4)` ，因为拆分出来的整数必须互不相同。

请你返回一个整数数组，表示将整数拆分成 __最多__ 数目的正偶数数组。如果没有办法将 `finalSum` 进行拆分，请你返回一个 __空__ 数组。你可以按 _任意_ 顺序返回这些整数。

__示例:__

示例 1：

```text
输入: finalSum = 12
输出: [2,4,6]
```

解释: 以下是一些符合要求的拆分: `(2 + 10)<span>，</span>` `(2 + 4 + 6)` 和 `(4 + 8) 。`
(2 + 4 + 6) 为最多数目的整数，数目为 3 ，所以我们返回 [2,4,6] 。
[2,6,4] ，[6,2,4] 等等也都是可行的解。

示例 2：

```text
输入：finalSum = 7
输出：[]
解释：没有办法将 finalSum 进行拆分。
所以返回空数组。
```

示例 3：

```text
输入: finalSum = 28
输出: [6,8,2,12]
```

解释: 以下是一些符合要求的拆分: `(2 + 26)<span>，</span>` `(6 + 8 + 2 + 12)` 和 `(4 + 24) 。`
 `(6 + 8 + 2 + 12)` 有最多数目的整数，数目为 4 ，所以我们返回 [6,8,2,12] 。
[10,2,4,12] ，[6,2,4,16] 等等也都是可行的解。

__提示：__

- `1 <= finalSum <= 10 ^ 10`

__思路:__

```text
贪心
检查 finalSum 是否为奇数
如果是, 返回空列表
从 i = 2 开始, 逐步增加 2, 直到 finalSum 不大于 i
将所有 i 加入结果列表中
最后如果 finalSum 不为 0, 将最后一个数加上 finalSum
时间复杂度为 O(N ^ 1 / 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<long long> maximumEvenSplit(long long finalSum) 
    {
        vector<long long> result;
        if ((finalSum & 1LL) == 1LL) return result;
        for (long long i = 2LL; i <= finalSum; finalSum -= i, i += 2LL) result.emplace_back(i);
        result.back() += finalSum;
        return result; 
    }
};
```

__Java__:

```Java
class Solution {
    public List<Long> maximumEvenSplit(long finalSum) {
        List<Long> result = new ArrayList<>();
        if ((finalSum & 1L) == 1L) return result;
        for (long i = 2L; i <= finalSum; finalSum -= i, i += 2L) result.add(i);
        result.set(result.size() - 1, result.get(result.size() - 1) + finalSum);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumEvenSplit(self, finalSum: int) -> List[int]:
        result, i = [], 2
        if finalSum & 1:
            return []
        while i <= finalSum:
            result.append(i)
            finalSum -= i
            i += 2
        return result[:-1] + [result[-1] + finalSum]
```
