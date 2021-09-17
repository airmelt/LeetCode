# 838 Push Dominoes 推多米诺

__Description__:
There are n dominoes in a line, and we place each domino vertically upright. In the beginning, we simultaneously push some of the dominoes either to the left or to the right.

After each second, each domino that is falling to the left pushes the adjacent domino on the left. Similarly, the dominoes falling to the right push their adjacent dominoes standing on the right.

When a vertical domino has dominoes falling on it from both sides, it stays still due to the balance of the forces.

For the purposes of this question, we will consider that a falling domino expends no additional force to a falling or already fallen domino.

You are given a string dominoes representing the initial state where:

dominoes[i] = 'L', if the ith domino has been pushed to the left,
dominoes[i] = 'R', if the ith domino has been pushed to the right, and
dominoes[i] = '.', if the ith domino has not been pushed.
Return a string representing the final state.

__Example:__

Example 1:

Input: dominoes = "RR.L"
Output: "RR.L"
Explanation: The first domino expends no additional force on the second domino.

Example 2:

![domino](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/05/18/domino.png)

Input: dominoes = ".L.R...LR..L.."
Output: "LL.RR.LLRRLL.."

__Constraints:__

n == dominoes.length
1 <= n <= 10^5
dominoes[i] is either 'L', 'R', or '.'.

__题目描述__:
一行中有 N 张多米诺骨牌，我们将每张多米诺骨牌垂直竖立。

在开始时，我们同时把一些多米诺骨牌向左或向右推。

![多米诺](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/05/19/domino.png)

每过一秒，倒向左边的多米诺骨牌会推动其左侧相邻的多米诺骨牌。

同样地，倒向右边的多米诺骨牌也会推动竖立在其右侧的相邻多米诺骨牌。

如果同时有多米诺骨牌落在一张垂直竖立的多米诺骨牌的两边，由于受力平衡， 该骨牌仍然保持不变。

就这个问题而言，我们会认为正在下降的多米诺骨牌不会对其它正在下降或已经下降的多米诺骨牌施加额外的力。

给定表示初始状态的字符串 "S" 。如果第 i 张多米诺骨牌被推向左边，则 S[i] = 'L'；如果第 i 张多米诺骨牌被推向右边，则 S[i] = 'R'；如果第 i 张多米诺骨牌没有被推动，则 S[i] = '.'。

返回表示最终状态的字符串。

__示例 :__

示例 1：

输入：".L.R...LR..L.."
输出："LL.RR.LLRRLL.."

示例 2：

输入："RR.L"
输出："RR.L"
说明：第一张多米诺骨牌没有给第二张施加额外的力。

__提示:__

0 <= N <= 10^5
表示多米诺骨牌状态的字符串只含有 'L'，'R'; 以及 '.';

__思路__:

受力分析
求每个板到相邻两个块的距离, 距离近的会被影响
从前往后遍历设 'R' 的力为 n, 'L' 的力为 0, 求出每块板的力
从后往前遍历设 'R' 的力为 0, 'L' 的力为 n, 求出每块板的力
求出合力, 大于 0 的为 'R', 小于 0 的为 'L', 等于 0 的为 '.'
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string pushDominoes(string dominoes) 
    {
        int n = dominoes.size();
        vector<int> force(n);
        for (int i = 0, f = 0; i < n; i++) 
        {
            f = dominoes[i] == 'R' ? n : (dominoes[i] == 'L' ? 0 : max(f - 1, 0));
            force[i] += f;
        }
        for (int i = n - 1, f = 0; i > -1; i--) {
            f = dominoes[i] == 'L' ? n : (dominoes[i] == 'R' ? 0 : max(f - 1, 0));
            force[i] -= f;
        }
        for (int i = 0; i < n; i++) dominoes[i] = force[i] > 0 ? 'R' : (force[i] < 0 ? 'L' : '.'); 
        return dominoes;
    }
};
```

__Java__:

```Java
class Solution {
    public String pushDominoes(String dominoes) {
        int n = dominoes.length(), force[] = new int[n];
        for (int i = 0, f = 0; i < n; i++) {
            f = dominoes.charAt(i) == 'R' ? n : (dominoes.charAt(i) == 'L' ? 0 : Math.max(f - 1, 0));
            force[i] += f;
        }
        for (int i = n - 1, f = 0; i > -1; i--) {
            f = dominoes.charAt(i) == 'L' ? n : (dominoes.charAt(i) == 'R' ? 0 : Math.max(f - 1, 0));
            force[i] -= f;
        }
        StringBuilder sb = new StringBuilder(dominoes);
        for (int i = 0; i < n; i++) sb.setCharAt(i, force[i] > 0 ? 'R' : (force[i] < 0 ? 'L' : '.')); 
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def pushDominoes(self, dominoes: str) -> str:
        force = [(f := 0)] * (n := len(dominoes))
        for i in range(n):
            f = n if dominoes[i] == 'R' else 0 if dominoes[i] == 'L' else max(f - 1, 0)
            force[i] += f
        f = 0
        for i in range(n - 1, -1, -1):
            f = n if dominoes[i] == 'L' else 0 if dominoes[i] == 'R' else max(f - 1, 0)
            force[i] -= f
        return ''.join('R' if f > 0 else 'L' if f < 0 else '.' for f in force)
```
