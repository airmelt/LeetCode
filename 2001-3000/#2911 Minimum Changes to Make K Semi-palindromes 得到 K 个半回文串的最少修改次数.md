# 2911 Minimum Changes to Make K Semi-palindromes 得到 K 个半回文串的最少修改次数

__Description:__

Given a string `s` and an integer `k`, partition `s` into `k` __substrings__ such that the letter changes needed to make each substring a __semi-palindrome__ are minimized.

Return the minimum number of letter changes required.

A semi-palindrome is a special type of string that can be divided into palindromes based on a repeating pattern. To check if a string is a semi-palindrome:

Consider the string `"abcabc"`:

- The length of `"abcabc"` is `6`. Valid divisors are `1`, `2`, and `3`.
- For `d = 1`: The entire string `"abcabc"` forms one group. Not a palindrome.
- For `d = 2`:
  - Group 1 (positions `1, 3, 5`): `"acb"`
  - Group 2 (positions `2, 4, 6`): `"bac"`
  - Neither group forms a palindrome.
- For `d = 3`:
  - Group 1 (positions `1, 4`): `"aa"`
  - Group 2 (positions `2, 5`): `"bb"`
  - Group 3 (positions `3, 6`): `"cc"`
  - All groups form palindromes. Therefore, `"abcabc"` is a semi-palindrome.

__Example:__

Example 1:

```text
Input: s = "abcac", k = 2

Output: 1
```

__Explanation:__ Divide `s` into `"ab"` and `"cac"`. `"cac"` is already semi-palindrome. Change `"ab"` to `"aa"`, it becomes semi-palindrome with `d = 1`.

Example 2:

```text
Input: s = "abcdef", k = 2

Output: 2
```

__Explanation:__ Divide `s` into substrings `"abc"` and `"def"`. Each needs one change to become semi-palindrome.

Example 3:

```text
Input: s = "aabbaa", k = 3

Output: 0
```

__Explanation:__ Divide `s` into substrings `"aa"`, `"bb"` and `"aa"`. All are already semi-palindromes.

__Constraints:__

- `2 <= s.length <= 200`
- `1 <= k <= s.length / 2`
- `s` contains only lowercase English letters.

__题目描述:__

给你一个字符串 `s` 和一个整数 `k` ，请你将 `s` 分成 `k` 个 __子字符串__ ，使得每个 __子字符串__ 变成 __半回文串__ 需要修改的字符数目最少。

请你返回一个整数，表示需要修改的 最少 字符数目。

注意：

- 如果一个字符串从左往右和从右往左读是一样的，那么它是一个 __回文串__ 。
- 如果长度为 `len` 的字符串存在一个满足 `1 <= d < len` 的正整数 `d` ， `len % d == 0` 成立且所有对 `d` 做除法余数相同的下标对应的字符连起来得到的字符串都是 __回文串__ ，那么我们说这个字符串是 __半回文串__ 。比方说 `"aa"` ， `"aba"` ， `"adbgad"` 和 `"abab"` 都是 __半回文串__ ，而 `"a"` ， `"ab"` 和 `"abca"` 不是。
- __子字符串__ 指的是一个字符串中一段连续的字符序列。

__示例:__

示例 1：

```text
输入：s = "abcac", k = 2
输出：1
解释：我们可以将 s 分成子字符串 "ab" 和 "cac" 。子字符串 "cac" 已经是半回文串。如果我们将 "ab" 变成 "aa" ，它也会变成一个 d = 1 的半回文串。
该方案是将 s 分成 2 个子字符串的前提下，得到 2 个半回文子字符串需要的最少修改次数。所以答案为 1 。
```

示例 2:

```text
输入：s = "abcdef", k = 2
输出：2
解释：我们可以将 s 分成子字符串 "abc" 和 "def" 。子字符串 "abc" 和 "def" 都需要修改一个字符得到半回文串，所以我们总共需要 2 次字符修改使所有子字符串变成半回文串。
该方案是将 s 分成 2 个子字符串的前提下，得到 2 个半回文子字符串需要的最少修改次数。所以答案为 2 。
```

示例 3：

```text
输入：s = "aabbaa", k = 3
输出：0
解释：我们可以将 s 分成子字符串 "aa" ，"bb" 和 "aa" 。
字符串 "aa" 和 "bb" 都已经是半回文串了。所以答案为 0 。
```

__提示：__

- `2 <= s.length <= 200`
- `1 <= k <= s.length / 2`
- `s` 只包含小写英文字母。

__思路:__

