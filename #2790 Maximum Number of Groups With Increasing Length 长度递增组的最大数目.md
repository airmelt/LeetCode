# 2790 Maximum Number of Groups With Increasing Length 长度递增组的最大数目

__Description:__

You are given a __0-indexed__ array `usageLimits` of length `n`.

Your task is to create __groups__ using numbers from `0` to `n - 1`, ensuring that each number, `i`, is used no more than `usageLimits[i]` times in total __across all groups__. You must also satisfy the following conditions:

- Each group must consist of __distinct__ numbers, meaning that no duplicate numbers are allowed within a single group.
- Each group (except the first one) must have a length __strictly greater__ than the previous group.

Return an integer denoting the maximum number of groups you can create while satisfying these conditions.

__Example:__

Example 1:

```text
Input:  usageLimits = [1,2,5]
Output:  3
Explanation:  In this example, we can use 0 at most once, 1 at most twice, and 2 at most five times.
One way of creating the maximum number of groups while satisfying the conditions is: 
Group 1 contains the number [2].
Group 2 contains the numbers [1,2].
Group 3 contains the numbers [0,1,2]. 
It can be shown that the maximum number of groups is 3. 
So, the output is 3.
```

Example 2:

```text
Input:  usageLimits = [2,1,2]
Output:  2
Explanation:  In this example, we can use 0 at most twice, 1 at most once, and 2 at most twice.
One way of creating the maximum number of groups while satisfying the conditions is:
Group 1 contains the number [0].
Group 2 contains the numbers [1,2].
It can be shown that the maximum number of groups is 2.
So, the output is 2.
```

Example 3:

```text
Input:  usageLimits = [1,1]
Output:  1
Explanation:  In this example, we can use both 0 and 1 at most once.
One way of creating the maximum number of groups while satisfying the conditions is:
Group 1 contains the number [0].
It can be shown that the maximum number of groups is 1.
So, the output is 1.
```

__Constraints:__

- `1 <= usageLimits.length <= 10 ^ 5`
- `1 <= usageLimits[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始、长度为 `n` 的数组 `usageLimits` 。

你的任务是使用从 `0` 到 `n - 1` 的数字创建若干组，并确保每个数字 `i` 在 __所有组__ 中使用的次数总共不超过 `usageLimits[i]` 次。此外，还必须满足以下条件:

- 每个组必须由 __不同__ 的数字组成，也就是说，单个组内不能存在重复的数字。
- 每个组（除了第一个）的长度必须 __严格大于__ 前一个组。

在满足所有条件的情况下，以整数形式返回可以创建的最大组数。

__示例:__

示例 1：

```text
输入: usageLimits = [1,2,5]
输出: 3
解释: 在这个示例中，我们可以使用 0 至多一次，使用 1 至多 2 次，使用 2 至多 5 次。
一种既能满足所有条件，又能创建最多组的方式是: 
组 1 包含数字 [2] 。
组 2 包含数字 [1,2] 。
组 3 包含数字 [0,1,2] 。 
可以证明能够创建的最大组数是 3 。 
所以，输出是 3 。
```

示例 2：

```text
输入: usageLimits = [2,1,2]
输出: 2
解释: 在这个示例中，我们可以使用 0 至多 2 次，使用 1 至多 1 次，使用 2 至多 2 次。
一种既能满足所有条件，又能创建最多组的方式是: 
组 1 包含数字 [0] 。 
组 2 包含数字 [1,2] 。
可以证明能够创建的最大组数是 2 。 
所以，输出是 2 。
```

示例 3：

```text
输入: usageLimits = [1,1]
输出: 1
解释: 在这个示例中，我们可以使用 0 和 1 至多 1 次。 
一种既能满足所有条件，又能创建最多组的方式是:
组 1 包含数字 [0] 。
可以证明能够创建的最大组数是 1 。 
所以，输出是 1 。
```

__提示：__

- `1 <= usageLimits.length <= 10 ^ 5`
- `1 <= usageLimits[i] <= 10 ^ 9`

__思路:__

```text
数学 ➕ 贪心
先将 usageLimits 从小到大排序
对于每一个 usageLimits 中的数字
我们需要分配给前 k 个组
那么每次一共需要 (k + 1) * (k + 2) // 2 个数字
如果能够分配就累加
否则可以提前退出循环
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxIncreasingGroups(vector<int>& usageLimits) 
    {
        sort(usageLimits.begin(), usageLimits.end());
        long long result = 0LL, cur = 0LL;
        for (const auto& v : usageLimits) result += (cur += v) >= ((result + 2LL) * (result + 1LL) >> 1LL);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxIncreasingGroups(List<Integer> usageLimits) {
        Collections.sort(usageLimits);
        long result = 0L, cur = 0L;
        for (int v : usageLimits) result += ((cur += v) >= ((result + 2L) * (result + 1L) >>> 1L) ? 1L : 0L);
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxIncreasingGroups(self, usageLimits: List[int]) -> int:
        usageLimits.sort()
        cur = result = 0
        for v in usageLimits:
            result += (cur := cur + v) >= (result + 2) * (result + 1) >> 1
        return result
```
