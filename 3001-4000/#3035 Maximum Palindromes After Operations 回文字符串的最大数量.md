# 3035 Maximum Palindromes After Operations 回文字符串的最大数量

__Description:__

You are given a __0-indexed__ string array `words` having length `n` and containing __0-indexed__ strings.

You are allowed to perform the following operation any number of times (including zero):

- Choose integers `i`, `j`, `x`, and `y` such that `0 <= i, j < n`, `0 <= x < words[i].length`, `0 <= y < words[j].length`, and __swap__ the characters `words[i][x]` and `words[j][y]`.

Return _an integer denoting the __maximum__ number of palindromes_ `words` _can contain, after performing some operations._

__Note:__ `i` and `j` may be equal during an operation.

__Example:__

Example 1:

```text
Input: words = ["abbb","ba","aa"]
Output: 3
Explanation: In this example, one way to get the maximum number of palindromes is:
Choose i = 0, j = 1, x = 0, y = 0, so we swap words[0][0] and words[1][0]. words becomes ["bbbb","aa","aa"].
All strings in words are now palindromes.
Hence, the maximum number of palindromes achievable is 3.
```

Example 2:

```text
Input: words = ["abc","ab"]
Output: 2
Explanation: In this example, one way to get the maximum number of palindromes is: 
Choose i = 0, j = 1, x = 1, y = 0, so we swap words[0][1] and words[1][0]. words becomes ["aac","bb"].
Choose i = 0, j = 0, x = 1, y = 2, so we swap words[0][1] and words[0][2]. words becomes ["aca","bb"].
Both strings are now palindromes.
Hence, the maximum number of palindromes achievable is 2.
```

Example 3:

```text
Input: words = ["cd","ef","a"]
Output: 1
Explanation: In this example, there is no need to perform any operation.
There is one palindrome in words "a".
It can be shown that it is not possible to get more than one palindrome after any number of operations.
Hence, the answer is 1.
```

__Constraints:__

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 100`
- `words[i]` consists only of lowercase English letters.

__题目描述:__

给你一个下标从 __0__ 开始的字符串数组 `words` ，数组的长度为 `n` ，且包含下标从 __0__ 开始的若干字符串。

你可以执行以下操作 任意 次数（包括零次）：

- 选择整数 `i`、 `j`、 `x`和 `y`，满足 `0 <= i, j < n`， `0 <= x < words[i].length`， `0 <= y < words[j].length`，__交换__ 字符 `words[i][x]` 和 `words[j][y]` 。

返回一个整数，表示在执行一些操作后， `words` 中可以包含的回文串的 __最大__ 数量。

__注意:__
在操作过程中， `i` 和 `j` 可以相等。

__示例:__

示例 1：

```text
输入：words = ["abbb","ba","aa"]
输出：3
解释：在这个例子中，获得最多回文字符串的一种方式是：
选择 i = 0, j = 1, x = 0, y = 0，交换 words[0][0] 和 words[1][0] 。words 变成了 ["bbbb","aa","aa"] 。
words 中的所有字符串都是回文。
因此，可实现的回文字符串的最大数量是 3 。
```

示例 2：

```text
输入：words = ["abc","ab"]
输出：2
解释：在这个例子中，获得最多回文字符串的一种方式是： 
选择 i = 0, j = 1, x = 1, y = 0，交换 words[0][1] 和 words[1][0] 。words 变成了 ["aac","bb"] 。
选择 i = 0, j = 0, x = 1, y = 2，交换 words[0][1] 和 words[0][2] 。words 变成了 ["aca","bb"] 。
两个字符串都是回文 。
因此，可实现的回文字符串的最大数量是 2。
```

示例 3：

```text
输入：words = ["cd","ef","a"]
输出：1
解释：在这个例子中，没有必要执行任何操作。
words 中有一个回文 "a" 。
可以证明，在执行任何次数操作后，无法得到更多回文。
因此，答案是 1 。
```

__提示：__

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 100`
- `words[i]` 仅由小写英文字母组成。

__思路:__

```text
贪心
首先将所有的字符统计个数
并累加字符长度
对于出现次数为奇数的字符提前填入
那么剩下待填的字符串都为偶数
先填长度最小的字符串
填入的时候如果字符串长度为奇数减少到最近的偶数
填完之后如果还能填入就累加结果
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要开销为排序, 记录字符出现的个数可以用位运算压缩到 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxPalindromesAfterOperations(vector<string>& words) 
    {
        int total = 0, result = 0;
        unordered_map<char, int> m;
        for (const auto& word : words) 
        {
            total += word.size();
            for (const auto& c : word) ++m[c];
        }
        for (const auto& [_, v] : m) total -= v & 1;
        ranges::sort(words, {}, &std::string::size);
        for (const auto& word : words) 
        {
            if ((total -= (word.size() >> 1 << 1)) < 0) break;
            ++result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxPalindromesAfterOperations(String[] words) {
        int total = 0, mask = 0, result = 0;
        for (var word : words) {
            total += word.length();
            for (var c : word.toCharArray()) mask ^= 1 << (c - 'a');
        }
        total -= Integer.bitCount(mask);
        Arrays.sort(words, Comparator.comparingInt(String::length));
        for (var word : words) {
            if ((total -= (word.length() >>> 1 << 1)) < 0) break;
            result++;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxPalindromesAfterOperations(self, words: List[str]) -> int:
        result, total, c = 0, 0, Counter()
        for word in words:
            total += len(word)
            c += Counter(word)
        total -= sum(v & 1 for v in c.values())
        words.sort(key=len)
        for word in words:
            if (total := total - (len(word) >> 1 << 1)) < 0:
                break
            result += 1
        return result
```
