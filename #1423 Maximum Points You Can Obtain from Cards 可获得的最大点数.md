# 1423 Maximum Points You Can Obtain from Cards 可获得的最大点数

__Description:__

There are several cards __arranged in a row__, and each card has an associated number of points. The points are given in the integer array `cardPoints`.

In one step, you can take one card from the beginning or from the end of the row. You have to take exactly `k` cards.

Your score is the sum of the points of the cards you have taken.

Given the integer array `cardPoints` and the integer `k`, return the _maximum score_ you can obtain.

__Example:__

Example 1:

```text
Input: cardPoints = [1,2,3,4,5,6,1], k = 3
Output: 12
Explanation: After the first step, your score will always be 1. However, choosing the rightmost card first will maximize your total score. The optimal strategy is to take the three cards on the right, giving a final score of 1 + 6 + 5 = 12.
```

Example 2:

```text
Input: cardPoints = [2,2,2], k = 2
Output: 4
Explanation: Regardless of which two cards you take, your score will always be 4.
```

Example 3:

```text
Input: cardPoints = [9,7,7,9,7,7,9], k = 7
Output: 55
Explanation: You have to take all the cards. Your score is the sum of points of all cards.
```

__Constraints:__

- `1 <= cardPoints.length <= 10 ^ 5`
- `1 <= cardPoints[i] <= 10 ^ 4`
- `1 <= k <= cardPoints.length`

__题目描述:__

几张卡牌 __排成一行__，每张卡牌都有一个对应的点数。点数由整数数组 `cardPoints` 给出。

每次行动，你可以从行的开头或者末尾拿一张卡牌，最终你必须正好拿 `k` 张卡牌。

你的点数就是你拿到手中的所有卡牌的点数之和。

给你一个整数数组 `cardPoints` 和整数 `k`，请你返回可以获得的最大点数。

__示例:__

示例 1：

```text
输入：cardPoints = [1,2,3,4,5,6,1], k = 3
输出：12
解释：第一次行动，不管拿哪张牌，你的点数总是 1 。但是，先拿最右边的卡牌将会最大化你的可获得点数。最优策略是拿右边的三张牌，最终点数为 1 + 6 + 5 = 12 。
```

示例 2：

```text
输入：cardPoints = [2,2,2], k = 2
输出：4
解释：无论你拿起哪两张卡牌，可获得的点数总是 4 。
```

示例 3：

```text
输入：cardPoints = [9,7,7,9,7,7,9], k = 7
输出：55
解释：你必须拿起所有卡牌，可以获得的点数为所有卡牌的点数之和。
```

示例 4：

```text
输入：cardPoints = [1,1000,1], k = 1
输出：1
解释：你无法拿到中间那张卡牌，所以可以获得的最大点数为 1 。
```

示例 5：

```text
输入：cardPoints = [1,79,80,1,1,1,200,1], k = 3
输出：202
```

__提示：__

- `1 <= cardPoints.length <= 10 ^ 5`
- `1 <= cardPoints[i] <= 10 ^ 4`
- `1 <= k <= cardPoints.length`

__思路:__

```text
滑动窗口
维护一个大小为 k 的窗口
初始值为这个窗口里的所有数的和
然后向数组的尾部滑动
即移除窗口中的最后一个数, 添加数组倒数第 i 个数
并更新最大和第值
时间复杂度为 O(K), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxScore(vector<int>& cardPoints, int k) 
    {
        int sum = accumulate(cardPoints.begin(), cardPoints.begin() + k, 0), result = sum, n = cardPoints.size();
        for (int i = 0; i < k; i++)
        {
            sum += cardPoints[n - 1 - i] - cardPoints[k - 1 - i];
            result = sum > result ? sum : result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxScore(int[] cardPoints, int k) {
        int s = 0, result = 0, n = cardPoints.length;
        for (int i = 0; i < k; i++) s += cardPoints[i];
        for (int i = 0; i < k; i++) {
            result = Math.max(s, result);
            s += cardPoints[n - i - 1] - cardPoints[k - i - 1];
        }
        return Math.max(s, result);
    }
}
```

__Python__:

```Python
class Solution:
    def maxScore(self, cardPoints: List[int], k: int) -> int:
        return sum(cardPoints) - min(c[i + len(cardPoints) - k] - c[i] for i in range(k + 1)) if (c := [0] + list(accumulate(cardPoints))) else -1
```
