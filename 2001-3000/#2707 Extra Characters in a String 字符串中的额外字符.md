# 2707 Extra Characters in a String 字符串中的额外字符

__Description:__

You are given a __0-indexed__ string `s` and a dictionary of words `dictionary`. You have to break `s` into one or more __non-overlapping__ substrings such that each substring is present in `dictionary`. There may be some __extra characters__ in `s` which are not present in any of the substrings.

Return _the __minimum__ number of extra characters left over if you break up_ `s` _optimally._

__Example:__

Example 1:

```text
Input: s = "leetscode", dictionary = ["leet","code","leetcode"]
Output: 1
Explanation: We can break s in two substrings: "leet" from index 0 to 3 and "code" from index 5 to 8. There is only 1 unused character (at index 4), so we return 1.
```

Example 2:

```text
Input: s = "sayhelloworld", dictionary = ["hello","world"]
Output: 3
Explanation: We can break s in two substrings: "hello" from index 3 to 7 and "world" from index 8 to 12. The characters at indices 0, 1, 2 are not used in any substring and thus are considered as extra characters. Hence, we return 3.
```

__Constraints:__

- `1 <= s.length <= 50`
- `1 <= dictionary.length <= 50`
- `1 <= dictionary[i].length <= 50`
- `dictionary[i]` and `s` consists of only lowercase English letters
- `dictionary` contains distinct words

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `s` 和一个单词字典 `dictionary` 。你需要将 `s` 分割成若干个 __互不重叠__ 的子字符串，每个子字符串都在 `dictionary` 中出现过。 `s` 中可能会有一些 __额外的字符__ 不在任何子字符串中。

请你采取最优策略分割 `s` ，使剩下的字符 __最少__ 。

__示例:__

示例 1：

```text
输入：s = "leetscode", dictionary = ["leet","code","leetcode"]
输出：1
解释：将 s 分成两个子字符串：下标从 0 到 3 的 "leet" 和下标从 5 到 8 的 "code" 。只有 1 个字符没有使用（下标为 4），所以我们返回 1 。
```

示例 2：

```text
输入：s = "sayhelloworld", dictionary = ["hello","world"]
输出：3
解释：将 s 分成两个子字符串：下标从 3 到 7 的 "hello" 和下标从 8 到 12 的 "world" 。下标为 0 ，1 和 2 的字符没有使用，所以我们返回 3 。
```

__提示：__

- `1 <= s.length <= 50`
- `1 <= dictionary.length <= 50`
- `1 <= dictionary[i].length <= 50`
- `dictionary[i]` 和 `s` 只包含小写英文字母。
- `dictionary` 中的单词互不相同。

__思路:__

```text
动态规划
设 dp[i + 1] 表示 s[:i] 分割后剩下的字符数
dp[0] 初始化为 0, 表示当 s 为空字符串时剩下 0 个字符
则 dp[i + 1] 最多为 dp[i] + 1, 即上一个字符的结果加上当前字符
如果 s[j:i + 1] 在字典中, 则 s[j:i + 1] 可以被删去
dp[i + 1] = min(dp[i + 1], dp[j]), 直接删除到 j 位置
最后返回 dp[n]
为了加快查找速度, 可以用集合记录字典中的字符串
时间复杂度为 O(M + N ^ 3), 空间复杂度为 O(M + N), M 为字典中的字符数, 每次处理需要二重循环遍历字符串, 还需要检查子串是否在字典中
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minExtraChar(string s, vector<string>& dictionary) 
    {
        int n = s.size();
        vector<int> dp(n + 1);
        unordered_set<string> d(dictionary.begin(), dictionary.end());
        for (int i = 0; i < n; i++) 
        {
            dp[i + 1] = dp[i] + 1;
            for (int j = 0; j <= i; j++) if (d.count(s.substr(j, i - j + 1))) dp[i + 1] = min(dp[i + 1], dp[j]);
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minExtraChar(String s, String[] dictionary) {
        int n = s.length(), dp[] = new int[n + 1];
        Set<String> d = new HashSet<>(Arrays.asList(dictionary));
        for (int i = 0; i < n; i++) {
            dp[i + 1] = dp[i] + 1;
            for (int j = 0; j <= i; j++) if (d.contains(s.substring(j, i + 1))) dp[i + 1] = Math.min(dp[i + 1], dp[j]);
        }
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def minExtraChar(self, s: str, dictionary: List[str]) -> int:
        d, dp = set(dictionary), [0] * ((n := len(s)) + 1)
        for i in range(n):
            dp[i + 1] = dp[i] + 1
            for j in range(i + 1):
                if s[j:i + 1] in d:
                    dp[i + 1] = min(dp[j], dp[i + 1])
        return dp[n]
```
