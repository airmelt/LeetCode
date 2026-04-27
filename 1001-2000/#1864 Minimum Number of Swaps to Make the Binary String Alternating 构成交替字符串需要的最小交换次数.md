# 1864 Minimum Number of Swaps to Make the Binary String Alternating 构成交替字符串需要的最小交换次数

__Description:__

Given a binary string `s`, return _the __minimum__ number of character swaps to make it __alternating__, or_ `-1` _if it is impossible._

The string is called __alternating__ if no two adjacent characters are equal. For example, the strings `"010"` and `"1010"` are alternating, while the string `"0100"` is not.

Any two characters may be swapped, even if they are not adjacent.

__Example:__

Example 1:

```text
Input: s = "111000"
Output: 1
Explanation: Swap positions 1 and 4: "111000" -> "101010"
The string is now alternating.
```

Example 2:

```text
Input: s = "010"
Output: 0
Explanation: The string is already alternating, no swaps are needed.
```

Example 3:

```text
Input: s = "1110"
Output: -1
```

__Constraints:__

- `1 <= s.length <= 1000`
- `s[i]` is either `'0'` or `'1'`.

__题目描述:__

给你一个二进制字符串 `s` ，现需要将其转化为一个 __交替字符串__ 。请你计算并返回转化所需的 __最小__ 字符交换次数，如果无法完成转化，返回 `-1` 。

__交替字符串__ 是指:相邻字符之间不存在相等情况的字符串。例如，字符串 `"010"` 和 `"1010"` 属于交替字符串，但 `"0100"` 不是。

任意两个字符都可以进行交换，不必相邻 。

__示例:__

示例 1：

```text
输入：s = "111000"
输出：1
解释：交换位置 1 和 4："111000" -> "101010" ，字符串变为交替字符串。
```

示例 2：

```text
输入：s = "010"
输出：0
解释：字符串已经是交替字符串了，不需要交换。
```

示例 3：

```text
输入：s = "1110"
输出：-1
```

__提示：__

- `1 <= s.length <= 1000`
- `s[i]` 的值为 `'0'` 或 `'1'`

__思路:__

```text
贪心
按照奇偶下标分别统计 s[i] 为 '0' 和 '1' 的数量
交换完成之后要么是 '0101...' 要么是 '1010...'
选择交换次数最少的一种情况即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minSwaps(string s) 
    {
        vector<int> a0(2), a1(2);
        for (int i = 0, n = s.size(); i < n; i++) 
        {
           a0.front() += s[i] == '0' and (i & 1);
           a1.front() += s[i] == '0' and !(i & 1);
           a0.back() += s[i] == '1' and !(i & 1);
           a1.back() += s[i] == '1' and (i & 1);
       }
       return (a0.front() != a0.back() and a1.front() != a1.back()) ? -1 : min(a0.front() == a0.back() ? a0[0] : 1001, a1.front() == a1.back() ? a1.front() : 1001);
    }
};
```

__Java__:

```Java
class Solution {
    public int minSwaps(String s) { 
        int a0[] = new int[2], a1[] = new int[2], n = s.length(), INF = 1001;
        for (int i = 0; i < n; i++) {
           a0[0] += s.charAt(i) == '0' ? (i % 2 == 1 ? 1 : 0) : 0;
           a1[0] += s.charAt(i) == '0' ? (i % 2 == 0 ? 1 : 0) : 0;
           a0[1] += s.charAt(i) == '1' ? (i % 2 == 0 ? 1 : 0) : 0;
           a1[1] += s.charAt(i) == '1' ? (i % 2 == 1 ? 1 : 0) : 0;
       }
       return (a0[0] != a0[1] && a1[0] != a1[1]) ? -1 : Math.min(a0[0] == a0[1] ? a0[0] : INF, a1[0] == a1[1] ? a1[0] : INF);
   }
}
```

__Python__:

```Python
class Solution:
    def minSwaps(self, s: str) -> int:
        return -1 if ((len(s) & 1) and abs(s.count('0') - s.count('1')) != 1) or (not (len(s) & 1) and s.count('0') != s.count('1')) else s.count('0') - sum(c == '0' for c in s[::2]) if (len(s) & 1) and s.count('0') > s.count('1') else s.count('1') - sum(c == '1' for c in s[::2]) if (len(s) & 1) and s.count('0') < s.count('1') else min(s.count('0') - sum(c == '0' for c in s[::2]), s.count('0') - sum(c == '0' for c in s[1::2]), s.count('1') - sum(c == '1' for c in s[::2]), s.count('1') - sum(c == '1' for c in s[1::2]))
```
