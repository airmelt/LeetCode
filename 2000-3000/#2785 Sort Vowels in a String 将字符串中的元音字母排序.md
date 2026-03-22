# 2785 Sort Vowels in a String 将字符串中的元音字母排序

__Description:__

Given a __0-indexed__ string `s`, __permute__ `s` to get a new string `t` such that:

- All consonants remain in their original places. More formally, if there is an index `i` with `0 <= i < s.length` such that `s[i]` is a consonant, then `t[i] = s[i]`.
- The vowels must be sorted in the __nondecreasing__ order of their __ASCII__ values. More formally, for pairs of indices `i`, `j` with `0 <= i < j < s.length` such that `s[i]` and `s[j]` are vowels, then `t[i]` must not have a higher ASCII value than `t[j]`.

Return the resulting string.

The vowels are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`, and they can appear in lowercase or uppercase. Consonants comprise all letters that are not vowels.

__Example:__

Example 1:

```text
Input: s = "lEetcOde"
Output: "lEOtcede"
Explanation: 'E', 'O', and 'e' are the vowels in s; 'l', 't', 'c', and 'd' are all consonants. The vowels are sorted according to their ASCII values, and the consonants remain in the same places.
```

Example 2:

```text
Input: s = "lYmpH"
Output: "lYmpH"
Explanation: There are no vowels in s (all characters in s are consonants), so we return "lYmpH".
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists only of letters of the English alphabet in __uppercase and lowercase__.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `s` ，将 `s` 中的元素重新 _排列_ 得到新的字符串 `t` ，它满足:

- 所有辅音字母都在原来的位置上。更正式的，如果满足 `0 <= i < s.length` 的下标 `i` 处的 `s[i]` 是个辅音字母，那么 `t[i] = s[i]` 。
- 元音字母都必须以他们的 __ASCII__ 值按 __非递减__ 顺序排列。更正式的，对于满足 `0 <= i < j < s.length` 的下标 `i` 和 `j` ，如果 `s[i]` 和 `s[j]` 都是元音字母，那么 `t[i]` 的 ASCII 值不能大于 `t[j]` 的 ASCII 值。

请你返回结果字母串。

元音字母为 `'a'` ， `'e'` ， `'i'` ， `'o'` 和 `'u'` ，它们可能是小写字母也可能是大写字母，辅音字母是除了这 5 个字母以外的所有字母。

__示例:__

示例 1：

```text
输入：s = "lEetcOde"
输出："lEOtcede"
解释：'E' ，'O' 和 'e' 是 s 中的元音字母，'l' ，'t' ，'c' 和 'd' 是所有的辅音。将元音字母按照 ASCII 值排序，辅音字母留在原地。
```

示例 2：

```text
输入：s = "lYmpH"
输出："lYmpH"
解释：s 中没有元音字母（s 中都为辅音字母），所以我们返回 "lYmpH" 。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 只包含英语字母表中的 __大写__ 和 __小写__ 字母。

__思路:__

```text
1. 排序
拿出所有的元音字母并排序
然后按照顺序插入到结果中
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
2. 计数排序
由于只有 128 种 ASCII 码
可以分别记录每种字母出现的次数
当当前字符为辅音字母时不做操作
如果当前字符为元音字母则按照出现次数数组找到第一个不为 0 的元素插入到结果中
时间复杂度为 O(N), 空间复杂度为 O(1), 实际上需要常数空间, 最少为 10
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string sortVowels(string s) 
    {
        int VOWEL_MASK = 0x208222, c['u' + 1]{}, j = 'A';
        for (const auto& ch : s) if (VOWEL_MASK >> (ch & 31) & 1) ++c[ch];
        for (auto& ch : s) 
        {
            if ((VOWEL_MASK >> (ch & 31) & 1)) 
            {
                while (!c[j]) j = j == 'Z' ? 'a' : j + 1;
                ch = (char)j;
                --c[j];
            }
        }
        return s;
    }
};
```

__Java__:

```Java
class Solution {
    public String sortVowels(String s) {
        int n = s.length(), MASK = 0x208222, k = 0, vowels[] = new int[n], i = 0;
        char[] result = s.toCharArray();
        for (var c : result) if ((MASK >> (c & 31) & 1) > 0) vowels[k++] = c;
        Arrays.sort(vowels, 0, k);
        for (k = 0; i < n; i++) if ((MASK >> (result[i] & 31) & 1) > 0) result[i] = (char)vowels[k++];
        return new String(result);
    }
}
```

__Python__:

```Python
class Solution:
    def sortVowels(self, s: str) -> str:
        return ''.join(s1.pop() if c in "AEIOUaeiou" else c for c in s) if (s1 := sorted([c for c in s if c in "AEIOUaeiou"], reverse=True)) else s
```
