# 2375 Construct Smallest Number From DI String 根据模式串构造最小数字

__Description:__

You are given a __0-indexed__ string `pattern` of length `n` consisting of the characters `'I'` meaning __increasing__ and `'D'` meaning __decreasing__.

A __0-indexed__ string `num` of length `n + 1` is created using the following conditions:

- `num` consists of the digits `'1'` to `'9'`, where each digit is used __at most__ once.
- If `pattern[i] == 'I'`, then `num[i] < num[i + 1]`.
- If `pattern[i] == 'D'`, then `num[i] > num[i + 1]`.

Return _the lexicographically __smallest__ possible string_ `num` _that meets the conditions._

__Example:__

Example 1:

```text
Input: pattern = "IIIDIDDD"
Output: "123549876"
Explanation:
At indices 0, 1, 2, and 4 we must have that num[i] < num[i+1].
At indices 3, 5, 6, and 7 we must have that num[i] > num[i+1].
Some possible values of num are "245639871", "135749862", and "123849765".
It can be proven that "123549876" is the smallest possible num that meets the conditions.
Note that "123414321" is not possible because the digit '1' is used more than once.
```

Example 2:

```text
Input: pattern = "DDD"
Output: "4321"
Explanation:
Some possible values of num are "9876", "7321", and "8742".
It can be proven that "4321" is the smallest possible num that meets the conditions.
```

__Constraints:__

- `1 <= pattern.length <= 8`
- `pattern` consists of only the letters `'I'` and `'D'`.

__题目描述:__

给你下标从 __0__ 开始、长度为 `n` 的字符串 `pattern` ，它包含两种字符， `'I'` 表示 __上升__ ， `'D'` 表示 __下降__ 。

你需要构造一个下标从 __0__ 开始长度为 `n + 1` 的字符串，且它要满足以下条件:

- `num` 包含数字 `'1'` 到 `'9'` ，其中每个数字 __至多__ 使用一次。
- 如果 `pattern[i] == 'I'` ，那么 `num[i] < num[i + 1]` 。
- 如果 `pattern[i] == 'D'` ，那么 `num[i] > num[i + 1]` 。

请你返回满足上述条件字典序 __最小__ 的字符串 `num`。

__示例:__

示例 1：

```text
输入：pattern = "IIIDIDDD"
输出："123549876"
解释：
下标 0 ，1 ，2 和 4 处，我们需要使 num[i] < num[i+1] 。
下标 3 ，5 ，6 和 7 处，我们需要使 num[i] > num[i+1] 。
一些可能的 num 的值为 "245639871" ，"135749862" 和 "123849765" 。
"123549876" 是满足条件最小的数字。
注意，"123414321" 不是可行解因为数字 '1' 使用次数超过 1 次。
```

示例 2：

```text
输入：pattern = "DDD"
输出："4321"
解释：
一些可能的 num 的值为 "9876" ，"7321" 和 "8742" 。
"4321" 是满足条件最小的数字。
```

__提示：__

- `1 <= pattern.length <= 8`
- `pattern` 只包含字符 `'I'` 和 `'D'` 。

__思路:__

```text
贪心
按照 "III...DDD" 的模式
从 i 开始找到 I 的末尾位置
正向填入结果字符串
记录该位置为 pre
从 pre 开始找到 D 的末尾位置
再将当前位置的反向填入结果字符串
重复上述过程
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string smallestNumber(string pattern) 
    {
        int i = 0, n = pattern.size(), cur = '1', pre = i;
        string result(n + 1, '1');
        while (i < n) 
        {
            for (i += i > 0 and pattern[i] == 'I'; i < n and pattern[i] == 'I'; i++) result[i] = cur++;
            for (pre = i; i < n and pattern[i] == 'D'; i++) ;
            for (int j = i; j >= pre; j--) result[j] = cur++;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String smallestNumber(String pattern) {
        int i = 0, n = pattern.length(), pre = i;
        char result[] = new char[n + 1], cur = '1';
        while (i < n) {
            for (i += i > 0 && pattern.charAt(i) == 'I' ? 1 : 0; i < n && pattern.charAt(i) == 'I'; i++) result[i] = cur++;
            for (pre = i; i < n && pattern.charAt(i) == 'D'; i++) ;
            for (int j = i; j >= pre; j--) result[j] = cur++;
        }
        return new String(result);
    }
}
```

__Python__:

```Python
class Solution:
    def smallestNumber(self, pattern: str) -> str:
        return next(''.join(map(str, t)) for t in permutations(range(1, len(pattern) + 2)) if all(d == 'I' and a < b or d == 'D' and a > b for d, (a, b) in zip(pattern, pairwise(t))))
```
