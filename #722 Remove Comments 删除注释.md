# 722 Remove Comments 删除注释

__Description__:
Given a C++ program, remove comments from it. The program source is an array of strings source where source[i] is the ith line of the source code. This represents the result of splitting the original source code string by the newline character '\n'.

In C++, there are two types of comments, line comments, and block comments.

The string "//" denotes a line comment, which represents that it and the rest of the characters to the right of it in the same line should be ignored.
The string "/*" denotes a block comment, which represents that all characters until the next (non-overlapping) occurrence of "*/" should be ignored. (Here, occurrences happen in reading order: line by line from left to right.) To be clear, the string "/*/" does not yet end the block comment, as the ending would be overlapping the beginning.
The first effective comment takes precedence over others.

For example, if the string "//" occurs in a block comment, it is ignored.
Similarly, if the string "/*" occurs in a line or block comment, it is also ignored.
If a certain line of code is empty after removing comments, you must not output that line: each string in the answer list will be non-empty.

There will be no control characters, single quote, or double quote characters.

For example, source = "string s = "/\* Not a comment. \*/";" will not be a test case.
Also, nothing else such as defines or macros will interfere with the comments.

It is guaranteed that every open block comment will eventually be closed, so "/*" outside of a line or block comment always starts a new comment.

Finally, implicit newline characters can be deleted by block comments. Please see the examples below for details.

After removing the comments from the source code, return the source code in the same format.

__Example:__

Example 1:

Input: source = ["/*Test program */", "int main()", "{ ", "  // variable declaration ", "int a, b, c;", "/* This is a test", "   multiline  ", "   comment for ", "   testing */", "a = b + c;", "}"]
Output: ["int main()","{ ","  ","int a, b, c;","a = b + c;","}"]
Explanation: The line by line code is visualized as below:

```C++
/*Test program */
int main()
{ 
  // variable declaration 
int a, b, c;
/* This is a test
   multiline  
   comment for 
   testing */
a = b + c;
}
```

The string /* denotes a block comment, including line 1 and lines 6-9. The string // denotes line 4 as comments.
The line by line output code is visualized as below:

```C++
int main()
{ 
  
int a, b, c;
a = b + c;
}
```

Example 2:

Input: source = ["a/*comment", "line", "more_comment*/b"]
Output: ["ab"]
Explanation: The original source string is "a/*comment\nline\nmore_comment*/b", where we have bolded the newline characters.  After deletion, the implicit newline characters are deleted, leaving the string "ab", which when delimited by newline characters becomes ["ab"].

__Constraints:__

1 <= source.length <= 100
0 <= source[i].length <= 80
source[i] consists of printable ASCII characters.
Every open block comment is eventually closed.
There are no single-quote or double-quote in the input.

__题目描述__:
给一个 C++ 程序，删除程序中的注释。这个程序source是一个数组，其中source[i]表示第i行源码。 这表示每行源码由\n分隔。

在 C++ 中有两种注释风格，行内注释和块注释。

字符串// 表示行注释，表示//和其右侧的其余字符应该被忽略。

字符串/\* 表示一个块注释，它表示直到\*/的下一个（非重叠）出现的所有字符都应该被忽略。（阅读顺序为从左到右）非重叠是指，字符串/*/并没有结束块注释，因为注释的结尾与开头相重叠。

第一个有效注释优先于其他注释：如果字符串//出现在块注释中会被忽略。 同样，如果字符串/*出现在行或块注释中也会被忽略。

如果一行在删除注释之后变为空字符串，那么不要输出该行。即，答案列表中的每个字符串都是非空的。

样例中没有控制字符，单引号或双引号字符。比如，source = "string s = "/\* Not a comment. \*/";" 不会出现在测试样例里。（此外，没有其他内容（如定义或宏）会干扰注释。）

我们保证每一个块注释最终都会被闭合， 所以在行或块注释之外的/*总是开始新的注释。

最后，隐式换行符可以通过块注释删除。 有关详细信息，请参阅下面的示例。

从源代码中删除注释后，需要以相同的格式返回源代码。

__示例 :__

示例 1:

输入:
source = ["/*Test program */", "int main()", "{ ", "  // variable declaration ", "int a, b, c;", "/* This is a test", "   multiline  ", "   comment for ", "   testing */", "a = b + c;", "}"]

示例代码可以编排成这样:

```C++
/*Test program */
int main()
{ 
  // variable declaration 
int a, b, c;
/* This is a test
   multiline  
   comment for 
   testing */
a = b + c;
}
```

输出: ["int main()","{ ","  ","int a, b, c;","a = b + c;","}"]

编排后:

```C++
int main()
{ 
  
int a, b, c;
a = b + c;
}
```

解释:
第 1 行和第 6-9 行的字符串 /* 表示块注释。第 4 行的字符串 // 表示行注释。

示例 2:

输入:
source = ["a/*comment", "line", "more_comment*/b"]
输出: ["ab"]
解释: 原始的 source 字符串是 "a/*comment\nline\nmore_comment*/b", 其中我们用粗体显示了换行符。删除注释后，隐含的换行符被删除，留下字符串 "ab" 用换行符分隔成数组时就是 ["ab"].

__注意:__

source的长度范围为[1, 100].
source[i]的长度范围为[0, 80].
每个块注释都会被闭合。
给定的源码中不会有单引号、双引号或其他控制字符。

__思路__:

字符串处理
先用 '\n' 作为分隔符将所有字符串连成一个字符串, 注意 '\n' 的长度为 1
遍历字符串, 从当前位置开始搜索 '/', 结果为 j, 判断下一个字符
如果是 '/' 说明是单行注释, 删除 j 开始到第一个换行符(包括)的所有字符
如果是 '*' 说明是多行注释, 删除 j 开始到第一个 "*/" (包括)的所有字符
否则说明 '/' 是一个普通字符 i 移动到 j + 2(因为 j + 1 已经判断过不可能是 '/')
最后按照换行符 split 字符串, 空字符串跳过
时间复杂度为 O(mn), 空间复杂度为 O(mn), m 为字符串的长度, n 为 source 数组中的字符串数

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> removeComments(vector<string>& source) 
    {
        string program = "", buf;
        for (auto s : source) program += s + "\n";
        for (int i = 0; i < program.size();)
        {
            int j = program.find("/", i);
            if (j == -1) break;
            auto c = program[j + 1];
            if (c == '/')
            {
                program.erase(j, program.find("\n", j + 2) - j);
                i = j;
            }
            else if (c == '*')
            {
                program.erase(j, program.find("*/", j + 2) + 2 - j);
                i = j;
            }
            else i = j + 2;
        }
        vector<string> result;
        stringstream ss(program);
        while (getline(ss, buf, '\n')) if (!buf.empty()) result.emplace_back(buf);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> removeComments(String[] source) {
        StringBuilder sb = new StringBuilder(String.join("\n", source)).append("\n");
        for (int i = 0; i < sb.length(); ) {
            int j = sb.indexOf("/", i);
            if (j == -1) break;
            char ch = sb.charAt(j + 1);
            if (ch == '/') {
                sb.delete(j, sb.indexOf("\n", j + 2));
                i = j;
            } else if (ch == '*') {
                sb.delete(j, sb.indexOf("*/", j + 2) + 2);
                i = j;
            } else i = j + 2;
        }
        List<String> result = new ArrayList<>();
        for (String line : sb.toString().split("\n")) if (!line.isEmpty()) result.add(line);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def removeComments(self, source: List[str]) -> List[str]:
        return [line for line in re.sub(r'(/\*.*?\*/|//[^\n]*)', '', '\n'.join(source), flags=re.S).splitlines() if line]
```
