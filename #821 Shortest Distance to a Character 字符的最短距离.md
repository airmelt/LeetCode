__Description__:
Given a string S and a character C, return an array of integers representing the shortest distance from the character C in the string.

__Example:__
Example 1:

Input: S = "loveleetcode", C = 'e'
Output: [3, 2, 1, 0, 1, 0, 0, 1, 2, 2, 1, 0]
 
__Note:__

S string length is in [1, 10000].
C is a single character, and guaranteed to be in string S.
All letters in S and C are lowercase.

__题目描述__:
给定一个字符串 S 和一个字符 C。返回一个代表字符串 S 中每个字符到字符串 S 中的字符 C 的最短距离的数组。

__示例 :__
示例 1:

输入: S = "loveleetcode", C = 'e'
输出: [3, 2, 1, 0, 1, 0, 0, 1, 2, 2, 1, 0]

__说明：__

字符串 S 的长度范围为 [1, 10000]。
C 是一个单字符，且保证是字符串 S 里的字符。
S 和 C 中的所有字母均为小写字母。

__思路__:
用一个指针记录此前出现的 C的位置, 从左向右和从右向左遍历两次 S
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution {
public:
    vector<int> shortestToChar(string S, char C) {
        vector<int> result(S.size());
        int pre = 10001;
        for (int i = 0; i < S.size(); i++) 
        {
            if (S[i] == C) 
            {
                result[i] = 0;
                pre = 0;
            } 
            else 
            {
                if (pre < 10001) result[i] = ++pre;
                else result[i] = pre;
            }
        }
        pre = result[result.size() - 1] ? 10001 : 0;
        for (int i = S.size() - 1; i > -1; i--) 
        {
            if (result[i] == 0) pre = 0;
            else if (pre < 10001) result[i] = min(++pre, result[i]);
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int[] shortestToChar(String S, char C) {
        int result[] = new int[S.length()], pre = 10001;
        for (int i = 0; i < S.length(); i++) {
            if (S.charAt(i) == C) {
                result[i] = 0;
                pre = 0;
            } else {
                if (pre < 10001) result[i] = ++pre;
                else result[i] = pre;
            }
        }
        pre = result[result.length - 1] == 0 ? 0 : 10001;
        for (int i = S.length() - 1; i > -1; i--) {
            if (result[i] == 0) pre = 0;
            else if (pre < 10001) result[i] = Math.min(++pre, result[i]);
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def shortestToChar(self, S: str, C: str) -> List[int]:
        return [min([abs(i - j) for j in [k for k, c in enumerate(S) if c == C]]) for i in range(len(S))]
```