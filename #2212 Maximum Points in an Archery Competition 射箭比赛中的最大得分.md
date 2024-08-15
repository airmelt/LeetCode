# 2212 Maximum Points in an Archery Competition 射箭比赛中的最大得分

__Description:__

Alice and Bob are opponents in an archery competition. The competition has set the following rules:

For example, if Alice and Bob both shot `2` arrows on the section with score `11`, then Alice takes `11` points. On the other hand, if Alice shot `0` arrows on the section with score `11` and Bob shot `2` arrows on that same section, then Bob takes `11` points.

You are given the integer `numArrows` and an integer array `aliceArrows` of size `12`, which represents the number of arrows Alice shot on each scoring section from `0` to `11`. Now, Bob wants to __maximize__ the total number of points he can obtain.

Return _the array_ `bobArrows` _which represents the number of arrows Bob shot on __each__ scoring section from_ `0` _to_ `11`. The sum of the values in `bobArrows` should equal `numArrows`.

If there are multiple ways for Bob to earn the maximum total points, return any one of them.

__Example:__

Example 1:

![2212-1](https://assets.leetcode.com/uploads/2022/02/24/ex1.jpg)

```text
Input: numArrows = 9, aliceArrows = [1,1,0,1,0,0,2,1,0,1,2,0]
Output: [0,0,0,0,1,1,0,0,1,2,3,1]
Explanation: The table above shows how the competition is scored. 
Bob earns a total point of 4 + 5 + 8 + 9 + 10 + 11 = 47.
It can be shown that Bob cannot obtain a score higher than 47 points.
```

Example 2:

![2212-2](https://assets.leetcode.com/uploads/2022/02/24/ex2new.jpg)

```text
Input: numArrows = 3, aliceArrows = [0,0,1,0,0,0,0,0,0,0,0,2]
Output: [0,0,0,0,0,0,0,0,1,1,1,0]
Explanation: The table above shows how the competition is scored.
Bob earns a total point of 8 + 9 + 10 = 27.
It can be shown that Bob cannot obtain a score higher than 27 points.
```

__Constraints:__

- `1 <= numArrows <= 10 ^ 5`
- `aliceArrows.length == bobArrows.length == 12`
- `0 <= aliceArrows[i], bobArrows[i] <= numArrows`
- `sum(aliceArrows[i]) == numArrows`

__题目描述:__

Alice 和 Bob 是一场射箭比赛中的对手。比赛规则如下：

例如，Alice 和 Bob 都向计分为 `11` 的区域射 `2` 支箭，那么 Alice 得 `11` 分。如果 Alice 向计分为 `11` 的区域射 `0` 支箭，但 Bob 向同一个区域射 `2` 支箭，那么 Bob 得 `11` 分。

给你整数 `numArrows` 和一个长度为 `12` 的整数数组 `aliceArrows` ，该数组表示 Alice 射中 `0` 到 `11` 每个计分区域的箭数量。现在，Bob 想要尽可能 __最大化__ 他所能获得的总分。

返回数组 `bobArrows` ，该数组表示 Bob 射中 `0` 到 `11` __每个__ 计分区域的箭数量。且 `bobArrows` 的总和应当等于 `numArrows` 。

如果存在多种方法都可以使 Bob 获得最大总分，返回其中 任意一种 即可。

__示例:__

示例 1：

![2212-3](https://pic.leetcode-cn.com/1647744752-kQKrXw-image.png)

```text
输入：numArrows = 9, aliceArrows = [1,1,0,1,0,0,2,1,0,1,2,0]
输出：[0,0,0,0,1,1,0,0,1,2,3,1]
解释：上表显示了比赛得分情况。
Bob 获得总分 4 + 5 + 8 + 9 + 10 + 11 = 47 。
可以证明 Bob 无法获得比 47 更高的分数。
```

示例 2：

![2212-4](https://pic.leetcode-cn.com/1647744785-cMHzaC-image.png)

```text
输入：numArrows = 3, aliceArrows = [0,0,1,0,0,0,0,0,0,0,0,2]
输出：[0,0,0,0,0,0,0,0,1,1,1,0]
解释：上表显示了比赛得分情况。
Bob 获得总分 8 + 9 + 10 = 27 。
可以证明 Bob 无法获得比 27 更高的分数。
```

__提示：__

- `1 <= numArrows <= 10 ^ 5`
- `aliceArrows.length == bobArrows.length == 12`
- `0 <= aliceArrows[i], bobArrows[i] <= numArrows`
- `sum(aliceArrows[i]) == numArrows`

__思路:__

```text
状态压缩
用一个长为 12 的二进制数表示每个状态, 1 表示该状态被选中, 0 表示未被选中
遍历所有的状态, 计算每个状态的得分, 并更新最大得分和状态
时间复杂度为 O(2 ^ N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> maximumBobPoints(int numArrows, vector<int>& aliceArrows) 
    {
        int n = aliceArrows.size(), total = 1 << n, max_score = 0, state = 0;
        vector<int> result(n);
        for (int mask = 0; mask < total; mask++) 
        {
            int cur = 0, score = 0;
            for (int i = 0; i < n; i++) 
            {
                if ((mask >> i) & 1) 
                {
                    cur += aliceArrows[i] + 1;
                    score += i;
                }
            }
            if (cur <= numArrows and score > max_score) 
            {
                max_score = score;
                state = mask;
            }
        }
        for (int i = 0; i < n; i++) 
        {
            if ((state >> i) & 1) 
            {
                result[i] += aliceArrows[i] + 1;
                numArrows -= result[i];
            }
        }
        result.front() += numArrows;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] maximumBobPoints(int numArrows, int[] aliceArrows) {
        int n = aliceArrows.length, total = 1 << n, maxScore = 0, state = 0, result[] = new int[n];
        for (int mask = 0; mask < total; mask++) {
            int cur = 0, score = 0;
            for (int i = 0; i < n; i++) {
                if (((mask >> i) & 1) == 1) {
                    cur += aliceArrows[i] + 1;
                    score += i;
                }
            }
            if (cur <= numArrows && score > maxScore) {
                maxScore = score;
                state = mask;
            }
        }
        for (int i = 0; i < n; i++) {
            if (((state >> i) & 1) == 1) {
                result[i] += aliceArrows[i] + 1;
                numArrows -= result[i];
            }
        }
        result[0] += numArrows;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumBobPoints(self, numArrows: int, aliceArrows: List[int]) -> List[int]:
        total, max_score, state, result = 1 << (n := len(aliceArrows)), 0, 0, [0] * n
        for mask in range(total):
            cur = score = 0
            for i in range(n):
                if (mask >> i) & 1:
                    cur += aliceArrows[i] + 1
                    score += i
            if cur <= numArrows and score > max_score:
                max_score, state = score, mask
        for i in range(n):
            if (state >> i) & 1:
                result[i] = aliceArrows[i] + 1
                numArrows -= result[i]
        result[0] += numArrows
        return result
```
