# 1049 Last Stone Weight II 最后一块石头的重量 II

__Description__:
You are given an array of integers stones where stones[i] is the weight of the ith stone.

We are playing a game with the stones. On each turn, we choose any two stones and smash them together. Suppose the stones have weights x and y with x <= y. The result of this smash is:

If x == y, both stones are destroyed, and
If x != y, the stone of weight x is destroyed, and the stone of weight y has new weight y - x.
At the end of the game, there is at most one stone left.

Return the smallest possible weight of the left stone. If there are no stones left, return 0.

__Example:__

Example 1:

Input: stones = [2,7,4,1,8,1]
Output: 1
Explanation:
We can combine 2 and 4 to get 2, so the array converts to [2,7,1,8,1] then,
we can combine 7 and 8 to get 1, so the array converts to [2,1,1,1] then,
we can combine 2 and 1 to get 1, so the array converts to [1,1,1] then,
we can combine 1 and 1 to get 0, so the array converts to [1], then that's the optimal value.
Example 2:

Input: stones = [31,26,33,21,40]
Output: 5

__Constraints:__

1 <= stones.length <= 30
1 <= stones[i] <= 100

__题目描述__:
有一堆石头，用整数数组 stones 表示。其中 stones[i] 表示第 i 块石头的重量。

每一回合，从中选出任意两块石头，然后将它们一起粉碎。假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：

如果 x == y，那么两块石头都会被完全粉碎；
如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。
最后，最多只会剩下一块 石头。返回此石头 最小的可能重量 。如果没有石头剩下，就返回 0。

__示例 :__

示例 1：

输入：stones = [2,7,4,1,8,1]
输出：1
解释：
组合 2 和 4，得到 2，所以数组转化为 [2,7,1,8,1]，
组合 7 和 8，得到 1，所以数组转化为 [2,1,1,1]，
组合 2 和 1，得到 1，所以数组转化为 [1,1,1]，
组合 1 和 1，得到 0，所以数组转化为 [1]，这就是最优值。

示例 2：

输入：stones = [31,26,33,21,40]
输出：5

示例 3：

输入：stones = [1,2]
输出：1

__提示:__

1 <= stones.length <= 30
1 <= stones[i] <= 100

__思路__:

动态规划
从长的单词中删除一个字母来得到更短的单词
设 dp[word] 表示以 word 开头能得到的依次递减的最长字符串链
dp[word] = max(dp[word[:i] + word[i + 1:]] + 1)
初始化 dp[word] = 1, 因为 word 本身也是一个长度为 1 的字符串链
最后取 max(dp.values())
时间复杂度O(mn), 空间复杂度O(mn), n 为 words 数组的长度, m 为每个单词的平均长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int lastStoneWeightII(vector<int>& stones) 
    {
        int sum = accumulate(stones.begin(), stones.end(), 0);
        vector<int> dp((sum >> 1) + 1);
        for (const auto& stone : stones) for (int j = (sum >> 1); j > stone - 1; j--) dp[j] = max(dp[j], dp[j - stone] + stone);
        return sum - (dp[(sum >> 1)] << 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for (int stone : stones) sum += stone;
        int[] dp = new int[(sum >>> 1) + 1];
        for (int stone : stones) for (int j = (sum >>> 1); j > stone - 1; j--) dp[j] = Math.max(dp[j], dp[j - stone] + stone);
        return sum - (dp[(sum >>> 1)] << 1);
    }
}
```

__Python__:

```Python
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        dp = [0] * (((s := sum(stones)) >> 1) + 1)
        for i in stones:
            for j in range((s >> 1), i - 1, -1):
                dp[j] = max(dp[j], dp[j - i] + i)
        return s - (dp[-1] << 1)
```