```text
动态规划
由于字符串长度最多为 200 首先预处理 200 以内的数字的约数
比如 2 是 4, 6, 8, ..., 200 的真约数
将 2 分别放入 divisors[4], divisors[6], divisors[8], ... divisors[200]
预处理子串需要 modify[i][j] 次才能成为回文串
检查 len = j - i + 1 长度内的字符串
比如 len = 9, begin = 0, d = 3
那么 s[0] + s[3] + s[6], s[1] + s[4] + s[7], s[2] + s[5] + s[8] 都需要是回文串, 检查最少需要累计修改 cur 次
找到所有能整除 len 的 d 取最小的 cur 作为子串 s[i:j + 1] 的结果, 即 modify[i][j]
设 dp[i][j] 表示将 s[:j] 分割成 i + 1 个子串需要的最少修改数目
则初始值 dp[0][j] = modify[0][j]
最后结果返回 dp[k - 1][n - 1]
设 s[j] 对应的子串从 L 开始
那么 dp[i][j] = min(dp[i - 1][L - 1]) + modify[L][j]
其中 2 * i <= L < j, 因为子字符串长度最少为 2, 所以 L 最左边可以从 2 * i 开始, 右边还需要 k - i - 1 个字符串, 最多到 n - 1 - (k - i - 1) * 2
时间复杂度为 O(N ^ 3logN), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
const int MX = 201;
vector<int> divisors[MX];

int init = []
{
    for (int i = 1; i < MX; i++) for (int j = i << 1; j < MX; j += i) divisors[j].emplace_back(i);
    return 0;
}();

class Solution 
{
public:
    int minimumChanges(string s, int k) 
    {
        int n = s.size();
        vector<vector<int>> modify(n - 1, vector<int>(n));
        auto get_modify = [&](const auto& begin, const auto& len) -> int
        {
            int result = len, cur = 0;
            for (const auto& d: divisors[len]) 
            {
                cur = 0;
                for (int i0 = 0; i0 < d; i0++) for (int i = i0, j = len - d + i0; i < j; i += d, j -= d) cur += s[i + begin] != s[j + begin];
                result = min(result, cur);
            }
            return result;
        };
        for (int left = 0; left < n - 1; left++) for (int right = left + 1; right < n; right++) modify[left][right] = get_modify(left, right - left + 1);
        vector<int> dp(modify.front());
        for (int i = 1; i < k; i++) 
        {
            for (int j = n - 1 - ((k - 1 - i) << 1); j > i << 1; j--) 
            {
                dp[j] = INT_MAX;
                for (int L = i << 1; L < j; L++) dp[j] = min(dp[j], dp[L - 1] + modify[L][j]);
            }
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    private static final int MX = 201;
    private static final List<Integer>[] divisors = new ArrayList[MX];

    static {
        Arrays.setAll(divisors, k -> new ArrayList<>());
        for (int i = 1; i < MX; i++) for (int j = i << 1; j < MX; j += i) divisors[j].add(i);
    }

    public int minimumChanges(String s, int k) {
        char[] chars = s.toCharArray();
        int n = chars.length, modify[][] = new int[n - 1][n];
        for (int left = 0; left < n - 1; left++) for (int right = left + 1; right < n; right++) modify[left][right] = getModify(chars, left, right - left + 1);

        int[] dp = modify[0];
        for (int i = 1; i < k; i++) {
            for (int j = n - 1 - ((k - 1 - i) << 1); j > (i << 1); j--) {
                dp[j] = Integer.MAX_VALUE;
                for (int L = i << 1; L < j; L++) dp[j] = Math.min(dp[j], dp[L - 1] + modify[L][j]);
            }
        }
        return dp[n - 1];
    }

    private int getModify(char[] s, int begin, int len) {
        int result = len, cur = 0;
        for (int d : divisors[len]) {
            cur = 0;
            for (int i0 = 0; i0 < d; i0++) for (int i = i0, j = len - d + i0; i < j; i += d, j -= d) if (s[begin + i] != s[begin + j]) ++cur;
            result = Math.min(result, cur);
        }
        return result;
    }
}
```

__Python__:

```Python
MX = 201
divisors = [[] for _ in range(MX)]
for i in range(1, MX):
    for j in range(i << 1, MX, i):
        divisors[j].append(i)

class Solution:
    def minimumChanges(self, s: str, k: int) -> int:
        n = len(s)
        modify = [[0] * n for _ in range(n - 1)]

        def get_modify(s: str) -> int:
            result = n = len(s)
            for d in divisors[n]:
                cur = 0
                for i0 in range(d):
                    i, j = i0, n - d + i0
                    while i < j:
                        cur += s[i] != s[j]
                        i += d
                        j -= d
                result = min(result, cur)
            return result

        for left in range(n - 1):
            for right in range(left + 1, n):
                modify[left][right] = get_modify(s[left:right + 1])

        dp = modify[0]
        for i in range(1, k):
            for j in range(n - 1 - ((k - 1 - i) << 1), i << 1, -1): 
                dp[j] = min(dp[L - 1] + modify[L][j] for L in range(i << 1, j))
        return dp[-1]
```
