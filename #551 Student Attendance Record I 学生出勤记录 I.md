__Description__:
You are given a string representing an attendance record for a student. The record only contains the following three characters:
'A' : Absent.
'L' : Late.
'P' : Present.
A student could be rewarded if his attendance record doesn't contain more than one 'A' (absent) or more than two continuous 'L' (late).

You need to return whether the student could be rewarded according to his attendance record.

__Example:__
Example 1:
Input: "PPALLP"
Output: True

Example 2:
Input: "PPALLL"
Output: False

__题目描述__:
给定一个字符串来代表一个学生的出勤记录，这个记录仅包含以下三个字符：

'A' : Absent，缺勤
'L' : Late，迟到
'P' : Present，到场
如果一个学生的出勤记录中不超过一个'A'(缺勤)并且不超过两个连续的'L'(迟到),那么这个学生会被奖赏。

你需要根据这个学生的出勤记录判断他是否会被奖赏。

__示例 :__
示例 1:

输入: "PPALLP"
输出: True
示例 2:

输入: "PPALLL"
输出: False

__思路__:
扫描一遍找到 A的数量, 然后判断 'LLL'是否在字符串中即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    bool checkRecord(string s) {
        int countA = 0;
        for (char c : s) {
            if (c == 'A') countA++;
            if (countA > 1) return false;
        }
        return s.find("LLL") == s.npos;
    }
};
```

__Java__:
```
class Solution {
    public boolean checkRecord(String s) {
        int countA = 0;
        for (char c : s.toCharArray()) {
            if (c == 'A') countA++;
            if (countA > 1) return false;
        }
        return s.indexOf("LLL") == -1;
    }
}
```

__Python__:
```
class Solution:
    def checkRecord(self, s: str) -> bool:
        return 'LLL' not in s and s.count('A') < 2
```
