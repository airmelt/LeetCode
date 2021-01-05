# 859 Buddy Strings 亲密字符串

__Description__:
Given two strings A and B of lowercase letters, return true if and only if we can swap two letters in A so that the result equals B.

__Example:__

Example 1:

Input: A = "ab", B = "ba"
Output: true

Example 2:

Input: A = "ab", B = "ab"
Output: false

Example 3:

Input: A = "aa", B = "aa"
Output: true

Example 4:

Input: A = "aaaaaaabc", B = "aaaaaaacb"
Output: true

Example 5:

Input: A = "", B = "aa"
Output: false

__Note:__

0 <= A.length <= 20000
0 <= B.length <= 20000
A and B consist only of lowercase letters.

__题目描述__:
给定两个由小写字母构成的字符串 A 和 B ，只要我们可以通过交换 A 中的两个字母得到与 B 相等的结果，就返回 true ；否则返回 false 。

__示例 :__

示例 1：

输入： A = "ab", B = "ba"
输出： true

示例 2：

输入： A = "ab", B = "ab"
输出： false

示例 3:

输入： A = "aa", B = "aa"
输出： true

示例 4：

输入： A = "aaaaaaabc", B = "aaaaaaacb"
输出： true

示例 5：

输入： A = "", B = "aa"
输出： false

__提示：__

0 <= A.length <= 20000
0 <= B.length <= 20000
A 和 B 仅由小写字母构成。

__思路__:

首先, 如果 A和 B长度不相等, 直接返回false
如果 A等于 B, 只要 A中有重复字符, 返回true, 否则返回 false
如果 A不等于 B, 遍历 A, 找到两个下标, 尝试交换, 能交换返回 true, 其他情况都返回 false
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool buddyStrings(string A, string B) 
    {
        if (A.size() != B.size()) return false;
        if (A == B)
        {
            int count[26] = {0};
            for (char c : A) count[c - 'a']++;
            for (int i : count) if (i > 1) return true;
            return false;
        }
        int first = -1, second = -1;
        for (int i = 0; i < A.size(); i++)
        {
            if (A[i] != B[i])
            {
                if (first == -1) first = i;
                else if (second == -1) second = i;
                else return false;
            }
        }
        return second != -1 && A[first] == B[second] && A[second] == B[first];
    }
};
```

__Java__:

```Java
class Solution {
    public boolean buddyStrings(String A, String B) {
        if (A.length() != B.length()) return false;
        if (A.equals(B)) {
            int count[] = new int[26];
            for (char c : A.toCharArray()) count[c - 'a']++;
            for (int i : count) if (i > 1) return true;
            return false;
        }
        int first = -1, second = -1;
        for (int i = 0; i < A.length(); i++) {
            if (A.charAt(i) != B.charAt(i)) {
                if (first == -1) first = i;
                else if (second == -1) second = i;
                else return false;
            }
        }
        return second != -1 && A.charAt(first) == B.charAt(second) && A.charAt(second) == B.charAt(first);
    }
}
```

__Python__:

```Python
class Solution:
    def buddyStrings(self, A: str, B: str) -> bool:
        if len(A) != len(B):
            return False
        if A == B:
            return len(A) != len(set(A))
        d = [(a, b) for a, b in zip(A, B) if a != b]
        return len(d) == 2 and d[0] == d[1][::-1]
```
