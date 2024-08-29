# 2225 Find Players With Zero or One Losses 找出输掉零场或一场比赛的玩家

__Description:__

You are given an integer array `matches` where `matches[i] = [winneri, loseri]` indicates that the player `winneri` defeated player `loseri` in a match.

Return _a list_ `answer` _of size_ `2` _where:_

- `answer[0]` is a list of all players that have __not__ lost any matches.
- `answer[1]` is a list of all players that have lost exactly __one__ match.

The values in the two lists should be returned in increasing order.

Note:

- You should only consider the players that have played __at least one__ match.
- The testcases will be generated such that __no__ two matches will have the __same__ outcome.

__Example:__

Example 1:

```text
Input: matches = [[1,3],[2,3],[3,6],[5,6],[5,7],[4,5],[4,8],[4,9],[10,4],[10,9]]
Output: [[1,2,10],[4,5,7,8]]
Explanation:
Players 1, 2, and 10 have not lost any matches.
Players 4, 5, 7, and 8 each have lost one match.
Players 3, 6, and 9 each have lost two matches.
Thus, answer[0] = [1,2,10] and answer[1] = [4,5,7,8].
```

Example 2:

```text
Input: matches = [[2,3],[1,3],[5,4],[6,4]]
Output: [[1,2,5,6],[]]
Explanation:
Players 1, 2, 5, and 6 have not lost any matches.
Players 3 and 4 each have lost two matches.
Thus, answer[0] = [1,2,5,6] and answer[1] = [].
```

__Constraints:__

- `1 <= matches.length <= 10 ^ 5`
- `matches[i].length == 2`
- `1 <= winneri, loseri <= 10 ^ 5`
- `winneri != loseri`
- All `matches[i]` are __unique__.

__题目描述:__

给你一个整数数组 `matches` 其中 `matches[i] = [winneri, loseri]` 表示在一场比赛中 `winneri` 击败了 `loseri` 。

返回一个长度为 2 的列表 `answer` :

- `answer[0]` 是所有 __没有__ 输掉任何比赛的玩家列表。
- `answer[1]` 是所有恰好输掉 __一场__ 比赛的玩家列表。

两个列表中的值都应该按 递增 顺序返回。

注意：

- 只考虑那些参与 __至少一场__ 比赛的玩家。
- 生成的测试用例保证 __不存在__ 两场比赛结果 __相同__ 。

__示例:__

示例 1：

```text
输入：matches = [[1,3],[2,3],[3,6],[5,6],[5,7],[4,5],[4,8],[4,9],[10,4],[10,9]]
输出：[[1,2,10],[4,5,7,8]]
解释：
玩家 1、2 和 10 都没有输掉任何比赛。
玩家 4、5、7 和 8 每个都输掉一场比赛。
玩家 3、6 和 9 每个都输掉两场比赛。
因此，answer[0] = [1,2,10] 和 answer[1] = [4,5,7,8] 。
```

示例 2：

```text
输入：matches = [[2,3],[1,3],[5,4],[6,4]]
输出：[[1,2,5,6],[]]
解释：
玩家 1、2、5 和 6 都没有输掉任何比赛。
玩家 3 和 4 每个都输掉两场比赛。
因此，answer[0] = [1,2,5,6] 和 answer[1] = [] 。
```

__提示：__

- `1 <= matches.length <= 10 ^ 5`
- `matches[i].length == 2`
- `1 <= winneri, loseri <= 10 ^ 5`
- `winneri != loseri`
- 所有 `matches[i]` __互不相同__

__思路:__

```text
哈希表
使用哈希表记录每场比赛的输家
赢家也记录在哈希表中
统计所有哈希表中输掉比赛次数小于 2 的玩家
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 其中 N 为比赛场数, 主要时间消耗在排序上
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> findWinners(vector<vector<int>>& matches) 
    {
        vector<vector<int>> result(2);
        unordered_map<int, int> loses;
        for (const auto& match : matches) 
        {
            if (!loses.count(match.front())) loses[match.front()] = 0;
            ++loses[match.back()];
        }
        for (const auto& [player, v] : loses) if (v < 2) result[v].push_back(player);
        sort(result.front().begin(), result.front().end());
        sort(result.back().begin(), result.back().end());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> findWinners(int[][] matches) {
        List<List<Integer>> result = List.of(new ArrayList<>(), new ArrayList<>());
        Map<Integer, Integer> loses = new HashMap<>();
        for (var match : matches) {
            if (!loses.containsKey(match[0])) loses.put(match[0], 0);
            loses.merge(match[1], 1, Integer::sum);
        }
        for (var entry : loses.entrySet()) if (entry.getValue() < 2) result.get(entry.getValue()).add(entry.getKey());
        Collections.sort(result.get(0));
        Collections.sort(result.get(1));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findWinners(self, matches: List[List[int]]) -> List[List[int]]:
        return [sorted(x for x in players if x not in loses), sorted(x for x, c in loses.items() if c == 1)] if (loses := Counter(loser for _, loser in matches)) and (players := set(x for m in matches for x in m)) else [[], []]
```
