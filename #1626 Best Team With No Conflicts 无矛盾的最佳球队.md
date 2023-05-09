# 1626 Best Team With No Conflicts 无矛盾的最佳球队

__Description:__

You are the manager of a basketball team. For the upcoming tournament, you want to choose the team with the highest overall score. The score of the team is the sum of scores of all the players in the team.

However, the basketball team is not allowed to have conflicts. A conflict exists if a younger player has a strictly higher score than an older player. A conflict does not occur between players of the same age.

Given two lists, `scores` and `ages`, where each `scores[i]` and `ages[i]` represents the score and age of the `i ^ th` player, respectively, return _the highest overall score of all possible basketball teams_.

__Example:__

Example 1:

```text
Input: scores = [1,3,5,10,15], ages = [1,2,3,4,5]
Output: 34
Explanation: You can choose all the players.
```

Example 2:

```text
Input: scores = [4,5,6,5], ages = [2,1,2,1]
Output: 16
Explanation: It is best to choose the last 3 players. Notice that you are allowed to choose multiple people of the same age.
```

Example 3:

```text
Input: scores = [1,2,3,5], ages = [8,9,10,1]
Output: 6
Explanation: It is best to choose the first 3 players.
```

__Constraints:__

- `1 <= scores.length, ages.length <= 1000`
- `scores.length == ages.length`
- `1 <= scores[i] <= 10 ^ 6`
- `1 <= ages[i] <= 1000`

__题目描述:__

假设你是球队的经理。对于即将到来的锦标赛，你想组合一支总体得分最高的球队。球队的得分是球队中所有球员的分数 总和 。

然而，球队中的矛盾会限制球员的发挥，所以必须选出一支 没有矛盾 的球队。如果一名年龄较小球员的分数 严格大于 一名年龄较大的球员，则存在矛盾。同龄球员之间不会发生矛盾。

给你两个列表 `scores` 和 `ages`，其中每组 `scores[i]` 和 `ages[i]` 表示第 `i` 名球员的分数和年龄。请你返回 __所有可能的无矛盾球队中得分最高那支的分数__ 。

__示例:__

示例 1：

```text
输入：scores = [1,3,5,10,15], ages = [1,2,3,4,5]
输出：34
解释：你可以选中所有球员。
```

示例 2：

```text
输入：scores = [4,5,6,5], ages = [2,1,2,1]
输出：16
解释：最佳的选择是后 3 名球员。注意，你可以选中多个同龄球员。
```

示例 3：

```text
输入：scores = [1,2,3,5], ages = [8,9,10,1]
输出：6
解释：最佳的选择是前 3 名球员。
```

__提示：__

- `1 <= scores.length, ages.length <= 1000`
- `scores.length == ages.length`
- `1 <= scores[i] <= 10 ^ 6`
- `1 <= ages[i] <= 1000`

__思路:__

```text
1. 动态规划
先将所有人的分数和年龄按照分数优先排序
设 dp[i] 表示以第 i 名选手结尾能够获得的最大分数
遍历 i 前面的选手, 分数是递增的, 所以只要年龄不超过 i 就能加入队伍
最后加上 i 自己的分数就是最大得分
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
2. 动态规划 ➕ 树状数组
从年龄出发, 记录以某个 age 为最大 age 的总得分
按分数和年龄排序之后使用树状数组求出前缀和即可
时间复杂度为 O(NlogN + NlogM), 空间复杂度为 O(N + M), M 为 max(nums)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int max_age;
    vector<int> t;
    vector<pair<int, int>> people;
    int lowbit(int x) 
    {
        return x & (-x);
    }

    void update(int i, int val) 
    {
        for (; i <= max_age; i += lowbit(i)) t[i] = max(t[i], val);
    }
    
    int query(int i) 
    {
        int result = 0;
        for (; i > 0; i -= lowbit(i)) result = max(result, t[i]);
        return result;
    }

    int bestTeamScore(vector<int>& scores, vector<int>& ages) 
    {
        max_age = *max_element(ages.begin(), ages.end());
        t = vector<int>(max_age + 1, 0);
        int result = 0, n = scores.size();
        for (int i = 0; i < n; ++i) people.emplace_back(make_pair(scores[i], ages[i]));
        sort(people.begin(), people.end());
        for (int i = 0; i < n; i++) 
        {
            int score = people[i].first, age = people[i].second, cur = score + query(age);
            update(age, cur);
            result = max(result, cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int bestTeamScore(int[] scores, int[] ages) {
        int n = scores.length, perple[][] = new int[n][2], dp[] = new int[n], result = 0;
        for (int i = 0; i < n; i++) {
            perple[i][0] = ages[i];
            perple[i][1] = scores[i];
        }
        Arrays.sort(perple, (o1, o2) -> {
            return o1[0] != o2[0] ? o1[0] - o2[0] : o1[1] - o2[1];
        });
        for (int i = 0; i < n; i++) {
            dp[i] = perple[i][1];
            for (int j = 0; j < i; j++) if (perple[j][1] <= perple[i][1]) dp[i] = Math.max(dp[i], perple[i][1] + dp[j]);
            result = Math.max(result, dp[i]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def bestTeamScore(self, scores: List[int], ages: List[int]) -> int:
        perple, dp, result = sorted(zip(scores, ages)), [0] * (n := len(scores)), 0
        for i in range(n):
            for j in range(i):
                if perple[i][1] >= perple[j][1]:
                    dp[i] = max(dp[i], dp[j])
            dp[i] += perple[i][0]
            result = max(result, dp[i])
        return result
```
