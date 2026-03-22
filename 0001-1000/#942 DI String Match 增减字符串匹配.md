# 942 DI String Match 增减字符串匹配

__Description__:
Given a string S that only contains "I" (increase) or "D" (decrease), let N = S.length.

Return any permutation A of [0, 1, ..., N] such that for all i = 0, ..., N-1:

If S[i] == "I", then A[i] < A[i+1]
If S[i] == "D", then A[i] > A[i+1]

__Example:__

Example 1:

Input: "IDID"
Output: [0,4,1,3,2]

Example 2:

Input: "III"
Output: [0,1,2,3]

Example 3:

Input: "DDI"
Output: [3,2,0,1]

__Note:__
1 <= S.length <= 10000
S only contains characters "I" or "D".

__题目描述__:
给定只含 "I"（增大）或 "D"（减小）的字符串 S ，令 N = S.length。

返回 [0, 1, ..., N] 的任意排列 A 使得对于所有 i = 0, ..., N-1，都有：

如果 S[i] == "I"，那么 A[i] < A[i+1]
如果 S[i] == "D"，那么 A[i] > A[i+1]

__示例 :__

示例 1：

输出："IDID"
输出：[0,4,1,3,2]

示例 2：

输出："III"
输出：[0,1,2,3]

示例 3：

输出："DDI"
输出：[3,2,0,1]

__提示：__

1 <= S.length <= 1000
S 只包含字符 "I" 或 "D"。

__思路__:

双指针法, 遇到 D就将右侧的数字放入这个位置, 否则放入左侧的数字
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> diStringMatch(string S) 
    {
        int i = 0, j = S.size(), pos = 0;
        vector<int> result(S.size() + 1);
        for (auto s : S)
        {
            if (s == 'I') result[pos++] = i++;
            else result[pos++] = j--;
        }
        result[pos] = j;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] diStringMatch(String S) {
        int result[] = new int[S.length() + 1], i = 0, j = S.length(), pos = 0;
        for (char c : S.toCharArray()) {
            if (c == 'D') result[pos++] = j--;
            else result[pos++] = i++;
        }
        result[pos] = j;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def diStringMatch(self, S: str) -> List[int]:
        i, j, pos, result = 0, len(S), 0, [s for s in range(len(S) + 1)]
        for s in S:
            if s == 'I':
                result[pos] = i
                i += 1
            else:
                result[pos] = j
                j -= 1
            pos += 1
        result[pos] = j
        return result
```
