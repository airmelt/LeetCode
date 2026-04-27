# 1320 Minimum Distance to Type a Word Using Two Fingers 二指输入的的最小距离

__Description:__

[图片上传失败...(image-8a268-1663948138656)]

You have a keyboard layout as shown above in the X-Y plane, where each English uppercase letter is located at some coordinate.

For example, the letter 'A' is located at coordinate (0, 0), the letter 'B' is located at coordinate (0, 1), the letter 'P' is located at coordinate (2, 3) and the letter 'Z' is located at coordinate (4, 1).
Given the string word, return the minimum total distance to type such string using only two fingers.

The distance between coordinates (x1, y1) and (x2, y2) is |x1 - x2| + |y1 - y2|.

Note that the initial positions of your two fingers are considered free so do not count towards your total distance, also your two fingers do not have to start at the first letter or the first two letters.

__Example:__

Example 1:

Input: word = "CAKE"
Output: 3
Explanation: Using two fingers, one optimal way to type "CAKE" is:
Finger 1 on letter 'C' -> cost = 0
Finger 1 on letter 'A' -> cost = Distance from letter 'C' to letter 'A' = 2
Finger 2 on letter 'K' -> cost = 0
Finger 2 on letter 'E' -> cost = Distance from letter 'K' to letter 'E' = 1
Total distance = 3

Example 2:

Input: word = "HAPPY"
Output: 6
Explanation: Using two fingers, one optimal way to type "HAPPY" is:
Finger 1 on letter 'H' -> cost = 0
Finger 1 on letter 'A' -> cost = Distance from letter 'H' to letter 'A' = 2
Finger 2 on letter 'P' -> cost = 0
Finger 2 on letter 'P' -> cost = Distance from letter 'P' to letter 'P' = 0
Finger 1 on letter 'Y' -> cost = Distance from letter 'A' to letter 'Y' = 4
Total distance = 6

__Constraints:__

2 <= word.length <= 300
word consists of uppercase English letters.

__题目描述：__

[图片上传失败...(image-a4743b-1663948138656)]

二指输入法定制键盘在 X-Y 平面上的布局如上图所示，其中每个大写英文字母都位于某个坐标处。

例如字母 A 位于坐标 (0,0)，字母 B 位于坐标 (0,1)，字母 P 位于坐标 (2,3) 且字母 Z 位于坐标 (4,1)。
给你一个待输入字符串 word，请你计算并返回在仅使用两根手指的情况下，键入该字符串需要的最小移动总距离。

坐标 (x1,y1) 和 (x2,y2) 之间的 距离 是 |x1 - x2| + |y1 - y2|。

注意，两根手指的起始位置是零代价的，不计入移动总距离。你的两根手指的起始位置也不必从首字母或者前两个字母开始。

__示例：__

示例 1：

输入：word = "CAKE"
输出：3
解释：
使用两根手指输入 "CAKE" 的最佳方案之一是：
手指 1 在字母 'C' 上 -> 移动距离 = 0
手指 1 在字母 'A' 上 -> 移动距离 = 从字母 'C' 到字母 'A' 的距离 = 2
手指 2 在字母 'K' 上 -> 移动距离 = 0
手指 2 在字母 'E' 上 -> 移动距离 = 从字母 'K' 到字母 'E' 的距离  = 1
总距离 = 3

示例 2：

输入：word = "HAPPY"
输出：6
解释：
使用两根手指输入 "HAPPY" 的最佳方案之一是：
手指 1 在字母 'H' 上 -> 移动距离 = 0
手指 1 在字母 'A' 上 -> 移动距离 = 从字母 'H' 到字母 'A' 的距离 = 2
手指 2 在字母 'P' 上 -> 移动距离 = 0
手指 2 在字母 'P' 上 -> 移动距离 = 从字母 'P' 到字母 'P' 的距离 = 0
手指 1 在字母 'Y' 上 -> 移动距离 = 从字母 'A' 到字母 'Y' 的距离 = 4
总距离 = 6

