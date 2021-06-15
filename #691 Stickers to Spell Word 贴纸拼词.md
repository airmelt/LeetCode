# 691 Stickers to Spell Word 贴纸拼词

__Description__:
We are given n different types of stickers. Each sticker has a lowercase English word on it.

You would like to spell out the given string target by cutting individual letters from your collection of stickers and rearranging them. You can use each sticker more than once if you want, and you have infinite quantities of each sticker.

Return the minimum number of stickers that you need to spell out target. If the task is impossible, return -1.

__Note:__
In all test cases, all words were chosen randomly from the 1000 most common US English words, and target was chosen as a concatenation of two random words.

__Example:__

Example 1:

Input: stickers = ["with","example","science"], target = "thehat"
Output: 3
Explanation:
We can use 2 "with" stickers, and 1 "example" sticker.
After cutting and rearrange the letters of those stickers, we can form the target "thehat".
Also, this is the minimum number of stickers necessary to form the target string.

Example 2:

Input: stickers = ["notice","possible"], target = "basicbasic"
Output: -1
Explanation:
We cannot form the target "basicbasic" from cutting letters from the given stickers.

__Constraints:__

n == stickers.length
1 <= n <= 50
1 <= stickers[i].length <= 10
1 <= target <= 15
stickers[i] and target consist of lowercase English letters.

__题目描述__:
我们给出了 N 种不同类型的贴纸。每个贴纸上都有一个小写的英文单词。

你希望从自己的贴纸集合中裁剪单个字母并重新排列它们，从而拼写出给定的目标字符串 target。

如果你愿意的话，你可以不止一次地使用每一张贴纸，而且每一张贴纸的数量都是无限的。

拼出目标 target 所需的最小贴纸数量是多少？如果任务不可能，则返回 -1。

__示例 :__

示例 1：

输入：

["with", "example", "science"], "thehat"
输出：

3
解释：

我们可以使用 2 个 "with" 贴纸，和 1 个 "example" 贴纸。
把贴纸上的字母剪下来并重新排列后，就可以形成目标 “thehat“ 了。
此外，这是形成目标字符串所需的最小贴纸数量。

示例 2：

输入：

["notice", "possible"], "basicbasic"
输出：

-1
解释：

我们不能通过剪切给定贴纸的字母来形成目标“basicbasic”。

__提示:__

stickers 长度范围是 [1, 50]。
stickers 由小写英文单词组成（不带撇号）。
target 的长度在 [1, 15] 范围内，由小写字母组成。
在所有的测试案例中，所有的单词都是从 1000 个最常见的美国英语单词中随机选取的，目标是两个随机单词的串联。
时间限制可能比平时更具挑战性。预计 50 个贴纸的测试案例平均可在35ms内解决。

__思路__:

状态压缩
因为 target 的长度为 15, 小于 20, 题目要求 target 各位字母是否存在, 考虑用状态压缩
每一位二进制数代表 target 各位是否存在
dp[i] = min(dp[i], dp[j] + 1), 其中 j 可以通过贴纸转化为 i
预处理用一个 count 数组记录贴纸的各字母个数
时间复杂度为 O(2 ^ n \* mn), 空间复杂度为 O(2 ^ n + ml), 其中 n 为 target 长度, m 为 stickers 长度, l 为 stickers 中各字符串长度

__代码__:
__C++__:

```C++
#define INF 0x3f3f3f3f

class Solution 
{
public:
    int minStickers(vector<string>& stickers, string target) 
    {
        int m = stickers.size(), n = target.size();    
        vector<int> dp(1 << 15, INF);
        dp.front() = 0;
        vector<vector<int>> count(m, vector<int>(26));
        for (int i = 0; i < m; i++) for (const auto &c : stickers[i]) ++count[i][c - 'a'];
        for (int i = 0; i < (1 << n); i++)
        {
            if (dp[i] == INF) continue;
            for (int k = 0; k < m; k++)
            {
                int next = i;
                vector<int> cur(count[k]);
                for (int j = 0; j < n; j++)
                {
                    if (next & (1 << j)) continue;
                    if (cur[target[j] - 'a'] > 0)
                    {
                        next |= (1 << j);
                        --cur[target[j] - 'a'];
                    }
                }
                dp[next] = min(dp[next], dp[i] + 1);
            }
        }
        return dp[(1 << n) - 1] == INF ? -1 : dp[(1 << n) - 1];
    }
};
```

__Java__:

```Java
class Solution {
    public int minStickers(String[] stickers, String target) {
        int m = stickers.length, n = target.length();
        int dp[] = new int[1 << n], count[][] = new int[m][26];
        Arrays.fill(dp,Integer.MAX_VALUE);
        List<LinkedList<Integer>> stickerWithChar = new ArrayList<LinkedList<Integer>>(26);
        for (int i = 0; i < 26; i++) stickerWithChar.add(new LinkedList<Integer>());
        for (int i = 0; i < m; i++) {
            for (char c : stickers[i].toCharArray()) {
                int pos = c - 'a';
                ++count[i][pos];
                if (stickerWithChar.get(pos).isEmpty() || stickerWithChar.get(pos).getLast() != i) stickerWithChar.get(pos).add(i);
            }
        }
        dp[0] = 0;
        for (int i = 0; i < (1 << n) - 1; i++) {
            if (dp[i] == Integer.MAX_VALUE) continue;
            int pos = 0;
            for (int j = 0; j < n; j++) {
                if ((i & (1 << j)) == 0) {
                    pos = j;
                    break;
                }
            }
            pos = target.charAt(pos) - 'a';
            for (int k : stickerWithChar.get(pos)) {
                int cur=i;
                int[] left = count[k].clone();
                for (int j = 0; j < n; j++) {
                    if ((cur & (1<<j)) != 0) continue;
                    if (left[target.charAt(j) - 'a'] > 0) {
                        --left[target.charAt(j) - 'a'];
                        cur |= (1 << j);
                    }
                }
                dp[cur] = Math.min(dp[cur], dp[i] + 1);
            }
        }
        return dp[(1 << n) - 1] == Integer.MAX_VALUE ? -1 : dp[(1 << n) - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def minStickers(self, stickers: List[str], target: str) -> int:
        stickers = [Counter(s) for s in stickers]
        @functools.lru_cache(None)
        def helper(target: str) -> int:
            if not target: 
                return 0
            result, count = float("inf"), Counter(target)
            for c in stickers:
                if not c[target[-1]]:
                    continue
                cur = helper("".join([s * t for (s, t) in (count - c).items()]))
                if cur != -1: 
                    result = min(result, cur + 1)
            return -1 if result == float("inf") else result
        return helper(target)
```
