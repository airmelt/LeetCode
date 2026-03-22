# 1849 Splitting a String Into Descending Consecutive Values 将字符串拆分为递减的连续值

__Description:__

You are given a string `s` that consists of only digits.

Check if we can split `s` into __two or more non-empty substrings__ such that the __numerical values__ of the substrings are in __descending order__ and the __difference__ between numerical values of every two __adjacent__ __substrings__ is equal to `1`.

- For example, the string `s = "0090089"` can be split into `["0090", "089"]` with numerical values `[90,89]`. The values are in descending order and adjacent values differ by `1`, so this way is valid.
- Another example, the string `s = "001"` can be split into `["0", "01"]`, `["00", "1"]`, or `["0", "0", "1"]`. However all the ways are invalid because they have numerical values `[0,1]`, `[0,1]`, and `[0,0,1]` respectively, all of which are not in descending order.

Return `true` _if it is possible to split_ `s`​​​​​​ _as described above__, or_ `false` _otherwise._

A substring is a contiguous sequence of characters in a string.

__Example:__

Example 1:

```text
Input: s = "1234"
Output: false
Explanation: There is no valid way to split s.
```

Example 2:

```text
Input: s = "050043"
Output: true
Explanation: s can be split into ["05", "004", "3"] with numerical values [5,4,3].
The values are in descending order with adjacent values differing by 1.
```

Example 3:

```text
Input: s = "9080701"
Output: false
Explanation: There is no valid way to split s.
```

__Constraints:__

- `1 <= s.length <= 20`
- `s` only consists of digits.

__题目描述:__

给你一个仅由数字组成的字符串 `s` 。

请你判断能否将 `s` 拆分成两个或者多个 __非空子字符串__ ，使子字符串的 __数值__ 按 __降序__ 排列，且每两个 __相邻子字符串__ 的数值之 __差__ 等于 `1` 。

- 例如，字符串 `s = "0090089"` 可以拆分成 `["0090", "089"]` ，数值为 `[90,89]` 。这些数值满足按降序排列，且相邻值相差 `1` ，这种拆分方法可行。
- 另一个例子中，字符串 `s = "001"` 可以拆分成 `["0", "01"]`、 `["00", "1"]` 或 `["0", "0", "1"]` 。然而，所有这些拆分方法都不可行，因为对应数值分别是 `[0,1]`、 `[0,1]` 和 `[0,0,1]` ，都不满足按降序排列的要求。

如果可以按要求拆分 `s` ，返回 `true` ；否则，返回 `false` 。

子字符串 是字符串中的一个连续字符序列。

__示例:__

示例 1：

```text
输入：s = "1234"
输出：false
解释：不存在拆分 s 的可行方法。
```

示例 2：

```text
输入: s = "050043"
输出: true
解释: s 可以拆分为 ["05", "004", "3"] ，对应数值为 [5,4,3] 。
满足按降序排列，且相邻值相差 `1` 。
```

示例 3：

```text
输入：s = "9080701"
输出：false
解释：不存在拆分 s 的可行方法。
```

示例 4：

```text
输入: s = "10009998"
输出: true
解释: s 可以拆分为 ["100", "099", "98"] ，对应数值为 [100,99,98] 。
满足按降序排列，且相邻值相差 `1` 。
```

__提示：__

- `1 <= s.length <= 20`
- `s` 仅由数字组成

__思路:__

```text
枚举
由于数列中至少有两个数
从第一个字符开始枚举到倒数第二个字符
用 s[i:j] 表示第一个数
目标是找到 s[j:] 中的数与 s[i:j] 的数相差 1
如果找到一个这样的数继续往后遍历知道字符串结尾
否则遍历到比之前的数大的数就停止
当前一个数为 0 时, 可以简单的比较后缀是否全为 0
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool splitString(string s) 
    {
        for (int i = 0, n = s.size(); i < n - 1; i++) if (helper(s, stoull(s.substr(0, i + 1)), i + 1)) return true;
        return false;
    }
private:
    bool helper(string& s, unsigned long long pre, int cur)
    {
        int start = cur, flag = 0, n = s.size();
        if (start == n) return true;
        for (unsigned long long next = 0; start < n; ++start) if ((next = stoull(s.substr(cur, start - cur + 1))) == pre - 1) flag |= helper(s, next, start + 1);
        return !!flag;
    }
};
```

__Java__:

```Java
class Solution {
    int n;
    char[] cs;

    public boolean splitString(String s) {
        this.n = s.length();
        this.cs = s.toCharArray();
        long num = 0;
        for (int i = 0; i < n - 1; i++) if (helper(i + 1, num = num * 10 + cs[i] - '0')) return true;
        return false;
    }

    private boolean helper(int start, long last) {
        if (start == n) return true;
        long num = 0;
        for (int i = start; i < n; i++) if ((num = num * 10 + this.cs[i] - '0') == last - 1) if (helper(i + 1, num)) return true;
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def splitString(self, s: str) -> bool:
        n, start, target = len(s), 0, {'0'}
        for i in range(n - 1):
            pre, cur, idx = (start := 10 * start + int(s[i])), 0, i + 1
            for j in range(i + 1, n):
                if pre == 1:
                    if set(s[idx:n]) == target:
                        return True
                    else:
                        break
                if (cur := 10 * cur + int(s[j])) > pre - 1:
                    break
                elif cur == pre - 1:
                    if j + 1 == n:
                        return True
                    pre, cur, idx = cur, 0, j + 1
        return False
```
