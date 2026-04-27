# 1449 Form Largest Integer With Digits That Add up to Target 数位成本和为目标值的最大数字

__Description:__

Given an array of integers `cost` and an integer `target`, return _the __maximum__ integer you can paint under the following rules_:

- The cost of painting a digit `(i + 1)` is given by `cost[i]` (__0-indexed__).
- The total cost used must be equal to `target`.
- The integer does not have `0` digits.

Since the answer may be very large, return it as a string. If there is no way to paint any integer given the condition, return `"0"`.

__Example:__

Example 1:

```text
Input: cost = [4,3,2,5,6,7,2,5,5], target = 9
Output: "7772"
Explanation: The cost to paint the digit '7' is 2, and the digit '2' is 3. Then cost("7772") = 2*3+ 3*1 = 9. You could also paint "977", but "7772" is the largest number.
Digit    cost
  1  ->   4
  2  ->   3
  3  ->   2
  4  ->   5
  5  ->   6
  6  ->   7
  7  ->   2
  8  ->   5
  9  ->   5
```

Example 2:

```text
Input: cost = [7,6,5,5,5,6,8,7,8], target = 12
Output: "85"
Explanation: The cost to paint the digit '8' is 7, and the digit '5' is 5. Then cost("85") = 7 + 5 = 12.
```

Example 3:

```text
Input: cost = [2,4,6,2,4,6,4,4,4], target = 5
Output: "0"
Explanation: It is impossible to paint any integer with total cost equal to target.
```

__Constraints:__

- `cost.length == 9`
- `1 <= cost[i], target <= 5000`

__题目描述:__

给你一个整数数组 `cost` 和一个整数 `target` 。请你返回满足如下规则可以得到的 __最大__ 整数：

- 给当前结果添加一个数位（`i + 1`）的成本为 `cost[i]` （`cost` 数组下标从 0 开始）。
- 总成本必须恰好等于 `target` 。
- 添加的数位中没有数字 0 。

由于答案可能会很大，请你以字符串形式返回。

如果按照上述要求无法得到任何整数，请你返回 "0" 。

__示例:__

示例 1：

```text
输入：cost = [4,3,2,5,6,7,2,5,5], target = 9
输出："7772"
解释：添加数位 '7' 的成本为 2 ，添加数位 '2' 的成本为 3 。所以 "7772" 的代价为 2*3+ 3*1 = 9 。 "977" 也是满足要求的数字，但 "7772" 是较大的数字。
 数字     成本
  1  ->   4
  2  ->   3
  3  ->   2
  4  ->   5
  5  ->   6
  6  ->   7
  7  ->   2
  8  ->   5
  9  ->   5
```

示例 2：

```text
输入：cost = [7,6,5,5,5,6,8,7,8], target = 12
输出："85"
解释：添加数位 '8' 的成本是 7 ，添加数位 '5' 的成本是 5 。"85" 的成本为 7 + 5 = 12 。
```

示例 3：

```text
输入：cost = [2,4,6,2,4,6,4,4,4], target = 5
输出："0"
解释：总成本是 target 的条件下，无法生成任何整数。
```

示例 4：

```text
输入：cost = [6,10,15,40,40,40,40,40,40], target = 47
输出："32211"
```

__提示：__

- `cost.length == 9`
- `1 <= cost[i] <= 5000`
- `1 <= target <= 5000`

__思路:__

```text
动态规划 ➕ 贪心
1. 设 dp[i + 1][j] 表示使用前 i 个数位且总成本恰好为 j 的结果的最大位数
由于如果最后结果不恰好为 j, 返回 "0" 特殊值, 所以所有不恰好为 j 的都初始化为一个取不到的负值
初始化 dp[0][0] = 0, dp[0][j] = -float('inf'), j > 0
转移方程 dp[i + 1][j] = dp[i][j], 如果 j < cost[i]
dp[i + 1][j] = max(dp[i][j], dp[i + 1][j - cost[i]] + 1), 如果 j >= cost[i], 分别对应选择一个新的数或者不选两种情况
最后 dp[9][target] 即为位数最大的整数
根据 dp[i + 1][j] 和 dp[i + 1][j - cost[d]] 判断是否进行了转移即可还原出最大整数
注意到 dp[i + 1] 只与 dp[i] 有关, 可以用滚动数组优化空间复杂度 

2. 如果 dp[j - cost[d]] 已经存在或者 len(dp[j - cost[d]]) + 1 >= len(dp[j]), 就可以转移到 dp[j]
这是因为后面使用的数位 d 一定大于之前的所以如果后面的数位能进行转移, 只要其长度 + 1 不小于当前的数位即可
时间复杂度为 O(NM), 空间复杂度为 O(N), M 即 target
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string largestNumber(vector<int>& cost, int target) 
    {
        vector<string> dp(target + 1, "0");
        dp.front() = "";
        for (int d = 0; d < 9; d++) for (int i = cost[d]; i <= target; i++) if (dp[i - cost[d]] != "0" and dp[i - cost[d]].size() + 1 >= dp[i].size()) dp[i] = to_string(d + 1) + dp[i - cost[d]];
        return dp[target];
    }
};
```

__Java__:

```Java
class Solution {
    public String largestNumber(int[] cost, int target) {
        int[] dp = new int[target + 1];
        Arrays.fill(dp, Integer.MIN_VALUE);
        dp[0] = 0;
        for (int c: cost) for (int i = c; i <= target; i++) dp[i] = Math.max(dp[i], dp[i - c] + 1);
        if (dp[target] < 0) return "0";
        StringBuilder sb = new StringBuilder();
        for (int i = 9, j = target; i > 0; i--) for (int c = cost[i - 1]; j >= c && dp[j] == dp[j - c] + 1; j -= c) sb.append(i);
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def largestNumber(self, cost: List[int], target: int) -> str:
        dp = [''] + ['0'] * target
        for d in range(9):
            for i in range(cost[d], target + 1):
                if dp[i - cost[d]] != '0' and len(dp[i - cost[d]]) + 1 >= len(dp[i]):
                    dp[i] = str(d + 1) + dp[i - cost[d]]
        return dp[target]
```
