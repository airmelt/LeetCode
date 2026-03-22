# 893 Groups of Special-Equivalent Strings 特殊等价字符串组

__Description__:
You are given an array A of strings.

Two strings S and T are special-equivalent if after any number of moves, S == T.

A move consists of choosing two indices i and j with i % 2 == j % 2, and swapping S[i] with S[j].

Now, a group of special-equivalent strings from A is a non-empty subset S of A such that any string not in S is not special-equivalent with any string in S.

Return the number of groups of special-equivalent strings from A.

__Example:__

Example 1:

Input: ["a","b","c","a","c","c"]
Output: 3
Explanation: 3 groups ["a","a"], ["b"], ["c","c","c"]

Example 2:

Input: ["aa","bb","ab","ba"]
Output: 4
Explanation: 4 groups ["aa"], ["bb"], ["ab"], ["ba"]

Example 3:

Input: ["abc","acb","bac","bca","cab","cba"]
Output: 3
Explanation: 3 groups ["abc","cba"], ["acb","bca"], ["bac","cab"]

Example 4:

Input: ["abcd","cdab","adcb","cbad"]
Output: 1
Explanation: 1 group ["abcd","cdab","adcb","cbad"]

__Note:__

1 <= A.length <= 1000
1 <= A[i].length <= 20
All A[i] have the same length.
All A[i] consist of only lowercase letters.

__题目描述__:
你将得到一个字符串数组 A。

如果经过任意次数的移动，S == T，那么两个字符串 S 和 T 是特殊等价的。

一次移动包括选择两个索引 i 和 j，且 i ％ 2 == j ％ 2，交换 S[j] 和 S [i]。

现在规定，A 中的特殊等价字符串组是 A 的非空子集 S，这样不在 S 中的任何字符串与 S 中的任何字符串都不是特殊等价的。

返回 A 中特殊等价字符串组的数量。

__示例 :__

示例 1：

输入：["a","b","c","a","c","c"]
输出：3
解释：3 组 ["a","a"]，["b"]，["c","c","c"]

示例 2：

输入：["aa","bb","ab","ba"]
输出：4
解释：4 组 ["aa"]，["bb"]，["ab"]，["ba"]

示例 3：

输入：["abc","acb","bac","bca","cab","cba"]
输出：3
解释：3 组 ["abc","cba"]，["acb","bca"]，["bac","cab"]

示例 4：

输入：["abcd","cdab","adcb","cbad"]
输出：1
解释：1 组 ["abcd","cdab","adcb","cbad"]

__提示：__

1 <= A.length <= 1000
1 <= A[i].length <= 20
所有 A[i] 都具有相同的长度。
所有 A[i] 都只由小写字母组成。

__思路__:
注意到数组 A中的各个元素长度相等
将奇数位置和偶数位置上的字符分别拿出来统计各字符出现的次数
出现次数不同的分到不同的组
用 set统计分组结果
时间复杂度O(nm), 空间复杂度O(n), 其中 n为数组 A的长度, m为 A[0]的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numSpecialEquivGroups(vector<string>& A) 
    {
        set<pair<vector<int>,vector<int>>> s;
        for (auto a : A)
        {
            vector<int> odd(26), even(26);
            for (int i = 0; i < a.size(); i++) ++odd[a[i++] - 'a'];
            for (int i = 1; i < a.size(); i++) ++even[a[i++] - 'a'];     
            s.insert({odd, even});
        }
        return s.size();
    }
};
```

__Java__:

```Java
class Solution {
    public int numSpecialEquivGroups(String[] A) {
        Set<String> set = new HashSet<>();
        for (String a : A) set.add(helper(a));
        return set.size();
    }
    
    private String helper(String a) {
        int[] count = new int[52];
        for (int i = 0; i < a.length(); i++) count[a.charAt(i) - 'a' + 26 * (i & 1)]++;
        return Arrays.toString(count);
    }
}
```

__Python__:

```Python
class Solution:
    def numSpecialEquivGroups(self, A: List[str]) -> int:
        return len(set((''.join(sorted(item[0::2]) + sorted(item[1::2])) for item in A)))
```
