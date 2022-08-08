# 1239 Maximum Length of a Concatenated String with Unique Characters 串联字符串的最大长度

__Description:__

You are given an array of strings arr. A string s is formed by the concatenation of a subsequence of arr that has unique characters.

Return the maximum possible length of s.

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

__Example:__

Example 1:

Input: arr = ["un","iq","ue"]
Output: 4
Explanation: All the valid concatenations are:

- ""
- "un"
- "iq"
- "ue"
- "uniq" ("un" + "iq")
- "ique" ("iq" + "ue")

Maximum length is 4.

Example 2:

Input: arr = ["cha","r","act","ers"]
Output: 6
Explanation: Possible longest valid concatenations are "chaers" ("cha" + "ers") and "acters" ("act" + "ers").

Example 3:

Input: arr = ["abcdefghijklmnopqrstuvwxyz"]
Output: 26
Explanation: The only string in arr has all 26 characters.

__Constraints:__

1 <= arr.length <= 16
1 <= arr[i].length <= 26
arr[i] contains only lowercase English letters.

__题目描述：__

给定一个字符串数组 arr，字符串 s 是将 arr 的含有 不同字母 的 子序列 字符串 连接 所得的字符串。

请返回所有可行解 s 中最长长度。

子序列 是一种可以从另一个数组派生而来的数组，通过删除某些元素或不删除元素而不改变其余元素的顺序。

__示例：__

示例 1：

输入：arr = ["un","iq","ue"]
输出：4
解释：所有可能的串联组合是：

- ""
- "un"
- "iq"
- "ue"
- "uniq" ("un" + "iq")
- "ique" ("iq" + "ue")

最大长度为 4。

示例 2：

输入：arr = ["cha","r","act","ers"]
输出：6
解释：可能的解答有 "chaers" 和 "acters"。

示例 3：

输入：arr = ["abcdefghijklmnopqrstuvwxyz"]
输出：26

__提示：__

1 <= arr.length <= 16
1 <= arr[i].length <= 26
arr[i] 中只含有小写英文字母

__思路：__

回溯 ➕ 状态压缩
由于只有 26 个小写字母, 可以用一个数字 mark 标记出现过的字符
mark |= (1 << (c - 'a')) 用来标记出现过的字符
记录当前的 mark 值, 表示之前的字符串中所有出现过的字符
如果 mark & (1 << (c - 'a')) 不为 0, 说明字符 c 已经出现过, 回溯, 丢弃当前的 mark 的值
遍历完字符串中所有字符就选择该字符, 或者不选择该字符而选择其他字符
取上述两种情况的较大值
时间复杂度为 O(2 ^ n), 空间复杂度为 O(2 ^ n)

__代码__:

__C++__:

```C++
class Solution 
{
private:
    int trackback(int start, int mark, vector<string>& arr)
    {
        if (start == arr.size()) return 0;
        int cur = mark, m = arr[start].size();
        for (const auto& c : arr[start])
        {
            if (mark & (1 << (c - 'a'))) return trackback(start + 1, cur, arr);
            mark |= (1 << (c - 'a'));
        }
        return max(m + trackback(start + 1, mark, arr), trackback(start + 1, cur, arr));
    }
public:
    int maxLength(vector<string>& arr) 
    {
        return trackback(0, 0, arr);
    }
};
```

__Java__:

```Java
class Solution {
    public int maxLength(List<String> arr) {
        return trackback(0, 0, arr);
    }
    
    private int trackback(int start, int mark, List<String> arr) {
        if (start == arr.size()) return 0;
        int cur = mark, m = arr.get(start).length();
        for (char c : arr.get(start).toCharArray()) {
            if ((mark & (1 << (c - 'a'))) != 0) return trackback(start + 1, cur, arr);
            mark |= (1 << (c - 'a'));
        }
        return Math.max(m + trackback(start + 1, mark, arr), trackback(start + 1, cur, arr));
    }
}
```

__Python__:

```Python
class Solution:
    def maxLength(self, arr: List[str]) -> int:
        n = len(arr)
        
        def trackback(start: int, mark: int) -> int:
            if start == n:
                return 0
            cur, m = mark, len(arr[start])
            for c in arr[start]:
                if mark & (1 << (ord(c) - ord('a'))):
                    return trackback(start + 1, cur)
                mark |= (1 << (ord(c) - ord('a')))
            return max(m + trackback(start + 1, mark), trackback(start + 1, cur))
        
        return trackback(0, 0)
```
