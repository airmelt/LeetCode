# 1405 Longest Happy String 最长快乐字符串

__Description:__

A string s is called happy if it satisfies the following conditions:

s only contains the letters 'a', 'b', and 'c'.
s does not contain any of "aaa", "bbb", or "ccc" as a substring.
s contains at most a occurrences of the letter 'a'.
s contains at most b occurrences of the letter 'b'.
s contains at most c occurrences of the letter 'c'.

Given three integers a, b, and c, return the longest possible happy string. If there are multiple longest happy strings, return any of them. If there is no such string, return the empty string "".

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

Input: a = 1, b = 1, c = 7
Output: "ccaccbcc"
Explanation: "ccbccacc" would also be a correct answer.

Example 2:

Input: a = 7, b = 1, c = 0
Output: "aabaa"
Explanation: It is the only correct answer in this case.

__Constraints:__

0 <= a, b, c <= 100
a + b + c > 0

__题目描述:__

如果字符串中不含有任何 'aaa'，'bbb' 或 'ccc' 这样的字符串作为子串，那么该字符串就是一个「快乐字符串」。

给你三个整数 a，b ，c，请你返回 任意一个 满足下列全部条件的字符串 s：

s 是一个尽可能长的快乐字符串。
s 中 最多 有a 个字母 'a'、b 个字母 'b'、c 个字母 'c' 。
s 中只含有 'a'、'b' 、'c' 三种字母。

如果不存在这样的字符串 s ，请返回一个空字符串 ""。

__示例:__

示例 1：

输入：a = 1, b = 1, c = 7
输出："ccaccbcc"
解释："ccbccacc" 也是一种正确答案。

示例 2：

输入：a = 2, b = 2, c = 1
输出："aabbc"

示例 3：

输入：a = 7, b = 1, c = 0
输出："aabaa"
解释：这是该测试用例的唯一正确答案。

__提示：__

0 <= a, b, c <= 100
a + b + c > 0

__思路:__

```text
贪心
每次取出最多的字符尝试加入结果中
如果全部取出直接返回结果
否则尝试加入最多的字符
除非加入的是同一个字符就选择另一个字符
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string longestDiverseString(int a, int b, int c) 
    {
        vector<pair<int, char>> x{{a, 'a'}, {b, 'b'}, {c, 'c'}};
        string result;
        while (1) 
        {
            sort(x.rbegin(), x.rend());
            for (auto& [count, value]: x) 
            {
                if (count <= 0) return result;
                if (result.size() > 1 and result.back() == value and result[result.size() - 2] == value) continue;
                result += value;
                --count;
                break;
            }
        }
    }
};
```

__Java__:

```Java
class Solution {
    public String longestDiverseString(int a, int b, int c) {
        int[][] x = new int[][]{{a, 'a'}, {b, 'b'}, {c, 'c'}};
        StringBuilder sb = new StringBuilder();
        while (true) {
            Arrays.sort(x, (int[] p, int[] q) -> q[0] - p[0]);
            for (int[] num: x) {
                if (num[0] <= 0) return sb.toString();
                if (sb.length() > 1 && sb.charAt(sb.length() - 1) == num[1] && sb.charAt(sb.length() - 2) == num[1]) continue;
                sb.append((char)num[1]);
                --num[0];
                break;
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def longestDiverseString(self, a: int, b: int, c: int) -> str:
        result, x = "", [[a, 'a'],[b, 'b'],[c, 'c']]
        while True:
            for num in sorted(x, reverse=True):
                if num[0] <= 0:
                    return result
                if len(result) >= 2 and result[-2:] == num[1] * 2:
                    continue
                result += num[1]
                num[0] -= 1
                break
```
