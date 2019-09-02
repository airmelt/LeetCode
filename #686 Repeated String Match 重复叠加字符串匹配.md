__Description__:
Given two strings A and B, find the minimum number of times A has to be repeated such that B is a substring of it. If no such solution, return -1.

__Example:__
For example, with A = "abcd" and B = "cdabcdab".

Return 3, because by repeating A three times (“abcdabcdabcd”), B is a substring of it; and B is not a substring of A repeated two times ("abcdabcd").

__Note:__
The length of A and B will be between 1 and 10000.

__题目描述__:
给定两个字符串 A 和 B, 寻找重复叠加字符串A的最小次数，使得字符串B成为叠加后的字符串A的子串，如果不存在则返回 -1。

__示例 :__
举个例子，A = "abcd"，B = "cdabcdab"。

答案为 3， 因为 A 重复叠加三遍后为 “abcdabcdabcd”，此时 B 是其子串；A 重复叠加两遍后为"abcdabcd"，B 并不是其子串。

__注意:__

 A 与 B 字符串的长度在1和10000区间范围内。

__思路__:
由于最长的字符串的形式最多是 A + B + A, 所以最长的字符串的长度, 也就是循环判断的终点应该是 2 * A + B
时间复杂度O(m + n), 空间复杂度O(m + n), m, n分别为 A和 B的长度

__代码__:
__C++__:
```
class Solution {
public:
    int repeatedStringMatch(string A, string B) {
        string s = A;
        int result = 1, max_length = 2 * A.size() + B.size();
        while (s.size() < max_length) {
            if (s.find(B) != string::npos) return result;
            s += A;
            result++;
        }
        return -1;
    }
};
```

__Java__:
```
class Solution {
    public int repeatedStringMatch(String A, String B) {
        StringBuilder sb = new StringBuilder(A);
        int result = 1, maxLength = A.length() * 2 + B.length();
        while (sb.length() < maxLength) {
            if (sb.toString().contains(B)) return result;
            result++;
            sb.append(A);
        }
        return -1;
    }
}
```

__Python__:
```
class Solution:
    def repeatedStringMatch(self, A: str, B: str) -> int:
        s, result = A, 1
        while len(s) < len(2 * A + B):
            if B in s:
                return result
            s += A
            result += 1
        return -1
```