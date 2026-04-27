# 1716 Calculate Money in Leetcode Bank 计算力扣银行的钱

__Description:__

Hercy wants to save money for his first car. He puts money in the Leetcode bank every day.

He starts by putting in `$1` on Monday, the first day. Every day from Tuesday to Sunday, he will put in `$1` more than the day before. On every subsequent Monday, he will put in `$1` more than the __previous Monday__.

Given `n`, return _the total amount of money he will have in the Leetcode bank at the end of the_ `n ^ th` _day._

__Example:__

Example 1:

```text
Input: n = 4
Output: 10
Explanation: After the 4th day, the total is 1 + 2 + 3 + 4 = 10.
```

Example 2:

```text
Input: n = 10
Output: 37
Explanation: After the 10th day, the total is (1 + 2 + 3 + 4 + 5 + 6 + 7) + (2 + 3 + 4) = 37. Notice that on the 2nd Monday, Hercy only puts in $2.
```

Example 3:

```text
Input: n = 20
Output: 96
Explanation: After the 20th day, the total is (1 + 2 + 3 + 4 + 5 + 6 + 7) + (2 + 3 + 4 + 5 + 6 + 7 + 8) + (3 + 4 + 5 + 6 + 7 + 8) = 96.
```

__Constraints:__

- `1 <= n <= 1000`

__题目描述:__

Hercy 想要为购买第一辆车存钱。他 每天 都往力扣银行里存钱。

最开始，他在周一的时候存入 `1` 块钱。从周二到周日，他每天都比前一天多存入 `1` 块钱。在接下来每一个周一，他都会比 __前一个周一__ 多存入 `1` 块钱。

给你 `n` ，请你返回在第 `n` 天结束的时候他在力扣银行总共存了多少块钱。

__示例:__

示例 1：

```text
输入：n = 4
输出：10
解释：第 4 天后，总额为 1 + 2 + 3 + 4 = 10 。
```

示例 2：

```text
输入：n = 10
输出：37
解释：第 10 天后，总额为 (1 + 2 + 3 + 4 + 5 + 6 + 7) + (2 + 3 + 4) = 37 。注意到第二个星期一，Hercy 存入 2 块钱。
```

示例 3：

```text
输入：n = 20
输出：96
解释：第 20 天后，总额为 (1 + 2 + 3 + 4 + 5 + 6 + 7) + (2 + 3 + 4 + 5 + 6 + 7 + 8) + (3 + 4 + 5 + 6 + 7 + 8) = 96 。
```

__提示：__

- `1 <= n <= 1000`

__思路:__

```text
数学
先计算需要多少周
再用取模计算剩余天数
两个都需要用等差数列求和公式计算总金额
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int totalMoney(int n) 
    {
        return (n / 7) * (49 + n / 7 * 7) + (n % 7) * (1 + (n / 7 << 1) + n % 7) >> 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int totalMoney(int n) {
        return (n / 7) * (49 + n / 7 * 7) + (n % 7) * (1 + (n / 7 << 1) + n % 7) >>> 1;
    }
}
```

__Python__:

```Python
class Solution:
    def totalMoney(self, n: int) -> int:
        return (weeks := n // 7) * (49 + 7 * weeks) + (days := n % 7) * (1 + (weeks << 1) + days) >> 1
```
