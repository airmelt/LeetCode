# 1717 Maximum Score From Removing Substrings 删除子字符串的最大得分

__Description:__

You are given a string `s` and two integers `x` and `y`. You can perform two types of operations any number of times.

- Remove substring `"ab"` and gain `x` points.
  - For example, when removing `"ab"` from `"cabxbae"` it becomes `"cxbae"`.
- Remove substring `"ba"` and gain `y` points.
  - For example, when removing `"ba"` from `"cabxbae"` it becomes `"cabxe"`.

Return _the maximum points you can gain after applying the above operations on_ `s`.

__Example:__

Example 1:

```text
Input: s = "cdbcbbaaabab", x = 4, y = 5
Output: 19
Explanation:
- Remove the "ba" underlined in "cdbcbbaaabab". Now, s = "cdbcbbaaab" and 5 points are added to the score.
- Remove the "ab" underlined in "cdbcbbaaab". Now, s = "cdbcbbaa" and 4 points are added to the score.
- Remove the "ba" underlined in "cdbcbbaa". Now, s = "cdbcba" and 5 points are added to the score.
- Remove the "ba" underlined in "cdbcba". Now, s = "cdbc" and 5 points are added to the score.
Total score = 5 + 4 + 5 + 5 = 19.
```

Example 2:

```text
Input: s = "aabbaaxybbaabb", x = 5, y = 4
Output: 20
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `1 <= x, y <= 10 ^ 4`
- `s` consists of lowercase English letters.

__题目描述:__

给你一个字符串 `s` 和两个整数 `x` 和 `y` 。你可以执行下面两种操作任意次。

- 删除子字符串 `"ab"` 并得到 `x` 分。
  - 比方说，从 `"c__ab__xbae"` 删除 `ab` ，得到 `"cxbae"` 。
- 删除子字符串 `"ba"` 并得到 `y` 分。
  - 比方说，从 `"cabx__ba__e"` 删除 `ba` ，得到 `"cabxe"` 。

请返回对 `s` 字符串执行上面操作若干次能得到的最大得分。

__示例:__

示例 1：

```text
输入：s = "cdbcbbaaabab", x = 4, y = 5
输出：19
解释：
- 删除 "cdbcbbaaabab" 中加粗的 "ba" ，得到 s = "cdbcbbaaab" ，加 5 分。
- 删除 "cdbcbbaaab" 中加粗的 "ab" ，得到 s = "cdbcbbaa" ，加 4 分。
- 删除 "cdbcbbaa" 中加粗的 "ba" ，得到 s = "cdbcba" ，加 5 分。
- 删除 "cdbcba" 中加粗的 "ba" ，得到 s = "cdbc" ，加 5 分。
总得分为 5 + 4 + 5 + 5 = 19 。
```

示例 2：

```text
输入：s = "aabbaaxybbaabb", x = 5, y = 4
输出：20
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `1 <= x, y <= 10 ^ 4`
- `s` 只包含小写英文字母。

__思路:__

```text
模拟
一边遍历一边记录 'a' 和 'b' 的出现次数
当当前字符为 'a' 时, 如果 'ba' 的得分大于 'ab' 并且 'b' 的数目大于 0, 结果加上 y, 并减去一个 'b'
否则 'ab' 得分更大那么加上 'a' 的出现次数
'b' 的类似
当字符不为 'a' 或 'b' 时, 将计数清零, 此时需要加上得分较少的种类, 因为这个时候不可能出现得分高的情况了
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumGain(string s, int x, int y) 
    {
        int count1 = 0, count2 = 0, result = 0;
        for (const auto& c : s) 
        {
            if (c == 'a') 
            {
                if (y > x and count2 > 0) 
                {
                    result += y;
                    --count2;
                } 
                else ++count1;
            } 
            else if (c == 'b') 
            {          
                if (x >= y and count1 > 0) 
                {
                    result += x;
                    --count1;
                } 
                else ++count2;
            } 
            else 
            {
                result += (count1 > 0 and count2 > 0) * min(x, y) * min(count1, count2);
                count1 = count2 = 0;
            }
        }
        return result + (count1 > 0 and count2 > 0) * min(x, y) * min(count1, count2);
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumGain(String s, int x, int y) {
        int count1 = 0, count2 = 0, result = 0;
        for (char c : s.toCharArray()) {
            if (c == 'a') {
                if (y > x && count2 > 0) {
                    result += y;
                    --count2;
                } else ++count1;
            } else if (c == 'b') {          
                if (x >= y && count1 > 0) {
                    result += x;
                    --count1;
                } else ++count2;
            } else {
                result += (count1 > 0 && count2 > 0 ? 1 : 0) * Math.min(x, y) * Math.min(count1, count2);
                count1 = count2 = 0;
            }
        }
        return result + (count1 > 0 && count2 > 0 ? 1 : 0) * Math.min(x, y) * Math.min(count1, count2);
    }
}
```

__Python__:

```Python
class Solution:
    def maximumGain(self, s: str, x: int, y: int) -> int:
        count1, count2, result = 0, 0, 0
        for c in s:
            if c == 'a':
                if y > x and count2 > 0:
                    result += y
                    count2 -= 1
                else:
                    count1 += 1
            elif c == 'b':
                if x >= y and count1 > 0:
                    result += x
                    count1 -= 1
                else:
                    count2 += 1
            else:
                result += (count1 > 0 and count2 > 0) * min(x, y) * min(count1, count2)
                count1, count2 = 0, 0
        return result + (count1 > 0 and count2 > 0) * min(x, y) * min(count1, count2)
```
