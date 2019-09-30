__Description__:
We are given two strings, A and B.

A shift on A consists of taking string A and moving the leftmost character to the rightmost position. For example, if A = 'abcde', then it will be 'bcdea' after one shift on A. Return True if and only if A can become B after some number of shifts on A.

__Example:__
Example 1:
Input: A = 'abcde', B = 'cdeab'
Output: true

Example 2:
Input: A = 'abcde', B = 'abced'
Output: false

__Note:__

A and B will have length at most 100.

__题目描述__:
给定两个字符串, A 和 B。

A 的旋转操作就是将 A 最左边的字符移动到最右边。 例如, 若 A = 'abcde'，在移动一次之后结果就是'bcdea' 。如果在若干次旋转操作之后，A 能变成B，那么返回True。

__示例 :__
示例 1:
输入: A = 'abcde', B = 'cdeab'
输出: true

示例 2:
输入: A = 'abcde', B = 'abced'
输出: false

__注意：__

A 和 B 长度不超过 100。

__思路__:
1. 如果旋转之后 B和 A相同, 只要 B在 A + A中即可
2. 参考[LeetCode #189 Rotate Array 旋转数组](https://www.jianshu.com/p/15443e6ea0de), 对 A进行旋转即可
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```
class Solution {
public:
    bool rotateString(string A, string B) {
        return A.size() == B.size() && (A + A).find(B) != -1;
    }
};
```

__Java__:
```
class Solution {
    public boolean rotateString(String A, String B) {
        return A.length() == B.length() && (A + A).contains(B);
    }
}
```

__Python__:
```
class Solution:
    def rotateString(self, A: str, B: str) -> bool:
        return len(A) == len(B) and B in A + A
```