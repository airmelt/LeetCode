# 1898 Maximum Number of Removable Characters 可移除字符的最大数目

__Description:__

You are given two strings `s` and `p` where `p` is a __subsequence__ of `s`. You are also given a __distinct 0-indexed__ integer array `removable` containing a subset of indices of `s` ( `s` is also __0-indexed__).

You want to choose an integer `k` ( `0 <= k <= removable.length`) such that, after removing `k` characters from `s` using the __first__ `k` indices in `removable`, `p` is still a __subsequence__ of `s`. More formally, you will mark the character at `s[removable[i]]` for each `0 <= i < k`, then remove all marked characters and check if `p` is still a subsequence.

Return _the __maximum___ `k` _you can choose such that_ `p` _is still a __subsequence__ of_ `s` _after the removals_.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

__Example:__

Example 1:

```text
Input: s = "abcacb", p = "ab", removable = [3,1,0]
Output: 2
Explanation: After removing the characters at indices 3 and 1, "abcacb" becomes "accb".
"ab" is a subsequence of "accb".
If we remove the characters at indices 3, 1, and 0, "abcacb" becomes "ccb", and "ab" is no longer a subsequence.
Hence, the maximum k is 2.
```

Example 2:

```text
Input: s = "abcbddddd", p = "abcd", removable = [3,2,1,4,5,6]
Output: 1
Explanation: After removing the character at index 3, "abcbddddd" becomes "abcddddd".
"abcd" is a subsequence of "abcddddd".
```

Example 3:

```text
Input: s = "abcab", p = "abc", removable = [0,1,2,3,4]
Output: 0
Explanation: If you remove the first index in the array removable, "abc" is no longer a subsequence.
```

__Constraints:__

- `1 <= p.length <= s.length <= 10 ^ 5`
- `0 <= removable.length < s.length`
- `0 <= removable[i] < s.length`
- `p` is a __subsequence__ of `s`.
- `s` and `p` both consist of lowercase English letters.
- The elements in `removable` are __distinct__.

__题目描述:__

给你两个字符串 `s` 和 `p` ，其中 `p` 是 `s` 的一个 __子序列__ 。同时，给你一个元素 __互不相同__ 且下标 __从 0 开始__ 计数的整数数组 `removable` ，该数组是 `s` 中下标的一个子集（ `s` 的下标也 __从 0 开始__ 计数）。

请你找出一个整数 `k`（ `0 <= k <= removable.length`），选出 `removable` 中的 __前__ `k` 个下标，然后从 `s` 中移除这些下标对应的 `k` 个字符。整数 `k` 需满足:在执行完上述步骤后， `p` 仍然是 `s` 的一个 __子序列__ 。更正式的解释是，对于每个 `0 <= i < k` ，先标记出位于 `s[removable[i]]` 的字符，接着移除所有标记过的字符，然后检查 `p` 是否仍然是 `s` 的一个子序列。

返回你可以找出的 __最大__ `k` ，满足在移除字符后 `p` 仍然是 `s` 的一个子序列。

字符串的一个 子序列 是一个由原字符串生成的新字符串，生成过程中可能会移除原字符串中的一些字符（也可能不移除）但不改变剩余字符之间的相对顺序。

__示例:__

示例 1：

```text
输入：s = "abcacb", p = "ab", removable = [3,1,0]
输出：2
解释：在移除下标 3 和 1 对应的字符后，"abcacb" 变成 "accb" 。
"ab" 是 "accb" 的一个子序列。
如果移除下标 3、1 和 0 对应的字符后，"abcacb" 变成 "ccb" ，那么 "ab" 就不再是 s 的一个子序列。
因此，最大的 k 是 2 。
```

示例 2：

```text
输入：s = "abcbddddd", p = "abcd", removable = [3,2,1,4,5,6]
输出：1
解释：在移除下标 3 对应的字符后，"abcbddddd" 变成 "abcddddd" 。
"abcd" 是 "abcddddd" 的一个子序列。
```

示例 3：

```text
输入：s = "abcab", p = "abc", removable = [0,1,2,3,4]
输出：0
解释：如果移除数组 removable 的第一个下标，"abc" 就不再是 s 的一个子序列。
```

__提示：__

- `1 <= p.length <= s.length <= 10 ^ 5`
- `0 <= removable.length < s.length`
- `0 <= removable[i] < s.length`
- `p` 是 `s` 的一个 __子字符串__
- `s` 和 `p` 都由小写英文字母组成
- `removable` 中的元素 __互不相同__

__思路:__

```text
二分法
对于每一个 mid, 将 removable 中的前 mid 个元素标记为 0
或者记录前 mid 个元素不访问这些元素
然后检查 p 是否是 s 的子序列
如果是, 则说明 mid 还可以增大, left = mid + 1
否则, 说明 mid 过大, right = mid
最后返回 left
时间复杂度为 O(NlogN), 空间复杂度为 O(N), N 为 s 的长度, 每次查询需要 O(N) 的空间复杂度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumRemovals(string s, string p, vector<int>& removable) 
    {
        int left = 0, right = removable.size(), mid;
        while (left < right) 
        {
            mid = left + ((right - left) >> 1);
            if (check(s, p, removable, mid)) left = mid + 1;
            else right = mid;
        }
        return left;
    }
private:
    bool check(string s, string& p, vector<int>& removable, int mid) 
    {
        for (int i = 0; i <= mid; i++) s[removable[i]] = 0;
        for (int i = 0, j = 0, m = s.size(), n = p.size(); i < m; i++) 
        {
            if (s[i] == p[j]) ++j;
            if (j == n) return true;
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumRemovals(String s, String p, int[] removable) {
        int left = 0, right = removable.length, mid = 0;
        while (left < right) {
            mid = left + ((right - left) >>> 1);
            if (check(s, p, removable, mid)) left = mid + 1;
            else right = mid;
        }
        return left;
    }

    private boolean check(String s, String p, int[] removable, int mid) {
        boolean[] states = new boolean[s.length()];
        for (int i = 0; i <= mid; i++) states[removable[i]] = true;
        for (int i = 0, j = 0, m = s.length(), n = p.length(); i < m; i++) {
            if (!states[i] && s.charAt(i) == p.charAt(j)) ++j;
            if (j == n) return true;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumRemovals(self, s: str, p: str, removable: List[int]) -> int:
        left, right, m, n = 0, len(removable), len(s), len(p)

        def check(mid: int) -> bool:
            states, i, j = set(removable[:mid + 1]), -1, 0
            while (i := i + 1) < m:
                if i not in states and s[i] == p[j]:
                    j += 1
                if j == n:
                    return True
            return False
        while left < right:
            if check(mid := left + ((right - left) >> 1)):
                left = mid + 1
            else:
                right = mid
        return left
```
