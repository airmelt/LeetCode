# 1156 Swap For Longest Repeated Character Substring 单字符重复子串的最大长度

__Description__:
You are given a string text. You can swap two of the characters in the text.

Return the length of the longest substring with repeated characters.

__Example:__

Example 1:

Input: text = "ababa"
Output: 3
Explanation: We can swap the first 'b' with the last 'a', or the last 'b' with the first 'a'. Then, the longest repeated character substring is "aaa" with length 3.

Example 2:

Input: text = "aaabaaa"
Output: 6
Explanation: Swap 'b' with the last 'a' (or the first 'a'), and we get longest repeated character substring "aaaaaa" with length 6.

Example 3:

Input: text = "aaaaa"
Output: 5
Explanation: No need to swap, longest repeated character substring is "aaaaa" with length is 5.

__Constraints:__

1 <= text.length <= 2 * 10^4
text consist of lowercase English characters only.

__题目描述__:
如果字符串中的所有字符都相同，那么这个字符串是单字符重复的字符串。

给你一个字符串 text，你只能交换其中两个字符一次或者什么都不做，然后得到一些单字符重复的子串。返回其中最长的子串的长度。

__示例 :__

示例 1：

输入：text = "ababa"
输出：3

示例 2：

输入：text = "aaabaaa"
输出：6

示例 3：

输入：text = "aaabbaaa"
输出：4

示例 4：

输入：text = "aaaaa"
输出：5

示例 5：

输入：text = "abcdef"
输出：1

__提示:__

1 <= text.length <= 20000
text 仅由小写英文字母组成。

__思路__:

模拟
只会出现 2 种情况
一段连续的加上其他地方的, 如 ...aaaa...a..., 可以移动之后形成 ...aaaaa..., 总数为连续最长的加 1
一段连续的中间只有一个其他字符, 如 ...aaabaaa...a..., 可以将 b 和 a 交换形成 ...aaaaaaa...b..., 总数为两端的总和加 1
先记录所有字符出现的次数
然后记录相同字符出现的长度
一边遍历一边更新结果
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxRepOpt1(string text) 
    {
        int n = text.size(), result = 0, idx = -1, nums[n];
        unordered_map<int, int> pos;
        for (const auto c : text) ++pos[c];
        for (int i = 0; i < n; i++)
        {
            int j = i;
            while (j < n and text[j] == text[i]) ++j;
            nums[++idx] = j - i;
            result = (idx > 1 and nums[idx - 1] == 1 and i > 1 and text[i - 2] == text[i]) ? max(result, min(nums[idx] + nums[idx - 2] + 1, pos[text[i]])) : max(result, min(nums[idx] + 1, pos[text[i]]));
            i = j - 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxRepOpt1(String text) {
        int n = text.length(), result = 0, idx = -1, nums[] = new int[n];
        Map<Character, Integer> pos = new HashMap<>();
        for (char c : text.toCharArray()) pos.put(c, pos.getOrDefault(c, 0) + 1);
        for (int i = 0; i < n; i++) {
            int j = i;
            while (j < n && text.charAt(j) == text.charAt(i)) ++j;
            nums[++idx] = j - i;
            result = (idx > 1 && nums[idx - 1] == 1 && i > 1 && text.charAt(i - 2) == text.charAt(i)) ? Math.max(result, Math.min(nums[idx] + nums[idx - 2] + 1, pos.get(text.charAt(i)))) : Math.max(result, Math.min(nums[idx] + 1, pos.get(text.charAt(i))));
            i = j - 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxRepOpt1(self, text: str) -> int:
        result, idx, nums, i, c = 0, -1, [0] * (n := len(text)), 0, Counter(text)
        while i < n:
            j = i
            while j < n and text[j] == text[i]:
                j += 1
            idx += 1
            nums[idx] = j - i
            result = max(result, min(nums[idx] + nums[idx - 2] + 1, c[text[i]])) if idx > 1 and nums[idx - 1] == 1 and i > 1 and text[i - 2] == text[i] else max(result, min(nums[idx] + 1, c[text[i]]))
            i = j
        return result
```
