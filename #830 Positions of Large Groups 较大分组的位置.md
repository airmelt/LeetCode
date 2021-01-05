# 830 Positions of Large Groups 较大分组的位置

__Description__:
In a string S of lowercase letters, these letters form consecutive groups of the same character.

For example, a string like S = "abbxxxxzyy" has the groups "a", "bb", "xxxx", "z" and "yy".

Call a group large if it has 3 or more characters.  We would like the starting and ending positions of every large group.

The final answer should be in lexicographic order.

__Example:__

Example 1:

Input: "abbxxxxzzy"
Output: [[3,6]]
Explanation: "xxxx" is the single large group with starting  3 and ending positions 6.

Example 2:

Input: "abc"
Output: []
Explanation: We have "a","b" and "c" but no large group.

Example 3:

Input: "abcdddeeeeaabbbcd"
Output: [[3,5],[6,9],[12,14]]

__Note:__  1 <= S.length <= 1000

__题目描述__:
在一个由小写字母构成的字符串 S 中，包含由一些连续的相同字符所构成的分组。

例如，在字符串 S = "abbxxxxzyy" 中，就含有 "a", "bb", "xxxx", "z" 和 "yy" 这样的一些分组。

我们称所有包含大于或等于三个连续字符的分组为较大分组。找到每一个较大分组的起始和终止位置。

最终结果按照字典顺序输出。

__示例 :__

示例 1:

输入: "abbxxxxzzy"
输出: [[3,6]]
解释: "xxxx" 是一个起始于 3 且终止于 6 的较大分组。

示例 2:

输入: "abc"
输出: []
解释: "a","b" 和 "c" 均不是符合要求的较大分组。

示例 3:

输入: "abcdddeeeeaabbbcd"
输出: [[3,5],[6,9],[12,14]]

__说明：__  1 <= S.length <= 1000

__思路__:

遍历 S, 给 S加上一个哨兵, 方便处理边界条件
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> largeGroupPositions(string S) 
    {
        S += "A";
        vector<vector<int>> result;
        int j = 0;
        for (int i = 1; i < S.length(); i++) 
        {
            if (S[i] != S[j]) 
            {
                if (i - j > 2) result.push_back(vector<int>{j, i - 1});
                j = i;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> largeGroupPositions(String S) {
        S += "A";
        List<List<Integer>> result = new ArrayList<>();
        int j = 0;
        for (int i = 1; i < S.length(); i++) {
            if (S.charAt(i) != S.charAt(j)) {
                if (i - j > 2) result.add(new ArrayList<Integer>(Arrays.asList(j, i - 1)));
                j = i;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def largeGroupPositions(self, S: str) -> List[List[int]]:
        S += 'A'
        result, j = [], 0
        for i in range(1, len(S)):
            if S[i] != S[j]:
                if i - j > 2:
                    result.append([j, i - 1])
                j = i
        return result
```
