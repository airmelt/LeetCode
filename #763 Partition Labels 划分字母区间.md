# 763 Partition Labels 划分字母区间

__Description__:
You are given a string s. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

Return a list of integers representing the size of these parts.

__Example:__

Example 1:

Input: s = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.

Example 2:

Input: s = "eccbbbbdec"
Output: [10]

__Constraints:__

1 <= s.length <= 500
s consists of lowercase English letters.

__题目描述__:
字符串 S 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。返回一个表示每个字符串片段的长度的列表。

__示例 :__

输入：S = "ababcbacadefegdehijhklij"
输出：[9,7,8]
解释：
划分结果为 "ababcbaca", "defegde", "hijhklij"。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 的划分是错误的，因为划分的片段数较少。

__提示:__

S的长度在[1, 500]之间。
S只包含小写字母 'a' 到 'z' 。

__思路__:

滑动窗口
遍历记录每个字符最后出现的位置
遍历记录当前字符和字符最后出现位置相等的下标, 更新字符串的长度
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> partitionLabels(string s) 
    {
        vector<int> result, count(26);
        int start = 0, end = 0, n = s.size();
        for (int i = 0; i < n; i++) count[s[i] - 'a'] = i;
        for (int i = 0; i < n; i++) 
        {
            end = max(end, count[s[i] - 'a']);
            if (i == end) 
            {
                result.emplace_back(end - start + 1);
                start = i + 1;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> partitionLabels(String s) {
        List<Integer> result = new ArrayList<>();
        int start = 0, end = 0, n = s.length(), count[] = new int[26];
        for (int i = 0; i < n; i++) count[s.charAt(i) - 'a'] = i;
        for (int i = 0; i < n; i++) {
            end = Math.max(end, count[s.charAt(i) - 'a']);
            if (i == end) {
                result.add(end - start + 1);
                start = i + 1;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        result, start, end, n, count = [], 0, 0, len(s), [0 if chr(i + ord('a')) not in s else s.rfind(chr(i + ord('a'))) for i in range(26)]
        for i in range(n):
            if i == (end := max(end, count[ord(s[i]) - ord('a')])):
                result.append(end - start + 1)
                start = i + 1
        return result
```
