# 649 Dota2 Senate Dota2 参议院

__Description__:
In the world of Dota2, there are two parties: the Radiant and the Dire.

The Dota2 senate consists of senators coming from two parties. Now the Senate wants to decide on a change in the Dota2 game. The voting for this change is a round-based procedure. In each round, each senator can exercise one of the two rights:

Ban one senator's right: A senator can make another senator lose all his rights in this and all the following rounds.
Announce the victory: If this senator found the senators who still have rights to vote are all from the same party, he can announce the victory and decide on the change in the game.
Given a string senate representing each senator's party belonging. The character 'R' and 'D' represent the Radiant party and the Dire party. Then if there are n senators, the size of the given string will be n.

The round-based procedure starts from the first senator to the last senator in the given order. This procedure will last until the end of voting. All the senators who have lost their rights will be skipped during the procedure.

Suppose every senator is smart enough and will play the best strategy for his own party. Predict which party will finally announce the victory and change the Dota2 game. The output should be "Radiant" or "Dire".

__Example:__

Example 1:

Input: senate = "RD"
Output: "Radiant"
Explanation:
The first senator comes from Radiant and he can just ban the next senator's right in round 1.
And the second senator can't exercise any rights anymore since his right has been banned.
And in round 2, the first senator can just announce the victory since he is the only guy in the senate who can vote.

Example 2:

Input: senate = "RDD"
Output: "Dire"
Explanation:
The first senator comes from Radiant and he can just ban the next senator's right in round 1.
And the second senator can't exercise any rights anymore since his right has been banned.
And the third senator comes from Dire and he can ban the first senator's right in round 1.
And in round 2, the third senator can just announce the victory since he is the only guy in the senate who can vote.

__Constraints:__

n == senate.length
1 <= n <= 10^4
senate[i] is either 'R' or 'D'.

__题目描述__:
Dota2 的世界里有两个阵营：Radiant(天辉)和 Dire(夜魇)

Dota2 参议院由来自两派的参议员组成。现在参议院希望对一个 Dota2 游戏里的改变作出决定。他们以一个基于轮为过程的投票进行。在每一轮中，每一位参议员都可以行使两项权利中的一项：

禁止一名参议员的权利：

参议员可以让另一位参议员在这一轮和随后的几轮中丧失所有的权利。

宣布胜利：
如果参议员发现有权利投票的参议员都是同一个阵营的，他可以宣布胜利并决定在游戏中的有关变化。

给定一个字符串代表每个参议员的阵营。字母 “R” 和 “D” 分别代表了 Radiant（天辉）和 Dire（夜魇）。然后，如果有 n 个参议员，给定字符串的大小将是 n。

以轮为基础的过程从给定顺序的第一个参议员开始到最后一个参议员结束。这一过程将持续到投票结束。所有失去权利的参议员将在过程中被跳过。

假设每一位参议员都足够聪明，会为自己的政党做出最好的策略，你需要预测哪一方最终会宣布胜利并在 Dota2 游戏中决定改变。输出应该是 Radiant 或 Dire。

__示例 :__

示例 1：

输入："RD"
输出："Radiant"
解释：第一个参议员来自 Radiant 阵营并且他可以使用第一项权利让第二个参议员失去权力，因此第二个参议员将被跳过因为他没有任何权利。然后在第二轮的时候，第一个参议员可以宣布胜利，因为他是唯一一个有投票权的人

示例 2：

输入："RDD"
输出："Dire"
解释：
第一轮中,第一个来自 Radiant 阵营的参议员可以使用第一项权利禁止第二个参议员的权利
第二个来自 Dire 阵营的参议员会被跳过因为他的权利被禁止
第三个来自 Dire 阵营的参议员可以使用他的第一项权利禁止第一个参议员的权利
因此在第二轮只剩下第三个参议员拥有投票的权利,于是他可以宣布胜利

__提示:__

给定字符串的长度在 [1, 10,000] 之间.

__思路__:

1. 直接模拟
每次能免去下一个投票就使用这项权利
直到将一方完全排除投票
每一轮都需要重新统计留下来的人数
时间复杂度 O(n), 空间复杂度 O(n)
2. 贪心 ➕ 队列
用两个队列分别存放两个参议院对应的人的下标
每次弹出队首, 将较小的元素加上数组长度再放入队列, 表示下一轮可以继续投票
直到有一个队列为空不能投票
时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string predictPartyVictory(string senate) 
    {
        queue<int> radiant, dire;
        for (int i = 0; i < senate.length(); i++) 
        {
            if (senate[i] == 'R') radiant.push(i);
            else dire.push(i);
        }
        while (!radiant.empty() and !dire.empty()) 
        {
            int r = radiant.front(), d = dire.front();
            radiant.pop();
            dire.pop();
            if (r < d) radiant.push(r + senate.size());
            else dire.push(d + senate.size());
        }
        return radiant.empty() ? "Dire" : "Radiant";
    }
};
```

__Java__:

```Java
class Solution {
    public String predictPartyVictory(String senate) {
        Queue<Integer> radiant = new LinkedList<>(), dire = new LinkedList<>();
        for (int i = 0; i < senate.length(); i++) {
            if (senate.charAt(i) == 'R') radiant.offer(i);
            else dire.offer(i);
        }
        while (!radiant.isEmpty() && !dire.isEmpty()) {
            int r = radiant.remove(), d = dire.remove();
            if (r < d) radiant.offer(r + senate.length());
            else dire.offer(d + senate.length());
        }
        return radiant.isEmpty() ? "Dire" : "Radiant";
    }
}
```

__Python__:

```Python
class Solution:
    def predictPartyVictory(self, senate: str) -> str:
        ban_d = ban_r = 0
        while True:
            cur = ''
            for i in senate:
                if i == 'R':
                    if ban_d:
                        ban_d -= 1
                    else:
                        ban_r += 1
                        cur += i
                else:
                    if ban_r:
                        ban_r -= 1
                    else:
                        ban_d += 1
                        cur += i      
            senate = cur
            if 'D' not in cur:
                return "Radiant"
            if 'R' not in cur:
                return "Dire"
```
