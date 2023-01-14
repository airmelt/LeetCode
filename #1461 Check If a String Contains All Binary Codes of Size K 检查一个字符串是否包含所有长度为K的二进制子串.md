# 1461 Check If a String Contains All Binary Codes of Size K 检查一个字符串是否包含所有长度为K的二进制子串

__Description:__

Given a binary string  `s` and an integer  `k`, return  `true` _if every binary code of length_  `k` _is a substring of_  `s`. Otherwise, return  `false`.

__Example:__

Example 1:

```text
Input: s = "00110110", k = 2
Output: true
Explanation: The binary codes of length 2 are "00", "01", "10" and "11". They can be all found as substrings at indices 0, 1, 3 and 2 respectively.
```

Example 2:

```text
Input: s = "0110", k = 1
Output: true
Explanation: The binary codes of length 1 are "0" and "1", it is clear that both exist as a substring.
```

Example 3:

```text
Input: s = "0110", k = 2
Output: false
Explanation: The binary code "00" is of length 2 and does not exist in the array.
```

__Constraints:__

- `1 <= s.length <= 5 * 10 ^ 5`
- `s[i]` is either  `'0'` or  `'1'`.
- `1 <= k <= 20`

__题目描述:__

给你一个二进制字符串  `s` 和一个整数  `k` 。如果所有长度为  `k` 的二进制字符串都是  `s` 的子串，请返回  `true` ，否则请返回  `false` 。

__示例:__

示例 1：

```text
输入：s = "00110110", k = 2
输出：true
解释：长度为 2 的二进制串包括 "00"，"01"，"10" 和 "11"。它们分别是 s 中下标为 0，1，3，2 开始的长度为 2 的子串。
```

示例 2：

```text
输入：s = "0110", k = 1
输出：true
解释：长度为 1 的二进制串包括 "0" 和 "1"，显然它们都是 s 的子串。
```

示例 3：

```text
输入：s = "0110", k = 2
输出：false
解释：长度为 2 的二进制串 "00" 没有出现在 s 中。
```

__提示：__

- `1 <= s.length <= 5 * 10 ^ 5`
- `s[i]` 不是 `'0'` 就是  `'1'`
- `1 <= k <= 20`

__思路:__

```text
哈希表 ➕ 滑动窗口
首先如果 s 包含所有 2 ^ K 个不同的长度为 K 的二进制字符串
其长度至少为 2 ^ K + K - 1
因为 2 ^ K 个字符串起点分别有 2 ^ K 个假设复用起点, 最后一个字符串还需要 K - 1 个字符
滑动初始状态为 s[:k], 每次向前滑动一个字符
s_new 为 s_old[1:] 左移 1 位加上 s[i + k - 1]
将所有子串加入哈希表
最后检查哈希表的大小是否为 2 ^ k
时间复杂度为 O(N), 空间复杂度为 O(2 ^ K)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool hasAllCodes(string s, int k) 
    {
        int n = s.size();
        if (n < (1 << k) + k - 1) return false;
        int num = stoi(s.substr(0, k), nullptr, 2);
        unordered_set<int> st{num};
        for (int i = 1; i + k <= n; i++) st.insert(num = (num - ((s[i - 1] - '0') << (k - 1))) * 2 + (s[i + k - 1] - '0'));
        return st.size() == (1 << k);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean hasAllCodes(String s, int k) {
        int n = s.length();
        if (n < (1 << k) + k - 1) return false;
        Set<Integer> set = new HashSet<>();
        int num = Integer.parseInt(s.substring(0, k), 2);
        set.add(num);
        for (int i = 1; i + k <= n; i++) set.add(num = (num - ((s.charAt(i - 1) - '0') << (k - 1))) * 2 + (s.charAt(i + k - 1) - '0'));
        return set.size() == (1 << k);
    }
}
```

__Python__:

```Python
class Solution:
    def hasAllCodes(self, s: str, k: int) -> bool:
        return len(set(s[i:i + k] for i in range(len(s) - k + 1))) == 2 ** k
```
