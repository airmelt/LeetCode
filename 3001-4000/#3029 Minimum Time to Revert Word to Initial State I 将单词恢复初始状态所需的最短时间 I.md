# 3029 Minimum Time to Revert Word to Initial State I 将单词恢复初始状态所需的最短时间 I

__Description:__

You are given a __0-indexed__ string `word` and an integer `k`.

At every second, you must perform the following operations:

- Remove the first `k` characters of `word`.
- Add any `k` characters to the end of `word`.

Note that you do not necessarily need to add the same characters that you removed. However, you must perform both operations at every second.

Return _the __minimum__ time greater than zero required for_ `word` _to revert to its __initial__ state_.

__Example:__

Example 1:

```text
Input: word = "abacaba", k = 3
Output: 2
Explanation: At the 1st second, we remove characters "aba" from the prefix of word, and add characters "bac" to the end of word. Thus, word becomes equal to "cababac".
At the 2nd second, we remove characters "cab" from the prefix of word, and add "aba" to the end of word. Thus, word becomes equal to "abacaba" and reverts to its initial state.
It can be shown that 2 seconds is the minimum time greater than zero required for word to revert to its initial state.
```

Example 2:

```text
Input: word = "abacaba", k = 4
Output: 1
Explanation: At the 1st second, we remove characters "abac" from the prefix of word, and add characters "caba" to the end of word. Thus, word becomes equal to "abacaba" and reverts to its initial state.
It can be shown that 1 second is the minimum time greater than zero required for word to revert to its initial state.
```

Example 3:

```text
Input: word = "abcbabcd", k = 2
Output: 4
Explanation: At every second, we will remove the first 2 characters of word, and add the same characters to the end of word.
After 4 seconds, word becomes equal to "abcbabcd" and reverts to its initial state.
It can be shown that 4 seconds is the minimum time greater than zero required for word to revert to its initial state.
```

__Constraints:__

- `1 <= word.length <= 50`
- `1 <= k <= word.length`
- `word` consists only of lowercase English letters.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `word` 和一个整数 `k` 。

在每一秒，你必须执行以下操作：

- 移除 `word` 的前 `k` 个字符。
- 在 `word` 的末尾添加 `k` 个任意字符。

注意 添加的字符不必和移除的字符相同。但是，必须在每一秒钟都执行 两种 操作。

返回将 `word` 恢复到其 __初始__ 状态所需的 __最短__ 时间（该时间必须大于零）。

__示例:__

示例 1：

```text
输入：word = "abacaba", k = 3
输出：2
解释：
第 1 秒，移除 word 的前缀 "aba"，并在末尾添加 "bac" 。因此，word 变为 "cababac"。
第 2 秒，移除 word 的前缀 "cab"，并在末尾添加 "aba" 。因此，word 变为 "abacaba" 并恢复到始状态。
可以证明，2 秒是 word 恢复到其初始状态所需的最短时间。
```

示例 2：

```text
输入：word = "abacaba", k = 4
输出：1
解释：
第 1 秒，移除 word 的前缀 "abac"，并在末尾添加 "caba" 。因此，word 变为 "abacaba" 并恢复到初始状态。
可以证明，1 秒是 word 恢复到其初始状态所需的最短时间。
```

示例 3：

```text
输入：word = "abcbabcd", k = 2
输出：4
解释：
每一秒，我们都移除 word 的前 2 个字符，并在 word 末尾添加相同的字符。
4 秒后，word 变为 "abcbabcd" 并恢复到初始状态。
可以证明，4 秒是 word 恢复到其初始状态所需的最短时间。
```

__提示：__

- `1 <= word.length <= 50`
- `1 <= k <= word.length`
- `word`仅由小写英文字母组成。

__思路:__

```text
暴力法
由于 N 不大于 50可以尝试暴力法
这里只需要比较 word[i + k:] 是否是 word 的前缀
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumTimeToInitialState(string word, int k) 
    {
        int result = 1, n = word.size();
        for (int i = 0; i + k < n; i += k, result++) if (word.substr(i + k) == word.substr(0, word.substr(i + k).size())) break;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumTimeToInitialState(String word, int k) {
        int result = 1, n = word.length();
        for (int i = 0; i + k < n; i += k, result++) if (word.substring(i + k, n).equals(word.substring(0, word.substring(i + k, n).length()))) break;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumTimeToInitialState(self, word: str, k: int) -> int:
        return a[0] + 1 if (a := [i for i in range(len(word) // k) if word[k * (i + 1):] == word[:-k * (i + 1)]]) else len(word) // k + 1
```
