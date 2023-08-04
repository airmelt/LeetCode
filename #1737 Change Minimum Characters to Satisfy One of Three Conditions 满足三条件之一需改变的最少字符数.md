# 1737 Change Minimum Characters to Satisfy One of Three Conditions 满足三条件之一需改变的最少字符数

__Description:__

You are given two strings `a` and `b` that consist of lowercase letters. In one operation, you can change any character in `a` or `b` to __any lowercase letter__.

Your goal is to satisfy one of the following three conditions:

- __Every__ letter in `a` is __strictly less__ than __every__ letter in `b` in the alphabet.
- __Every__ letter in `b` is __strictly less__ than __every__ letter in `a` in the alphabet.
- __Both__ `a` and `b` consist of __only one__ distinct letter.

Return the minimum number of operations needed to achieve your goal.

__Example:__

Example 1:

```text
Input: a = "aba", b = "caa"
Output: 2
Explanation: Consider the best way to make each condition true:
1) Change b to "ccc" in 2 operations, then every letter in a is less than every letter in b.
2) Change a to "bbb" and b to "aaa" in 3 operations, then every letter in b is less than every letter in a.
3) Change a to "aaa" and b to "aaa" in 2 operations, then a and b consist of one distinct letter.
The best way was done in 2 operations (either condition 1 or condition 3).
```

Example 2:

```text
Input: a = "dabadd", b = "cda"
Output: 3
Explanation: The best way is to make condition 1 true by changing b to "eee".
```

__Constraints:__

- `1 <= a.length, b.length <= 10 ^ 5`
- `a` and `b` consist only of lowercase letters.

__题目描述:__

给你两个字符串 `a` 和 `b` ，二者均由小写字母组成。一步操作中，你可以将 `a` 或 `b` 中的 __任一字符__ 改变为 __任一小写字母__ 。

操作的最终目标是满足下列三个条件 之一 ：

- `a` 中的 __每个字母__ 在字母表中 __严格小于__ `b` 中的 __每个字母__ 。
- `b` 中的 __每个字母__ 在字母表中 __严格小于__ `a` 中的 __每个字母__ 。
- `a` 和 `b` __都__ 由 __同一个__ 字母组成。

返回达成目标所需的 最少 操作数。

__示例:__

示例 1：

```text
输入：a = "aba", b = "caa"
输出：2
解释：满足每个条件的最佳方案分别是：
1) 将 b 变为 "ccc"，2 次操作，满足 a 中的每个字母都小于 b 中的每个字母；
2) 将 a 变为 "bbb" 并将 b 变为 "aaa"，3 次操作，满足 b 中的每个字母都小于 a 中的每个字母；
3) 将 a 变为 "aaa" 并将 b 变为 "aaa"，2 次操作，满足 a 和 b 由同一个字母组成。
最佳的方案只需要 2 次操作（满足条件 1 或者条件 3）。
```

示例 2：

```text
输入：a = "dabadd", b = "cda"
输出：3
解释：满足条件 1 的最佳方案是将 b 变为 "eee" 。
```

__提示：__

- `1 <= a.length, b.length <= 10 ^ 5`
- `a` 和 `b` 只由小写字母组成

__思路:__

```text
贪心
枚举每个字符 i
对于条件 1, 只需要统计 a 中大于等于 i 的字符数和 b 中小于 i 的字符数, 两者相加即为改变的字符数
条件 2 与条件 1 类似
条件 3 即 m + n - max_value[i]
枚举所有字符中的最小值即可
时间复杂度为 O(M + N), 空间复杂度为 O(1), 只需要统计 26 个字符的个数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minCharacters(string a, string b) 
    {
        int m = a.size(), n = b.size();
        vector<int> c1(26), c2(26);
        for (const auto& c : a) ++c1[c - 'a'];
        for (const auto& c : b) ++c2[c - 'a'];
        int result = m + n - c1.front() - c2.front();
        for (int i = 1; i < 26; i++)
        {
            result = min(result, m + n - c1[i] - c2[i]);
            int r1 = 0, r2 = 0;
            for (int j = i; j < 26; j++) r1 += c1[j];
            for (int j = 0; j < i; j++) r1 += c2[j];
            for (int j = i; j < 26; j++) r2 += c2[j];
            for (int j = 0; j < i; j++) r2 += c1[j];
            result = min({result, r1, r2});
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minCharacters(String a, String b) {
        int m = a.length(), n = b.length(), c1[] = new int[26], c2[] = new int[26];
        for (char c : a.toCharArray()) ++c1[c - 'a'];
        for (char c : b.toCharArray()) ++c2[c - 'a'];
        int result = m + n - c1[0] - c2[0];
        for (int i = 1; i < 26; i++) {
            result = Math.min(result, m + n - c1[i] - c2[i]);
            int r1 = 0, r2 = 0;
            for (int j = i; j < 26; j++) r1 += c1[j];
            for (int j = 0; j < i; j++) r1 += c2[j];
            for (int j = i; j < 26; j++) r2 += c2[j];
            for (int j = 0; j < i; j++) r2 += c1[j];
            result = Math.min(result, Math.min(r1, r2));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution {
    public int minCharacters(String a, String b) {
        int m = a.length(), n = b.length(), c1[] = new int[26], c2[] = new int[26];
        for (char c : a.toCharArray()) ++c1[c - 'a'];
        for (char c : b.toCharArray()) ++c2[c - 'a'];
        int result = m + n - c1[0] - c2[0];
        for (int i = 1; i < 26; i++) {
            result = Math.min(result, m + n - c1[i] - c2[i]);
            int r1 = 0, r2 = 0;
            for (int j = i; j < 26; j++) r1 += c1[j];
            for (int j = 0; j < i; j++) r1 += c2[j];
            for (int j = i; j < 26; j++) r2 += c2[j];
            for (int j = 0; j < i; j++) r2 += c1[j];
            result = Math.min(result, Math.min(r1, r2));
        }
        return result;
    }
}
```