__提示：__

2 <= word.length <= 300
每个 word[i] 都是一个大写英文字母。

__思路：__

动态规划
设 dp\[i][l][r] 表示输入第 i 个字符时, 左手在 l 字符处, 右手在 r 字符处的最小移动距离
输入要么是左手输入 word[i] == l, 要么是右手输入 word[i] == r
而从上一次 dp\[i - 1][pre_l][pre_r] 转移到 dp\[i][l][r] 可分为 4 种情况
word[i - 1] == pre_l, word[i] == l, dp\[i][l][r] = dp\[i - 1][word[i - 1]][r] + dist(word[i - 1], word[i])
word[i - 1] == pre_r, word[i] == l, dp\[i][l][r] = min(dp\[i - 1][c][word[i - 1]] + dist(c, word[i])), 上一个左手的位置可为任意位置
word[i - 1] == pre_r, word[i] == r, dp\[i][l][r] = dp\[i - 1][l][word[i - 1]] + dist(word[i - 1], word[i])
word[i - 1] == pre_l, word[i] == r, dp\[i][l][r] = min(dp\[i - 1][word[i - 1]][c] + dist(c, word[i])), 上一个右手的位置可为任意位置
注意到 dp\[i] 只由 dp\[i - 1] 决定, 可将空间复杂度优化为 O(26n)
时间复杂度为 O(26n), 空间复杂度为 O(26n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int minimumDistance(string word) 
    {
        int n = word.size();
        vector<vector<int>> dp(n, vector<int>(26, 0x3f3f3f3f));
        for (int i = 0; i < 26; i++) dp.front()[i] = 0;
        for (int i = 1; i < n; i++) 
        {
            for (int cur = word[i] - 'A', prev = word[i - 1] - 'A', d = abs(cur / 6 - prev / 6) + abs(cur % 6 - prev % 6), j = 0; j < 26; j++)
            {
                dp[i][j] = min(dp[i][j], dp[i - 1][j] + d);
                if (prev == j) for (int k = 0; k < 26; k++) dp[i][j] = min(dp[i][j], dp[i - 1][k] + abs(k / 6 - cur / 6) + abs(k % 6 - cur % 6));
            }
        }
        return *min_element(dp.back().begin(), dp.back().end());
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumDistance(String word) {
        int n = word.length(), result = 0x3f3f3f3f, dp[][] = new int[n][26];
        for (int i = 1; i < n; i++) Arrays.fill(dp[i], result);
        for (int i = 1; i < n; i++) {
            for (int cur = word.charAt(i) - 'A', prev = word.charAt(i - 1) - 'A', d = Math.abs(cur / 6 - prev / 6) + Math.abs(cur % 6 - prev % 6), j = 0; j < 26; j++) {
                dp[i][j] = Math.min(dp[i][j], dp[i - 1][j] + d);
                if (prev == j) for (int k = 0; k < 26; k++) dp[i][j] = Math.min(dp[i][j], dp[i - 1][k] + Math.abs(k / 6 - cur / 6) + Math.abs(k % 6 - cur % 6));
            }
        }
        for (int i = 0; i < 26; i++) result = Math.min(result, dp[n - 1][i]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumDistance(self, word: str) -> int:
        l, r = word[0], {chr(i): 0 for i in range(65, 65 + 26)}
        for c in word[1:]:
            d = abs((a := [(ord(l) - 65) // 6, (ord(l) - 65) % 6])[0] - (b := [(ord(c) - 65) // 6, (ord(c) - 65) % 6])[0]) + abs(a[1] - b[1])
            next_r = {chr(i): r[chr(i)] + d for i in range(65, 65 + 26)}
            next_r[l] = min(next_r[l], min(abs((a := [(ord(k) - 65) // 6, (ord(k) - 65) % 6])[0] - b[0]) + abs(a[1] - b[1]) + r[k] for k in r))
            l, r = c, next_r
        return min(r.values())
```
