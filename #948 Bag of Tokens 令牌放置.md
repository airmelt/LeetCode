# 948 Bag of Tokens 令牌放置

__Description__:
You have an initial power of power, an initial score of 0, and a bag of tokens where tokens[i] is the value of the ith token (0-indexed).

Your goal is to maximize your total score by potentially playing each token in one of two ways:

If your current power is at least tokens[i], you may play the ith token face up, losing tokens[i] power and gaining 1 score.
If your current score is at least 1, you may play the ith token face down, gaining tokens[i] power and losing 1 score.
Each token may be played at most once and in any order. You do not have to play all the tokens.

Return the largest possible score you can achieve after playing any number of tokens.

__Example:__

Example 1:

Input: tokens = [100], power = 50
Output: 0
Explanation: Playing the only token in the bag is impossible because you either have too little power or too little score.

Example 2:

Input: tokens = [100,200], power = 150
Output: 1
Explanation: Play the 0th token (100) face up, your power becomes 50 and score becomes 1.
There is no need to play the 1st token since you cannot play it face up to add to your score.

Example 3:

Input: tokens = [100,200,300,400], power = 200
Output: 2
Explanation: Play the tokens in this order to get a score of 2:

1. Play the 0th token (100) face up, your power becomes 100 and score becomes 1.
2. Play the 3rd token (400) face down, your power becomes 500 and score becomes 0.
3. Play the 1st token (200) face up, your power becomes 300 and score becomes 1.
4. Play the 2nd token (300) face up, your power becomes 0 and score becomes 2.

__Constraints:__

0 <= tokens.length <= 1000
0 <= tokens[i], power < 10^4

__题目描述__:
你的初始 能量 为 P，初始 分数 为 0，只有一包令牌 tokens 。其中 tokens[i] 是第 i 个令牌的值（下标从 0 开始）。

令牌可能的两种使用方法如下：

如果你至少有 token[i] 点 能量 ，可以将令牌 i 置为正面朝上，失去 token[i] 点 能量 ，并得到 1 分 。
如果我们至少有 1 分 ，可以将令牌 i 置为反面朝上，获得 token[i] 点 能量 ，并失去 1 分 。
每个令牌 最多 只能使用一次，使用 顺序不限 ，不需 使用所有令牌。

在使用任意数量的令牌后，返回我们可以得到的最大 分数 。

__示例 :__

示例 1：

输入：tokens = [100], P = 50
输出：0
解释：无法使用唯一的令牌，因为能量和分数都太少了。

示例 2：

输入：tokens = [100,200], P = 150
输出：1
解释：令牌 0 正面朝上，能量变为 50，分数变为 1 。不必使用令牌 1 ，因为你无法使用它来提高分数。

示例 3：

输入：tokens = [100,200,300,400], P = 200
输出：2
解释：按下面顺序使用令牌可以得到 2 分：

1. 令牌 0 正面朝上，能量变为 100 ，分数变为 1
2. 令牌 3 正面朝下，能量变为 500 ，分数变为 0
3. 令牌 1 正面朝上，能量变为 300 ，分数变为 1
4. 令牌 2 正面朝上，能量变为 0 ，分数变为 2

__提示:__

0 <= tokens.length <= 1000
0 <= tokens[i], P < 10^4

__思路__:

贪心
先对数组进行排序
每次先增加分数
只有当能量不够时才从数组最后选择能量最多的加上
分数一定在增加之后产生最大值
时间复杂度为 O(nlgn), 空间复杂度为 O(lgn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int bagOfTokensScore(vector<int>& tokens, int power) 
    {
        sort(tokens.begin(), tokens.end());
        int n = tokens.size(), i = 0, j = n - 1, score = 0;
        while (i <= j)
        {
            if (tokens[i] > power)
            {
                if (score > 0) power += (tokens[j--] - tokens[i++]);
                else return score;
            }
            else
            {
                power -= tokens[i++];
                ++score;
            }
        }
        return score;
    }
};
```

__Java__:

```Java
class Solution {
    public int bagOfTokensScore(int[] tokens, int power) {
        Arrays.sort(tokens);
        int n = tokens.length, i = 0, j = n - 1, score = 0;
        while (i <= j) {
            if (tokens[i] > power) {
                if (score > 0) power += (tokens[j--] - tokens[i++]);
                else return score;
            } else {
                power -= tokens[i++];
                ++score;
            }
        }
        return score;
    }
}
```

__Python__:

```Python
class Solution:
    def bagOfTokensScore(self, tokens: List[int], power: int) -> int:
        tokens.sort()
        i, j, score = 0, len(tokens) - 1, 0
        while i <= j:
            if tokens[i] > power:
                if score > 0:
                    power += tokens[j] - tokens[i]
                    j -= 1
                    i += 1
                else:
                    return score
            else:
                power -= tokens[i]
                i += 1
                score += 1
        return score
```
