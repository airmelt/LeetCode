# 1593 Split a String Into the Max Number of Unique Substrings 拆分字符串使唯一子字符串的数目最大

__Description:__

Given a string `s`, return _the maximum number of unique substrings that the given string can be split into_.

You can split string `s` into any list of __non-empty substrings__, where the concatenation of the substrings forms the original string. However, you must split the substrings such that all of them are __unique__.

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "ababccc"
Output: 5
Explanation: One way to split maximally is ['a', 'b', 'ab', 'c', 'cc']. Splitting like ['a', 'b', 'a', 'b', 'c', 'cc'] is not valid as you have 'a' and 'b' multiple times.
```

Example 2:

```text
Input: s = "aba"
Output: 2
Explanation: One way to split maximally is ['a', 'ba'].
```

Example 3:

```text
Input: s = "aa"
Output: 1
Explanation: It is impossible to split the string any further.
```

__Constraints:__

- `1 <= s.length <= 16`
- `s` contains only lower case English letters.

__题目描述:__

给你一个字符串 `s` ，请你拆分该字符串，并返回拆分后唯一子字符串的最大数目。

字符串 `s` 拆分后可以得到若干 __非空子字符串__ ，这些子字符串连接后应当能够还原为原字符串。但是拆分出来的每个子字符串都必须是 __唯一的__ 。

注意：子字符串 是字符串中的一个连续字符序列。

__示例:__

示例 1：

```text
输入：s = "ababccc"
输出：5
解释：一种最大拆分方法为 ['a', 'b', 'ab', 'c', 'cc'] 。像 ['a', 'b', 'a', 'b', 'c', 'cc'] 这样拆分不满足题目要求，因为其中的 'a' 和 'b' 都出现了不止一次。
```

示例 2：

```text
输入：s = "aba"
输出：2
解释：一种最大拆分方法为 ['a', 'ba'] 。
```

示例 3：

```text
输入：s = "aa"
输出：1
解释：无法进一步拆分字符串。
```

__提示：__

- `1 <= s.length <= 16`
- `s` 仅包含小写英文字母

__思路:__

```text
回溯法
尝试将 s 的子串加入哈希表中
如果已经存在这个子串就跳过
直到全部分割完毕, 更新结果为哈希表的长度, 即子串的数目
时间复杂度为 O(N2 ^ N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    int result;
    
    void backtrack(string& s, int start, int n, unordered_set<string>& us) 
    {
        if (start == n) 
        {
            result = max((int)us.size(), result);
            return;
        }
        for (int i = start; i < n; i++) 
        {
            string sub = s.substr(start, i - start + 1);
            if (us.find(sub) != us.end()) continue;
            us.insert(sub);
            backtrack(s, i + 1, n, us);
            us.erase(sub);
        }
    }
public:
    int maxUniqueSplit(string s) 
    {
        result = 1;
        unordered_set<string> us;
        backtrack(s, 0, s.size(), us);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int result = 1;
    
    public int maxUniqueSplit(String s) {
        backtrack(s, 0, new HashSet<>());
        return result;
    }
    
    private void backtrack(String s, int start, Set<String> set) {
        if (start == s.length()) {
            result = Math.max(set.size(), result);
            return;
        }
        for (int i = start + 1; i <= s.length(); i++) {
            String sub = s.substring(start, i);
            if (set.contains(sub)) continue;
            set.add(sub);
            backtrack(s, i, set);
            set.remove(sub);
        }
    }
}
```

__Python__:

```Python
class Solution:
    def maxUniqueSplit(self, s: str) -> int:
        n, visited, result = len(s), set(), 1
        
        def backtrack(start: int, cur: int) -> NoReturn:
            if start >= n:
                nonlocal result
                result = max(result, cur)
            else:
                for i in range(start, n):
                    if (sub := s[start:i + 1]) not in visited:
                        visited.add(sub)
                        backtrack(i + 1, cur + 1)
                        visited.remove(sub)
        backtrack(0, 0)
        return result
```
