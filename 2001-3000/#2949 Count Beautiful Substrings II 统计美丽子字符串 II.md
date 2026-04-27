# 2949 Count Beautiful Substrings II 统计美丽子字符串 II

__Description:__

You are given a string `s` and a positive integer `k`.

Let `vowels` and `consonants` be the number of vowels and consonants in a string.

A string is beautiful if:

- `vowels == consonants`.
- `(vowels * consonants) % k == 0`, in other terms the multiplication of `vowels` and `consonants` is divisible by `k`.

Return _the number of __non-empty beautiful substrings__ in the given string_ `s`.

A substring is a contiguous sequence of characters in a string.

__Vowel letters__ in English are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

Consonant letters in English are every letter except vowels.

__Example:__

Example 1:

```text
Input: s = "baeyh", k = 2
Output: 2
Explanation: There are 2 beautiful substrings in the given string.
```

- Substring "baeyh", vowels = 2 (["a",e"]), consonants = 2 (["y","h"]).
You can see that string "aeyh" is beautiful as vowels == consonants and vowels * consonants % k == 0.
- Substring "baeyh", vowels = 2 (["a",e"]), consonants = 2 (["b","y"]).

You can see that string "baey" is beautiful as vowels == consonants and vowels * consonants % k == 0.

It can be shown that there are only 2 beautiful substrings in the given string.

Example 2:

```text
Input: s = "abba", k = 1
Output: 3
Explanation: There are 3 beautiful substrings in the given string.
```

- Substring "abba", vowels = 1 (["a"]), consonants = 1 (["b"]).
- Substring "abba", vowels = 1 (["a"]), consonants = 1 (["b"]).
- Substring "abba", vowels = 2 (["a","a"]), consonants = 2 (["b","b"]).

It can be shown that there are only 3 beautiful substrings in the given string.

Example 3:

```text
Input: s = "bcdf", k = 1
Output: 0
Explanation: There are no beautiful substrings in the given string.
```

__Constraints:__

- `1 <= s.length <= 5 * 10 ^ 4`
- `1 <= k <= 1000`
- `s` consists of only English lowercase letters.

__题目描述:__

给你一个字符串 `s` 和一个正整数 `k` 。

用 `vowels` 和 `consonants` 分别表示字符串中元音字母和辅音字母的数量。

如果某个字符串满足以下条件，则称其为 美丽字符串 ：

- `vowels == consonants`，即元音字母和辅音字母的数量相等。
- `(vowels * consonants) % k == 0`，即元音字母和辅音字母的数量的乘积能被 `k` 整除。

返回字符串 `s` 中 __非空美丽子字符串__ 的数量。

子字符串是字符串中的一个连续字符序列。

英语中的 __元音字母__ 为 `'a'`、 `'e'`、 `'i'`、 `'o'` 和 `'u'` 。

英语中的 辅音字母 为除了元音字母之外的所有字母。

__示例:__

示例 1：

```text
输入：s = "baeyh", k = 2
输出：2
解释：字符串 s 中有 2 个美丽子字符串。
```

- 子字符串 "baeyh"，vowels = 2（["a","e"]），consonants = 2（["y","h"]）。
可以看出字符串 "aeyh" 是美丽字符串，因为 vowels == consonants 且 vowels * consonants % k == 0 。
- 子字符串 "baeyh"，vowels = 2（["a","e"]），consonants = 2（["b","y"]）。

可以看出字符串 "baey" 是美丽字符串，因为 vowels == consonants 且 vowels * consonants % k == 0 。

可以证明字符串 s 中只有 2 个美丽子字符串。

示例 2：

```text
输入：s = "abba", k = 1
输出：3
解释：字符串 s 中有 3 个美丽子字符串。
```

- 子字符串 "abba"，vowels = 1（["a"]），consonants = 1（["b"]）。
- 子字符串 "abba"，vowels = 1（["a"]），consonants = 1（["b"]）。
- 子字符串 "abba"，vowels = 2（["a","a"]），consonants = 2（["b","b"]）。

可以证明字符串 s 中只有 3 个美丽子字符串。

示例 3：

```text
输入：s = "bcdf", k = 1
输出：0
解释：字符串 s 中没有美丽子字符串。
```

__提示：__

- `1 <= s.length <= 5 * 10 ^ 4`
- `1 <= k <= 1000`
- `s` 仅由小写英文字母组成。

__思路:__

```text
前缀和 ➕ 哈希表
由于只关系元音和辅音字母的个数
可以将元音和辅音看作 1 和 -1
对于长为 l 的字符串
若元音辅音个数相等且乘积模 k 为 0
则 ((l / 2) ^ 2) mod k == 0
即 l ^ 2 mod (4 * k) == 0
那么对 4 * k 进行质因数分解
l ^ 2 满足大于等于所有质因子 p ^ q 的乘积
所以 l 为所有 p ^ ((q + 1) / 2) 的乘积
比如 k = 3, 4 * k = 12, 12 = 2 ^ 2 * 3 ^ 1
所以 l = 2 ^ ((2 + 1) / 2) * 3 ^ ((1 + 1) / 2) = 2 * 3 = 6
6 * 6 = 36 能够整除 12
先将 4 * k 进行质因数分解得到 a
这里只关心前缀和模 a 的情况
为了使前缀和不小于 0 可以初始化前缀和 pre 为 n
将 d[-1] = 1 存入哈希表, 这里由于 a - 1 和 -1 同余 k 所以可以存入 a - 1
然后检查每个字符是否在 'aeiou' 中
如果在前缀和 pre 加上 1 否则减去 1
如果前缀和在哈希表中出现过
那么对于这一段子字符串就有 sum[i:j] = 0, 如果模 a 的值也想等
那么就可以累加到结果中
时间复杂度为 O(N + K ^ 1 / 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long beautifulSubstrings(string s, int k) 
    {
        auto helper = [](int a) -> int
        {
            int result = 1;
            for (int i = 2, i2 = i * i; i2 <= a; i++, i2 = i * i)
            {
                while (!(a % i2))
                {
                    result *= i;
                    a /= i2;
                }
                if (!(a % i))
                {
                    result *= i;
                    a /= i;
                }
            }
            return a > 1 ? result * a : result;
        };
        int a = helper(k << 2), n = s.size(), AEIOU_MASK = (1 << ('a' - 'a')) + (1 << ('e' - 'a')) + (1 << ('i' - 'a')) + (1 << ('o' - 'a')) + (1 << ('u' - 'a')), pre = n;
        unordered_map<int, int> m;
        m[(a - 1) << 17 | pre] = 1;
        long long result = 0LL;
        for (int i = 0; i < n; i++) result += m[(i % a) << 17 | (pre += ((AEIOU_MASK >> (s[i] - 'a') & 1) << 1) - 1)]++;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long beautifulSubstrings(String s, int k) {
        int a = helper(k << 2), AEIOU_MASK = (1 << ('a' - 'a')) + (1 << ('e' - 'a')) + (1 << ('i' - 'a')) + (1 << ('o' - 'a')) + (1 << ('u' - 'a')), n = s.length(), pre = n;
        var map = new HashMap<Integer, Integer>();
        map.put((a - 1) << 17 | pre, 1);
        long result = 0L;
        for (int i = 0; i < n; i++) result += map.merge((i % a) << 17 | (pre += ((AEIOU_MASK >> (s.charAt(i) - 'a') & 1) << 1) - 1), 1, Integer::sum) - 1;
        return result;
    }

    private int helper(int a) {
        int result = 1;
        for (int i = 2, i2 = i * i; i2 <= a; i++, i2 = i * i) {
            while (a % i2 == 0) {
                result *= i;
                a /= i2;
            }
            if (a % i == 0) {
                result *= i;
                a /= i;
            }
        }
        return a > 1 ? result * a : result;
    }
}
```

__Python__:

```Python
class Solution:
    def beautifulSubstrings(self, s: str, k: int) -> int:
        def helper(a: int) -> int:
            result, i = 1, 2
            while i * i <= a:
                while not a % i ** 2:
                    result *= i
                    a //= i ** 2
                if not a % i:
                    result *= i
                    a //= i
                i += 1
            if i > 1:
                result *= a
            return result
        result, d, pre = 0, Counter([((k := helper(k << 2)) - 1, 0)]), 0
        for i, c in enumerate(s):
            pre += 1 if c in 'aeiou' else -1
            result += d[p := (i % k, pre)]
            d[p] += 1
        return result
```
