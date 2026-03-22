# 2554 Maximum Number of Integers to Choose From a Range I 从一个范围内选择最多整数 I

__Description:__

You are given an integer array `banned` and two integers `n` and `maxSum`. You are choosing some number of integers following the below rules:

- The chosen integers have to be in the range `[1, n]`.
- Each integer can be chosen __at most once__.
- The chosen integers should not be in the array `banned`.
- The sum of the chosen integers should not exceed `maxSum`.

Return the maximum number of integers you can choose following the mentioned rules.

__Example:__

Example 1:

```text
Input: banned = [1,6,5], n = 5, maxSum = 6
Output: 2
Explanation: You can choose the integers 2 and 4.
2 and 4 are from the range [1, 5], both did not appear in banned, and their sum is 6, which did not exceed maxSum.
```

Example 2:

```text
Input: banned = [1,2,3,4,5,6,7], n = 8, maxSum = 1
Output: 0
Explanation: You cannot choose any integer while following the mentioned conditions.
```

Example 3:

```text
Input: banned = [11], n = 7, maxSum = 50
Output: 7
Explanation: You can choose the integers 1, 2, 3, 4, 5, 6, and 7.
They are from the range [1, 7], all did not appear in banned, and their sum is 28, which did not exceed maxSum.
```

__Constraints:__

- `1 <= banned.length <= 10 ^ 4`
- `1 <= banned[i], n <= 10 ^ 4`
- `1 <= maxSum <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `banned` 和两个整数 `n` 和 `maxSum` 。你需要按照以下规则选择一些整数:

- 被选择整数的范围是 `[1, n]` 。
- 每个整数 __至多__ 选择 __一次__ 。
- 被选择整数不能在数组 `banned` 中。
- 被选择整数的和不超过 `maxSum` 。

请你返回按照上述规则 最多 可以选择的整数数目。

__示例:__

示例 1：

```text
输入：banned = [1,6,5], n = 5, maxSum = 6
输出：2
解释：你可以选择整数 2 和 4 。
2 和 4 在范围 [1, 5] 内，且它们都不在 banned 中，它们的和是 6 ，没有超过 maxSum 。
```

示例 2：

```text
输入：banned = [1,2,3,4,5,6,7], n = 8, maxSum = 1
输出：0
解释：按照上述规则无法选择任何整数。
```

示例 3：

```text
输入：banned = [11], n = 7, maxSum = 50
输出：7
解释：你可以选择整数 1, 2, 3, 4, 5, 6 和 7 。
它们都在范围 [1, 7] 中，且都没出现在 banned 中，它们的和是 28 ，没有超过 maxSum 。
```

__提示：__

- `1 <= banned.length <= 10 ^ 4`
- `1 <= banned[i], n <= 10 ^ 4`
- `1 <= maxSum <= 10 ^ 9`

__思路:__

```text
贪心
从小到大依次加入数字
直到无法加入为止
遇到 banned 中的数字则跳过
时间复杂度为 O(M + N), 空间复杂度为 O(M), 其中 M 为 banned 数组的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxCount(vector<int>& banned, int n, int maxSum) 
    {
        int result = 0, flag[n + 1];
        memset(flag, 0, sizeof flag);
        for (int x : banned) if (x <= n) flag[x] = 1;
        for (int i = 1; i <= n and i <= maxSum; i++) 
        {
            if (!flag[i]) 
            {
                ++result;
                maxSum -= i;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxCount(int[] banned, int n, int maxSum) {
        var flag = new boolean[n + 1];
        for (int x : banned) if (x <= n) flag[x] = true;
        int result = 0;
        for (int i = 1; i <= n && i <= maxSum; i++) {
            if (!flag[i]) {
                ++result;
                maxSum -= i;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxCount(self, banned: List[int], n: int, maxSum: int) -> int:
        s, result = set(banned), 0
        for i in range(1, n + 1):
            if maxSum < i:
                break
            if i not in s:
                result += 1
                maxSum -= i
        return result
```
