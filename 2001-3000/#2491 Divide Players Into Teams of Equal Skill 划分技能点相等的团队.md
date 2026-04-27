# 2491 Divide Players Into Teams of Equal Skill 划分技能点相等的团队

__Description:__

You are given a positive integer array `skill` of __even__ length `n` where `skill[i]` denotes the skill of the `i ^ th` player. Divide the players into `n / 2` teams of size `2` such that the total skill of each team is __equal__.

The chemistry of a team is equal to the product of the skills of the players on that team.

Return _the sum of the __chemistry__ of all the teams, or return_ `-1` _if there is no way to divide the players into teams such that the total skill of each team is equal._

__Example:__

Example 1:

```text
Input: skill = [3,2,5,1,3,4]
Output: 22
Explanation: 
Divide the players into the following teams: (1, 5), (2, 4), (3, 3), where each team has a total skill of 6.
The sum of the chemistry of all the teams is: 1 * 5 + 2 * 4 + 3 * 3 = 5 + 8 + 9 = 22.
```

Example 2:

```text
Input: skill = [3,4]
Output: 12
Explanation: 
The two players form a team with a total skill of 7.
The chemistry of the team is 3 * 4 = 12.
```

Example 3:

```text
Input: skill = [1,1,2,3]
Output: -1
Explanation: 
There is no way to divide the players into teams such that the total skill of each team is equal.
```

__Constraints:__

- `2 <= skill.length <= 10 ^ 5`
- `skill.length` is even.
- `1 <= skill[i] <= 1000`

__题目描述:__

给你一个正整数数组 `skill` ，数组长度为 __偶数__ `n` ，其中 `skill[i]` 表示第 `i` 个玩家的技能点。将所有玩家分成 `n / 2` 个 `2` 人团队，使每一个团队的技能点之和 __相等__ 。

团队的 化学反应 等于团队中玩家的技能点 乘积 。

返回所有团队的 __化学反应__ 之和，如果无法使每个团队的技能点之和相等，则返回 `-1` 。

__示例:__

示例 1：

```text
输入：skill = [3,2,5,1,3,4]
输出：22
解释：
将玩家分成 3 个团队 (1, 5), (2, 4), (3, 3) ，每个团队的技能点之和都是 6 。
所有团队的化学反应之和是 1 * 5 + 2 * 4 + 3 * 3 = 5 + 8 + 9 = 22 。
```

示例 2：

```text
输入：skill = [3,4]
输出：12
解释：
两个玩家形成一个团队，技能点之和是 7 。
团队的化学反应是 3 * 4 = 12 。
```

示例 3：

```text
输入：skill = [1,1,2,3]
输出：-1
解释：
无法将玩家分成每个团队技能点都相等的若干个 2 人团队。
```

__提示：__

- `2 <= skill.length <= 10 ^ 5`
- `skill.length` 是偶数
- `1 <= skill[i] <= 1000`

__思路:__

```text
贪心
由于要使得技能点之和匹配
必须要最大的和最小的匹配
依次类推
所以先排序
如果当前不匹配返回 -1
否则累加乘积
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long dividePlayers(vector<int>& skill) 
    {
        sort(skill.begin(), skill.end());
        long target = skill.front() + skill.back(), result = skill.front() * skill.back();
        for (int i = 1, n = skill.size(), half = n >> 1; i < half; i++) 
        {
            if (skill[i] + skill[n - i - 1] != target) return -1;
            result += skill[i] * skill[n - i - 1];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long dividePlayers(int[] skill) {
        Arrays.sort(skill);
        long target = skill[0] + skill[skill.length - 1], result = skill[0] * skill[skill.length - 1];
        for (int i = 1, n = skill.length, half = n >> 1; i < half; i++) {
            if (skill[i] + skill[n - i - 1] != target) return -1;
            result += skill[i] * skill[n - i - 1];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def dividePlayers(self, skill: List[int]) -> int:
        skill.sort()
        return sum(a * b for a, b in zip(skill, skill[::-1])) >> 1 if all(skill[0] + skill[-1] == a + b for a, b in zip(skill, skill[::-1])) else -1
```
