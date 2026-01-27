# 1520 Maximum Number of Non-Overlapping Substrings 最多的不重叠子字符串

__Description:__

Given a string `s` of lowercase letters, you need to find the maximum number of __non-empty__ substrings of `s` that meet the following conditions:

Find the maximum number of substrings that meet the above conditions. If there are multiple solutions with the same number of substrings, return the one with minimum total length. It can be shown that there exists a unique solution of minimum total length.

Notice that you can return the substrings in any order.

__Example:__

Example 1:

```text
Input: s = "adefaddaccc"
Output: ["e","f","ccc"]
Explanation: The following are all the possible substrings that meet the conditions:
[
  "adefaddaccc"
  "adefadda",
  "ef",
  "e",
  "f",
  "ccc",
]
If we choose the first string, we cannot choose anything else and we'd get only 1. If we choose "adefadda", we are left with "ccc" which is the only one that doesn't overlap, thus obtaining 2 substrings. Notice also, that it's not optimal to choose "ef" since it can be split into two. Therefore, the optimal way is to choose ["e","f","ccc"] which gives us 3 substrings. No other solution of the same number of substrings exist.
```

Example 2:

```text
Input: s = "abbaccd"
Output: ["d","bb","cc"]
Explanation: Notice that while the set of substrings ["d","abba","cc"] also has length 3, it's considered incorrect since it has larger total length.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` contains only lowercase English letters.

__题目描述:__

给你一个只包含小写字母的字符串 `s` ，你需要找到 `s` 中最多数目的非空子字符串，满足如下条件：

请你找到满足上述条件的最多子字符串数目。如果有多个解法有相同的子字符串数目，请返回这些子字符串总长度最小的一个解。可以证明最小总长度解是唯一的。

请注意，你可以以 任意 顺序返回最优解的子字符串。

__示例:__

示例 1：

```text
输入：s = "adefaddaccc"
输出：["e","f","ccc"]
解释：下面为所有满足第二个条件的子字符串：
[
  "adefaddaccc"
  "adefadda",
  "ef",
  "e",
  "f",
  "ccc",
]
如果我们选择第一个字符串，那么我们无法再选择其他任何字符串，所以答案为 1 。如果我们选择 "adefadda" ，剩下子字符串中我们只可以选择 "ccc" ，它是唯一不重叠的子字符串，所以答案为 2 。同时我们可以发现，选择 "ef" 不是最优的，因为它可以被拆分成 2 个子字符串。所以最优解是选择 ["e","f","ccc"] ，答案为 3 。不存在别的相同数目子字符串解。
```

示例 2：

```text
输入：s = "abbaccd"
输出：["d","bb","cc"]
解释：注意到解 ["d","abba","cc"] 答案也为 3 ，但它不是最优解，因为它的总长度更长。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 只包含小写英文字母。

__思路:__

```text
贪心
实际上是求不相交的线段的数量
每个线段的结尾一定是某个字符在 s 中最后出现的位置 
每个线段的开头一定是某个字符在 s 中首次出现的位置
记录每个字符在 s 中第一次和最后一次出现的位置分别为 left, right 数组
记录所有字符在 s 中最后一次出现的位置并排序得到 last 数组
将开始的位置设置为 -1, 该位置也用来记录每次线段搜索的起点, 超过该位置的线段都不用加入结果
遍历数组 last, 每次从 end = last[i] 开始搜索, 对应的开始字符的下标为 pre
从 end 开始向 pre 进行搜索, 如果字符最后出现的位置大于 pre, 表示没有以 end 结尾的子串, 退出循环
否则将 pre 更新为字符第一次出现的位置的值和 pre 的较小值
即向左搜索, 扩大字符串的长度, 使字符串包含字符第一次出现的位置和最后一次出现的位置
如果 end 等于 pre, 说明 s[pre:last[i] + 1] 就是所需子串, 加入结果中
时间复杂度为 O(N), 空间复杂度为 O(1), 只需要小写字母额外的 O(1) 时间和空间复杂度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> maxNumOfSubstrings(string s) 
    {
        vector<int> left(26, -1), right(26, -1), last;
        vector<string> result;
        for (int i = 0, n = s.size(); i < n; i++)
        {
            if (left[s[i] - 'a'] == -1) left[s[i] - 'a'] = i;
            right[s[i] - 'a'] = i;
        }
        for (const auto& i: right) if (i != -1) last.emplace_back(i);
        sort(last.begin(), last.end());
        int start = -1, m = last.size();
        for (int i = 0; i < m; i++)
        {
            int end = last[i], pre = left[s[end] - 'a'];
            while (end > pre)
            {
                if (right[s[end] - 'a'] > last[i] or end <= start) break;
                pre = min(pre, left[s[end--] - 'a']);
            }
            if (end == pre) result.emplace_back(s.substr(pre, (start = last[i]) - end + 1));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> maxNumOfSubstrings(String s) {
        int n = s.length(), left[] = new int[26], right[] = new int[26];
        Arrays.fill(left, -1);
        Arrays.fill(right, -1);
        List<Integer> last = new ArrayList<>();
        List<String> result = new ArrayList<>();
        for (int i = 0; i < n; i++){
            if (left[s.charAt(i) - 'a'] == -1) left[s.charAt(i) - 'a'] = i;
            right[s.charAt(i) - 'a'] = i;
        }
        for (int i: right) if (i != -1) last.add(i);
        Collections.sort(last);
        int start = -1, m = last.size();
        for (int i = 0; i < m; i++) {
            int end = last.get(i), pre = left[s.charAt(end) - 'a'];
            while (end > pre) {
                if (right[s.charAt(end) - 'a'] > last.get(i) || end <= start) break;
                pre = Math.min(pre, left[s.charAt(end--) - 'a']);
            }
            if (end == pre) result.add(s.substring(pre, (start = last.get(i)) + 1));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxNumOfSubstrings(self, s: str) -> List[str]:
        left, right, last, start, result = [s.index(c) if (c := chr(i + ord('a'))) in s else -1 for i in range(26)], [s.rindex(c) if (c := chr(i + ord('a'))) in s else -1 for i in range(26)], sorted(list(s.rindex(i) for i in set(s))), -1, deque()
        for i, end in enumerate(last):
            pre = left[ord(s[end]) - ord('a')]
            while end > pre:
                if right[ord(s[end]) - ord('a')] > last[i] or start >= end:
                    break
                pre = min(pre, left[ord(s[end := end - 1]) - ord('a')])
            if end == pre:
                result.append(s[pre:(start := last[i]) + 1])
        return list(result)
```
