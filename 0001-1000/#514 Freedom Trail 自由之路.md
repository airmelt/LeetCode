# 514 Freedom Trail 自由之路

__Description__:
In the video game Fallout 4, the quest "Road to Freedom" requires players to reach a metal dial called the "Freedom Trail Ring", and use the dial to spell a specific keyword in order to open the door.

Given a string ring, which represents the code engraved on the outer ring and another string key, which represents the keyword needs to be spelled. You need to find the minimum number of steps in order to spell all the characters in the keyword.

Initially, the first character of the ring is aligned at 12:00 direction. You need to spell all the characters in the string key one by one by rotating the ring clockwise or anticlockwise to make each character of the string key aligned at 12:00 direction and then by pressing the center button.

At the stage of rotating the ring to spell the key character key[i]:

You can rotate the ring clockwise or anticlockwise one place, which counts as 1 step. The final purpose of the rotation is to align one of the string ring's characters at the 12:00 direction, where this character must equal to the character key[i].
If the character key[i] has been aligned at the 12:00 direction, you need to press the center button to spell, which also counts as 1 step. After the pressing, you could begin to spell the next character in the key (next stage), otherwise, you've finished all the spelling.

__Example:__

![ring](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/ring.jpg)

Input: ring = "godding", key = "gd"
Output: 4
Explanation:
For the first key character 'g', since it is already in place, we just need 1 step to spell this character.
For the second key character 'd', we need to rotate the ring "godding" anticlockwise by two steps to make it become "ddinggo".
Also, we need 1 more step for spelling.
So the final output is 4.

__Note:__

Length of both ring and key will be in range 1 to 100.
There are only lowercase letters in both strings and might be some duplcate characters in both strings.
It's guaranteed that string key could always be spelled by rotating the string ring.

__题目描述__:
电子游戏“辐射4”中，任务“通向自由”要求玩家到达名为“Freedom Trail Ring”的金属表盘，并使用表盘拼写特定关键词才能开门。

给定一个字符串 ring，表示刻在外环上的编码；给定另一个字符串 key，表示需要拼写的关键词。您需要算出能够拼写关键词中所有字符的最少步数。

最初，ring 的第一个字符与12:00方向对齐。您需要顺时针或逆时针旋转 ring 以使 key 的一个字符在 12:00 方向对齐，然后按下中心按钮，以此逐个拼写完 key 中的所有字符。

旋转 ring 拼出 key 字符 key[i] 的阶段中：

您可以将 ring 顺时针或逆时针旋转一个位置，计为1步。旋转的最终目的是将字符串 ring 的一个字符与 12:00 方向对齐，并且这个字符必须等于字符 key[i] 。
如果字符 key[i] 已经对齐到12:00方向，您需要按下中心按钮进行拼写，这也将算作 1 步。按完之后，您可以开始拼写 key 的下一个字符（下一阶段）, 直至完成所有拼写。

__示例 :__

![ring](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/ring.jpg)

输入: ring = "godding", key = "gd"
输出: 4
解释:
 对于 key 的第一个字符 'g'，已经在正确的位置, 我们只需要1步来拼写这个字符。
 对于 key 的第二个字符 'd'，我们需要逆时针旋转 ring "godding" 2步使它变成 "ddinggo"。
 当然, 我们还需要1步进行拼写。
 因此最终的输出是 4。

__提示:__

ring 和 key 的字符串长度取值范围均为 1 至 100；
两个字符串中都只有小写字符，并且均可能存在重复字符；
字符串 key 一定可以由字符串 ring 旋转拼出。

__思路__:

动态规划
用一个 pos数组记录 ring中的各个字母对应的下标, godding中的 g, 下标应该为 [0, 6], 即 pos['g'] = [0, 6]
dp[i][j]表示从 key[i - 1]出发到达 key[i]需要的步数, 其中上一步是 pos[key[i - 1]]中的某一个
dp[i][j] = min(for k in pos[key[i - 1]])(dp[i - 1][k] + min(abs(j - k), m - abs(j - k)) + 1)
abs(j - k)和 m - abs(j - k)分别表示从左边和从右边搜索下一个节点的长度
最后只要求得 min(dp[n - 1])即可
注意到 dp更新只取决于前一行, 可以将时间复杂度压缩到 O(m)
时间复杂度O(mn ^ 2), 空间复杂度O(m), 其中 m, n分别为 ring和 key的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findRotateSteps(string ring, string key) 
    {
        int m = ring.size(), n = key.size();
        vector<int> pos[26], dp(m, 0x3f3f3f3f);
        for (int i = 0; i < m; i++) pos[ring[i] - 'a'].push_back(i);
        for (auto& i : pos[key[0] - 'a']) dp[i] = min(i, m - i) + 1;
        for (int i = 1; i < n; i++) 
        {
            vector<int> cur(m, 0x3f3f3f3f);
            for (auto& j : pos[key[i] - 'a']) for (auto& k : pos[key[i - 1] - 'a']) cur[j] = min(cur[j], dp[k] + min(abs(j - k), m - abs(j - k)) + 1);
            dp = cur;
        }
        return *min_element(dp.begin(), dp.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int findRotateSteps(String ring, String key) {
        int m = ring.length(), n = key.length(), result = 0x3f3f3f3f;
        List<List<Integer>> pos = new ArrayList<>();
        for (int i = 0; i < 26; i++) pos.add(new ArrayList<Integer>());
        for (int i = 0; i < m; i++) pos.get(ring.charAt(i) - 'a').add(i);
        int dp[][] = new int[n][m];
        for (int i = 0; i < n; i++) for (int j = 0; j < m; j++) dp[i][j] = 0x3f3f3f3f;
        for (int i: pos.get(key.charAt(0) - 'a')) dp[0][i] = Math.min(i, m - i) + 1;
        for (int i = 1; i < n; ++i) for (int j: pos.get(key.charAt(i) - 'a')) for (int k: pos.get(key.charAt(i - 1) - 'a')) dp[i][j] = Math.min(dp[i][j], dp[i - 1][k] + Math.min(Math.abs(j - k), m - Math.abs(j - k)) + 1);
        for (int i : dp[n - 1]) result = Math.min(result, i);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findRotateSteps(self, ring: str, key: str) -> int:
        pos, n, dp = defaultdict(list), len(ring), [(0,0)]
        for i, c in enumerate(ring):
            pos[c].append(i)
        for c in key:
            dp = [min([(1 + pre_cost + min(abs(i - pre_pos), n - abs(i - pre_pos)), i) for pre_cost, pre_pos in dp]) for i in pos[c]]
        return min(dp)[0]
```
