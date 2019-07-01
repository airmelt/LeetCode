__Description__:
Count the number of segments in a string, where a segment is defined to be a contiguous sequence of non-space characters.

Please note that the string does not contain any non-printable characters.

__Example:__

Input: "Hello, my name is John"
Output: 5

__题目描述__:
统计字符串中的单词个数，这里的单词指的是连续的不是空格的字符。

请注意，你可以假定字符串里不包括任何不可打印的字符。

__示例：__

输入: "Hello, my name is John"
输出: 5

__思路__:
题目的含义是, 找到被空格隔开的部分的数量
参考[stringstream的用法](https://zhuanlan.zhihu.com/p/44435521)
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```
class Solution {
public:
    int countSegments(string s) {
        // 输入输出流
        stringstream ss(s);
        int result = 0;
        // 可以使用 cout << s << endl; 打印 s所传入的字符
        while (ss >> s) result++;
        return result;
    }
};
```

__Java__:
```
class Solution {
    public int countSegments(String s) {
		return s.trim().length() == 0 ? 0 : s.trim().split("\\s+").length;
    }
}
```

__Python__:
```
class Solution:
    def countSegments(self, s: str) -> int:
        return len(s.split())
```
