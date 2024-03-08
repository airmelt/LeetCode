# 2024 Maximize the Confusion of an Exam 考试的最大困扰度

__Description:__

A teacher is writing a test with `n` true/false questions, with `'T'` denoting true and `'F'` denoting false. He wants to confuse the students by __maximizing__ the number of __consecutive__ questions with the __same__ answer (multiple trues or multiple falses in a row).

You are given a string `answerKey`, where `answerKey[i]` is the original answer to the `i ^ th` question. In addition, you are given an integer `k`, the maximum number of times you may perform the following operation:

- Change the answer key for any question to `'T'` or `'F'` (i.e., set `answerKey[i]` to `'T'` or `'F'`).

Return _the __maximum__ number of consecutive_ `'T'`s or `'F'`s _in the answer key after performing the operation at most_ `k` _times_.

__Example:__

Example 1:

```text
Input: answerKey = "TTFF", k = 2
Output: 4
Explanation: We can replace both the 'F's with 'T's to make answerKey = "TTTT".
There are four consecutive 'T's.
```

Example 2:

```text
Input: answerKey = "TFFT", k = 1
Output: 3
Explanation: We can replace the first 'T' with an 'F' to make answerKey = "FFFT".
Alternatively, we can replace the second 'T' with an 'F' to make answerKey = "TFFF".
In both cases, there are three consecutive 'F's.
```

Example 3:

```text
Input: answerKey = "TTFTTFTT", k = 1
Output: 5
Explanation: We can replace the first 'F' to make answerKey = "TTTTTFTT"
Alternatively, we can replace the second 'F' to make answerKey = "TTFTTTTT". 
In both cases, there are five consecutive 'T's.
```

__Constraints:__

- `n == answerKey.length`
- `1 <= n <= 5 * 10 ^ 4`
- `answerKey[i]` is either `'T'` or `'F'`
- `1 <= k <= n`

__题目描述:__

一位老师正在出一场由 `n` 道判断题构成的考试，每道题的答案为 true （用 `<span>'T'</span>` 表示）或者 false （用 `'F'` 表示）。老师想增加学生对自己做出答案的不确定性，方法是 __最大化__ 有 __连续相同__ 结果的题数。（也就是连续出现 true 或者连续出现 false）。

给你一个字符串 `answerKey` ，其中 `answerKey[i]` 是第 `i` 个问题的正确结果。除此以外，还给你一个整数 `k` ，表示你能进行以下操作的最多次数:

- 每次操作中，将问题的正确答案改为 `'T'` 或者 `'F'` （也就是将 `answerKey[i]` 改为 `'T'` 或者 `'F'` ）。

请你返回在不超过 `k` 次操作的情况下，__最大__ 连续 `'T'` 或者 `'F'` 的数目。

__示例:__

示例 1：

```text
输入：answerKey = "TTFF", k = 2
输出：4
解释：我们可以将两个 'F' 都变为 'T' ，得到 answerKey = "TTTT" 。
总共有四个连续的 'T' 。
```

示例 2：

```text
输入：answerKey = "TFFT", k = 1
输出：3
解释：我们可以将最前面的 'T' 换成 'F' ，得到 answerKey = "FFFT" 。
或者，我们可以将第二个 'T' 换成 'F' ，得到 answerKey = "TFFF" 。
两种情况下，都有三个连续的 'F' 。
```

示例 3：

```text
输入：answerKey = "TTFTTFTT", k = 1
输出：5
解释：我们可以将第一个 'F' 换成 'T' ，得到 answerKey = "TTTTTFTT" 。
或者我们可以将第二个 'F' 换成 'T' ，得到 answerKey = "TTFTTTTT" 。
两种情况下，都有五个连续的 'T' 。
```

__提示：__

- `n == answerKey.length`
- `1 <= n <= 5 * 10 ^ 4`
- `answerKey[i]` 要么是 `'T'` ，要么是 `'F'`
- `1 <= k <= n`

__思路:__

```text
滑动窗口
使用两个指针 left 和 right 分别指向滑动窗口的左右边界
用 t 和 f 分别记录滑动窗口中 'T' 和 'F' 的数量
如果 t 和 f 的数量都大于 k, 则移动 left 指针
同时更新结果为 max(result, right - left + 1)
也可以将 right 看作 len(answerKey)
只在 t 和 f 的数量都大于 k 时移动 left 指针并更新 t 和 f 的数量
返回结果为 len(answerKey) - left
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxConsecutiveAnswers(string answerKey, int k) 
    {
        int t = 0, f = 0, left = 0;
        for (const auto& c : answerKey)
        {
            t += c == 'T';
            f += c == 'F';
            if (t > k and f > k) 
            {
                t -= answerKey[left] == 'T';
                f -= answerKey[left++] == 'F';
            }
        }
        return answerKey.size() - left;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxConsecutiveAnswers(String answerKey, int k) {
        int t = 0, f = 0, result = 0, n = answerKey.length(), left = 0;
        for (int right = 0; right < n; right++) {
            if (answerKey.charAt(right) == 'T') ++t;
            else ++f;
            while (t > k && f > k) {
                if (answerKey.charAt(left) == 'T') --t;
                else --f;
                ++left;
            }
            result = Math.max(result, right - left + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxConsecutiveAnswers(self, answerKey: str, k: int) -> int:
        t = f = left = 0
        for c in answerKey:
            t += c == 'T'
            f += c == 'F'
            if f > k and t > k:
                if answerKey[left] == 'T':
                    t -= 1
                else:
                    f -= 1
                left += 1
        return len(answerKey) - left
```
