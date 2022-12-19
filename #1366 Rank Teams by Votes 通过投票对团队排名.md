# 1366 Rank Teams by Votes 通过投票对团队排名

__Description:__

In a special ranking system, each voter gives a rank from highest to lowest to all teams participated in the competition.

The ordering of teams is decided by who received the most position-one votes. If two or more teams tie in the first position, we consider the second position to resolve the conflict, if they tie again, we continue this process until the ties are resolved. If two or more teams are still tied after considering all positions, we rank them alphabetically based on their team letter.

Given an array of strings votes which is the votes of all voters in the ranking systems. Sort all teams according to the ranking system described above.

Return a string of all teams sorted by the ranking system.

__Example:__

Example 1:

Input: votes = ["ABC","ACB","ABC","ACB","ACB"]
Output: "ACB"
Explanation: Team A was ranked first place by 5 voters. No other team was voted as first place so team A is the first team.
Team B was ranked second by 2 voters and was ranked third by 3 voters.
Team C was ranked second by 3 voters and was ranked third by 2 voters.
As most of the voters ranked C second, team C is the second team and team B is the third.

Example 2:

Input: votes = ["WXYZ","XYZW"]
Output: "XWYZ"
Explanation: X is the winner due to tie-breaking rule. X has same votes as W for the first position but X has one vote as second position while W doesn't have any votes as second position.

Example 3:

Input: votes = ["ZMNAGUEDSJYLBOPHRQICWFXTVK"]
Output: "ZMNAGUEDSJYLBOPHRQICWFXTVK"
Explanation: Only one voter so his votes are used for the ranking.

__Constraints:__

1 <= votes.length <= 1000
1 <= votes[i].length <= 26
votes[i].length == votes[j].length for 0 <= i, j < votes.length.
votes\[i][j] is an English uppercase letter.
All characters of votes[i] are unique.
All the characters that occur in votes[0] also occur in votes[j] where 1 <= j < votes.length.

__题目描述：__

现在有一个特殊的排名系统，依据参赛团队在投票人心中的次序进行排名，每个投票者都需要按从高到低的顺序对参与排名的所有团队进行排位。

排名规则如下：

参赛团队的排名次序依照其所获「排位第一」的票的多少决定。如果存在多个团队并列的情况，将继续考虑其「排位第二」的票的数量。以此类推，直到不再存在并列的情况。
如果在考虑完所有投票情况后仍然出现并列现象，则根据团队字母的字母顺序进行排名。
给你一个字符串数组 votes 代表全体投票者给出的排位情况，请你根据上述排名规则对所有参赛团队进行排名。

请你返回能表示按排名系统 排序后 的所有团队排名的字符串。

__示例：__

示例 1：

输入：votes = ["ABC","ACB","ABC","ACB","ACB"]
输出："ACB"
解释：A 队获得五票「排位第一」，没有其他队获得「排位第一」，所以 A 队排名第一。
B 队获得两票「排位第二」，三票「排位第三」。
C 队获得三票「排位第二」，两票「排位第三」。
由于 C 队「排位第二」的票数较多，所以 C 队排第二，B 队排第三。

示例 2：

输入：votes = ["WXYZ","XYZW"]
输出："XWYZ"
解释：X 队在并列僵局打破后成为排名第一的团队。X 队和 W 队的「排位第一」票数一样，但是 X 队有一票「排位第二」，而 W 没有获得「排位第二」。

示例 3：

输入：votes = ["ZMNAGUEDSJYLBOPHRQICWFXTVK"]
输出："ZMNAGUEDSJYLBOPHRQICWFXTVK"
解释：只有一个投票者，所以排名完全按照他的意愿。

示例 4：

输入：votes = ["BCA","CAB","CBA","ABC","ACB","BAC"]
输出："ABC"
解释：
A 队获得两票「排位第一」，两票「排位第二」，两票「排位第三」。
B 队获得两票「排位第一」，两票「排位第二」，两票「排位第三」。
C 队获得两票「排位第一」，两票「排位第二」，两票「排位第三」。
完全并列，所以我们需要按照字母升序排名。

示例 5：

输入：votes = ["M","M","M","M"]
输出："M"
解释：只有 M 队参赛，所以它排名第一。

__提示：__

1 <= votes.length <= 1000
1 <= votes[i].length <= 26
votes[i].length == votes[j].length for 0 <= i, j < votes.length
votes\[i][j] 是英文 大写 字母
votes[i] 中的所有字母都是唯一的
votes[0] 中出现的所有字母 同样也 出现在 votes[j] 中，其中 1 <= j < votes.length

__思路：__

贪心
统计所有数字出现的次数和所有数字之和 s
如果 s 能整除 3, 从大到小输出所有数字即可
如果 s 除 3 余 1, 要么去掉一个最小的除 3 余 1 的数, 要么去掉 2 个最小除 3 余 2 的数
如果 s 除 3 余 2, 要么去掉一个最小的除 3 余 2 的数, 要么去掉 2 个最小除 3 余 2 的数
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    string rankTeams(vector<string>& votes) 
    {
        map<char, vector<int>> m;
        for (const auto& v : votes)
        {
            for (int i = 0; i < v.size(); i++)
            {
                m[v[i]].resize(v.size());
                m[v[i]][i]++;
            }      
        }
        string result = votes.front();
        sort(result.begin(), result.end(), [&](const auto& a, const auto& b) { return m[a] > m[b] or m[a] == m[b] and a < b; });
         return result;  
    }
};
```

__Java__:

```Java
class Solution {
    private Map<Character, List<Integer>> count = new HashMap<>();
    
    public String rankTeams(String[] votes) {
        int n = votes[0].length();
        for (String vote : votes) {
            for (int i = 0; i < n; i++) {
                Character c = vote.charAt(i);
                if (!count.containsKey(c)) {
                    count.put(c, new ArrayList<>());
                    for (int j = 0; j < n; j++) count.get(c).add(0);
                }
                count.get(c).set(i, count.get(c).get(i) - 1);
            }
        }
        char[] chars = votes[0].toCharArray();
        Character[] cs = new Character[n];
        for (int i = 0; i < n; i++) cs[i] = chars[i];
        Arrays.sort(cs, (a, b) -> {
            List<Integer> list1 = count.get(a), list2 = count.get(b);
            for (int i = 0; i < n; i++) {
                if (list1.get(i) > list2.get(i)) return 1;
                if (list1.get(i) < list2.get(i)) return -1;
            }
            return a - b;
        });
        for (int i = 0; i < n; i++) chars[i] = cs[i];
        return new String(chars);
    }
}
```

__Python__:

```Python
class Solution:
    def rankTeams(self, votes: List[str]) -> str:
        d = defaultdict(lambda: [0] * len(votes[0]))
        for vote in votes:
            for i, v in enumerate(vote):
                d[v][i] -= 1
        return ''.join(sorted(votes[0], key=lambda x: (d[x], x)))
```
