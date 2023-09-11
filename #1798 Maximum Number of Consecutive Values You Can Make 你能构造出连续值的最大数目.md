# 1798 Maximum Number of Consecutive Values You Can Make 你能构造出连续值的最大数目

__Description:__

You are given an integer array `coins` of length `n` which represents the `n` coins that you own. The value of the `i ^ th` coin is `coins[i]`. You can __make__ some value `x` if you can choose some of your `n` coins such that their values sum up to `x`.

Return the _maximum number of consecutive integer values that you __can__ __make__ with your coins __starting__ from and __including___ `0`.

Note that you may have multiple coins of the same value.

__Example:__

Example 1:

```text
Input: coins = [1,3]
Output: 2
Explanation: You can make the following values:
- 0: take []
- 1: take [1]
You can make 2 consecutive integer values starting from 0.
```

Example 2:

```text
Input: coins = [1,1,1,4]
Output: 8
Explanation: You can make the following values:
- 0: take []
- 1: take [1]
- 2: take [1,1]
- 3: take [1,1,1]
- 4: take [4]
- 5: take [4,1]
- 6: take [4,1,1]
- 7: take [4,1,1,1]
You can make 8 consecutive integer values starting from 0.
```

Example 3:

```text
Input: nums = [1,4,10,3,1]
Output: 20
```

__Constraints:__

- `coins.length == n`
- `1 <= n <= 4 * 10 ^ 4`
- `1 <= coins[i] <= 4 * 10 ^ 4`

__题目描述:__

给你一个长度为 `n` 的整数数组 `coins` ，它代表你拥有的 `n` 个硬币。第 `i` 个硬币的值为 `coins[i]` 。如果你从这些硬币中选出一部分硬币，它们的和为 `x` ，那么称，你可以 __构造__ 出 `x` 。

请返回从 `0` 开始（__包括__ `0` ），你最多能 __构造__ 出多少个连续整数。

你可能有多个相同值的硬币。

__示例:__

示例 1：

```text
输入：coins = [1,3]
输出：2
解释：你可以得到以下这些值：
- 0：什么都不取 []
- 1：取 [1]
从 0 开始，你可以构造出 2 个连续整数。
```

示例 2：

```text
输入：coins = [1,1,1,4]
输出：8
解释：你可以得到以下这些值：
- 0：什么都不取 []
- 1：取 [1]
- 2：取 [1,1]
- 3：取 [1,1,1]
- 4：取 [4]
- 5：取 [4,1]
- 6：取 [4,1,1]
- 7：取 [4,1,1,1]
从 0 开始，你可以构造出 8 个连续整数。
```

示例 3：

```text
输入：nums = [1,4,10,3,1]
输出：20
```

__提示：__

- `coins.length == n`
- `1 <= n <= 4 * 10 ^ 4`
- `1 <= coins[i] <= 4 * 10 ^ 4`

__思路:__

```text
贪心
数组的排列顺序不影响结果
先将数组排序
从第一个数开始取, 如果连续, 说明之前的连续数字都可以取到, 结果加上现在都数字
否则, 返回结果
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int getMaximumConsecutive(vector<int>& coins) 
    {
        sort(coins.begin(), coins.end());
        int result = 0;
        for (const auto& c : coins) 
        {
            if (c - result > 1) break;
            result += c;
        }
        return result + 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int getMaximumConsecutive(int[] coins) {
        Arrays.sort(coins);
        int result = 0;
        for (int c : coins) {
            if (c - result > 1) break;
            result += c;
        }
        return result + 1;
    }
}
```

__Python__:

```Python
class Solution:
    def getMaximumConsecutive(self, coins: List[int]) -> int:
        coins.sort()
        result = 0
        for c in coins:
            if c - result > 1:
                break
            result += c
        return result + 1
```
