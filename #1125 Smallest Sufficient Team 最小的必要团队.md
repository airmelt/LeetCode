# 1125 Smallest Sufficient Team 最小的必要团队

__Description__:
In a project, you have a list of required skills req_skills, and a list of people. The ith person people[i] contains a list of skills that the person has.

Consider a sufficient team: a set of people such that for every required skill in req_skills, there is at least one person in the team who has that skill. We can represent these teams by the index of each person.

For example, team = [0, 1, 3] represents the people with skills people[0], people[1], and people[3].
Return any sufficient team of the smallest possible size, represented by the index of each person. You may return the answer in any order.

It is guaranteed an answer exists.

__Example:__

Example 1:

Input: req_skills = ["java","nodejs","reactjs"], people = [["java"],["nodejs"],["nodejs","reactjs"]]
Output: [0,2]

Example 2:

Input: req_skills = ["algorithms","math","java","reactjs","csharp","aws"], people = [["algorithms","math","java"],["algorithms","math","reactjs"],["java","csharp","aws"],["reactjs","csharp"],["csharp","math"],["aws","java"]]
Output: [1,2]

__Constraints:__

1 <= req_skills.length <= 16
1 <= req_skills[i].length <= 16
req_skills[i] consists of lowercase English letters.
All the strings of req_skills are unique.
1 <= people.length <= 60
0 <= people[i].length <= 16
1 <= people[i][j].length <= 16
people[i][j] consists of lowercase English letters.
All the strings of people[i] are unique.
Every skill in people[i] is a skill in req_skills.
It is guaranteed a sufficient team exists.

__题目描述__:
作为项目经理，你规划了一份需求的技能清单 req_skills，并打算从备选人员名单 people 中选出些人组成一个「必要团队」（ 编号为 i 的备选人员 people[i] 含有一份该备选人员掌握的技能列表）。

所谓「必要团队」，就是在这个团队中，对于所需求的技能列表 req_skills 中列出的每项技能，团队中至少有一名成员已经掌握。可以用每个人的编号来表示团队中的成员：

例如，团队 team = [0, 1, 3] 表示掌握技能分别为 people[0]，people[1]，和 people[3] 的备选人员。
请你返回 任一 规模最小的必要团队，团队成员用人员编号表示。你可以按 任意顺序 返回答案，题目数据保证答案存在。

__示例 :__

示例 1：

输入：req_skills = ["java","nodejs","reactjs"], people = [["java"],["nodejs"],["nodejs","reactjs"]]
输出：[0,2]

示例 2：

输入：req_skills = ["algorithms","math","java","reactjs","csharp","aws"], people = [["algorithms","math","java"],["algorithms","math","reactjs"],["java","csharp","aws"],["reactjs","csharp"],["csharp","math"],["aws","java"]]
输出：[1,2]

__提示:__

1 <= req_skills.length <= 16
1 <= req_skills[i].length <= 16
req_skills[i] 由小写英文字母组成
req_skills 中的所有字符串 互不相同
1 <= people.length <= 60
0 <= people[i].length <= 16
1 <= people[i][j].length <= 16
people[i][j] 由小写英文字母组成
people[i] 中的所有字符串 互不相同
people[i] 中的每个技能是 req_skills 中的技能
题目数据保证「必要团队」一定存在

__思路__:

状态压缩动态规划
m, n 分别为技能数和人数
因为技能最多 16 个, 可以用一个整形记录所有技能, 每一个二进制位取 1 表示有对应技能
使用 map 记录下当前的技能对应的下标
设 dp[i] 表示获得当前技能组合需要的最少人数
dp.back() 表示获取所有技能组合需要的最少人数
设当前人对应的技能组合为 cur, cur 使用或计算求取
使用 cur 更新 dp, dp[cur | j] = dp[j] + 1, 0 <= j < (1 << m)
使用 team 维护当前 dp 对应的人的列表
最后返回 team.back() 表示获取所有技能的队伍
时间复杂度为 O(n \* 2 ^ m), 空间复杂度为 O(n \* 2 ^ m)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> smallestSufficientTeam(vector<string>& req_skills, vector<vector<string>>& people) 
    {
        int m = req_skills.size(), n = people.size();
        vector<int> dp((1 << m), -1);
        dp.front() = 0;
        unordered_map<string, int> skills;
        for (int i = 0; i < m; i++) skills[req_skills[i]] = i;
        vector<vector<int>> team((1 << m), vector<int>());
        for (int i = 0; i < n; i++)
        {
            int cur = 0;
            for (const auto& s : people[i]) cur |= (1 << skills[s]);
            for (int j = 0, x = 0; j < (1 << m); j++)
            {
                if (dp[j] > -1)
                {
                    if (dp[x = cur | j] == -1 or dp[x] > dp[j] + 1)
                    {
                        dp[x] = dp[j] + 1;
                        team[x].clear();
                        team[x].insert(team[x].end(), team[j].begin(), team[j].end());
                        team[x].emplace_back(i);
                    }
                }
            }
        }
        return team.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int[] smallestSufficientTeam(String[] req_skills, List<List<String>> people) {
        int m = req_skills.length, n = people.size(), dp[] = new int[1 << m];
        Arrays.fill(dp, -1);
        dp[0] = 0;
        Map<String, Integer> skills = new HashMap<>(m);
        for (int i = 0; i < m; i++) skills.put(req_skills[i], i);
        List<List<Integer>> team = new ArrayList<>(1 << m);
        for (int i = 0; i < (1 << m); i++) team.add(new ArrayList<>());
        for (int i = 0; i < n; i++) {
            int cur = 0;
            for (String s : people.get(i)) cur |= (1 << skills.get(s));
            for (int j = 0, x = 0; j < (1 << m); j++) {
                if (dp[j] > -1) {
                    if (dp[x = cur | j] == -1 || dp[x] > dp[j] + 1) {
                        dp[x] = dp[j] + 1;
                        team.get(x).clear();
                        team.get(x).addAll(team.get(j));
                        team.get(x).add(i);
                    }
                }
            }
        }
        return team.get((1 << m) - 1).stream().mapToInt(i -> i).toArray();
    }
}
```

__Python__:

```Python
class Solution:
    def smallestSufficientTeam(self, req_skills: List[str], people: List[List[str]]) -> List[int]:
        dp, n, skills, team, or_function = [0] + [-1] * ((1 << (m := len(req_skills))) - 1), len(people), {v: i for i, v in enumerate(req_skills)}, [[] for _ in range(1 << m)], lambda x, y: x | (1 << y)
        for i in range(n):
            cur = reduce(or_function, [0] + [skills[x] for x in people[i]])
            for j in range(1 << m):
                if dp[j] > -1:
                    if dp[x := cur | j] == -1 or dp[x] > dp[j] + 1:
                        dp[x], team[x] = dp[j] + 1, team[j] + [i]
        return team[-1]
```
