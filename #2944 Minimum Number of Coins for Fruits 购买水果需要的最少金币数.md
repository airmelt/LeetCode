# 2944 Minimum Number of Coins for Fruits 购买水果需要的最少金币数

__Description:__

You are given an __0-indexed__ integer array `prices` where `prices[i]` denotes the number of coins needed to purchase the `(i + 1)th` fruit.

The fruit market has the following reward for each fruit:

- If you purchase the `(i + 1)th` fruit at `prices[i]` coins, you can get any number of the next `i` fruits for free.

__Note__ that even if you __can__ take fruit `j` for free, you can still purchase it for `prices[j - 1]` coins to receive its reward.

Return the minimum number of coins needed to acquire all the fruits.

__Example:__

Example 1:

```text
Input: prices = [3,1,2]

Output: 4

Explanation:
```

- Purchase the 1st fruit with `prices[0] = 3` coins, you are allowed to take the 2nd fruit for free.
- Purchase the 2nd fruit with `prices[1] = 1` coin, you are allowed to take the 3rd fruit for free.
- Take the 3rd fruit for free.

Note that even though you could take the 2nd fruit for free as a reward of buying 1st fruit, you purchase it to receive its reward, which is more optimal.

Example 2:

```text
Input: prices = [1,10,1,1]

Output: 2

Explanation:
```

- Purchase the 1st fruit with `prices[0] = 1` coin, you are allowed to take the 2nd fruit for free.
- Take the 2nd fruit for free.
- Purchase the 3rd fruit for `prices[2] = 1` coin, you are allowed to take the 4th fruit for free.
- Take the 4th fruit for free.

Example 3:

```text
Input: prices = [26,18,6,12,49,7,45,45]

Output: 39

Explanation:
```

- Purchase the 1st fruit with `prices[0] = 26` coin, you are allowed to take the 2nd fruit for free.
- Take the 2nd fruit for free.
- Purchase the 3rd fruit for `prices[2] = 6` coin, you are allowed to take the 4th, 5th and 6th (the next three) fruits for free.
- Take the 4th fruit for free.
- Take the 5th fruit for free.
- Purchase the 6th fruit with `prices[5] = 7` coin, you are allowed to take the 8th and 9th fruit for free.
- Take the 7th fruit for free.
- Take the 8th fruit for free.

Note that even though you could take the 6th fruit for free as a reward of buying 3rd fruit, you purchase it to receive its reward, which is more optimal.

__Constraints:__

- `1 <= prices.length <= 1000`
- `1 <= prices[i] <= 10 ^ 5`

__题目描述:__

给你一个 __下标从 0 开始的__ 整数数组 `prices` ，其中 `prices[i]` 表示你购买第 `i + 1` 个水果需要花费的金币数目。

水果超市有如下促销活动：

- 如果你花费 `prices[i]` 购买了下标为 `i + 1` 的水果，那么你可以免费获得下标范围在 `[i + 1, i + i]` 的水果。

__注意__ ，即使你 __可以__ 免费获得水果 `j` ，你仍然可以花费 `prices[j - 1]` 个金币去购买它以获得它的奖励。

请你返回获得所有水果所需要的 最少 金币数。

__示例:__

示例 1：

```text
输入：prices = [3,1,2]

输出：4

解释：
```

- 用 `prices[0] = 3` 个金币购买第 1 个水果，你可以免费获得第 2 个水果。
- 用 `prices[1] = 1` 个金币购买第 2 个水果，你可以免费获得第 3 个水果。
- 免费获得第 3 个水果。

请注意，即使您可以免费获得第 2 个水果作为购买第 1 个水果的奖励，但您购买它是为了获得其奖励，这是更优化的。

示例 2：

```text
输入：prices = [1,10,1,1]

输出：2

解释：
```

- 用 `prices[0] = 1` 个金币购买第 1 个水果，你可以免费获得第 2 个水果。
- 免费获得第 2 个水果。
- 用 `prices[2] = 1` 个金币购买第 3 个水果，你可以免费获得第 4 个水果。
- 免费获得第 4 个水果。

示例 3：

```text
输入：prices = [26,18,6,12,49,7,45,45]

输出：39

解释：
```

- 用 `prices[0] = 26` 个金币购买第 1 个水果，你可以免费获得第 2 个水果。
- 免费获得第 2 个水果。
- 用 `prices[2] = 6` 个金币购买第 3 个水果，你可以免费获得第 4，5，6（接下来的三个）水果。
- 免费获得第 4 个水果。
- 免费获得第 5 个水果。
- 用 `prices[5] = 7` 个金币购买第 6 个水果，你可以免费获得第 7 和 第 8 个水果。
- 免费获得第 7 个水果。
- 免费获得第 8 个水果。

请注意，即使您可以免费获得第 6 个水果作为购买第 3 个水果的奖励，但您购买它是为了获得其奖励，这是更优化的。

__提示：__

- `1 <= prices.length <= 1000`
- `1 <= prices[i] <= 10 ^ 5`

__思路:__

```text
1. 动态规划
设 dp[i] 为购买当前水果的情况下获得当前水果及之后所有水果的最小花费
dp[i] = prices[i - 1] + min(dp[j]), 其中 i <= j <= (i << 1)
对于 i > (n + 1) >> 1, dp[i] = prices[i]
因为此时买了当前水果能免费获得之后所有的水果
可以直接在 prices 数组原地更新
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 单调队列优化
由方法 1 可以看出来
dp[i] 需要求 [i, (i << 1)] 这个不定长滑动窗口里的最小值
将下标和对应最小值存在一个单调队列中
从队首到队尾下标依次增大
可以在队列里放一个 n + 1 作为哨兵
当队尾的下标比当前遍历元素的两倍还要大时弹出队尾
比较队首的值与当前值的大小关系
如果队首的值不小于当前值 cur = prices[i - 1] + q[-1][1]
说明队首可以被当前值替代
也就是说买当前水果能够覆盖掉队首
弹出队首
最后遍历完之后加入 [i, cur] 于队首
最后直接返回队首花费
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumCoins(vector<int>& prices) 
    {
        int n = prices.size();
        deque<pair<int, int>> q;
        q.emplace_front(n + 1, 0);
        for (int i = n; i > 0; i--)
        {
            while (q.back().first > (i << 1) + 1) q.pop_back();
            int cur = prices[i - 1] + q.back().second;
            while (cur <= q.front().second) q.pop_front();
            q.emplace_front(i, cur);
        }
        return q.front().second;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumCoins(int[] prices) {
        int n = prices.length;
        var q = new ArrayDeque<int[]>();
        q.addLast(new int[]{ n + 1, 0 });
        for (int i = n; i > 0; i--) {
            while (q.peekLast()[0] > (i << 1) + 1) q.pollLast();
            int cur = prices[i - 1] + q.peekLast()[1];
            while (cur <= q.peekFirst()[1]) q.pollFirst();
            q.addFirst(new int[]{ i, cur });
        }
        return q.peekFirst()[1];
    }
}
```

__Python__:

```Python
class Solution:
    def minimumCoins(self, prices: List[int]) -> int:
        for i in range(((len(prices) + 1) >> 1) - 1, 0, -1):
            prices[i - 1] += min(prices[i:(i << 1) + 1])
        return prices[0]
```
