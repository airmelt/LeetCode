# 388 Longest Absolute File Path 文件的最长绝对路径

__Description__:
Suppose we have a file system that stores both files and directories. An example of one system is represented in the following picture:

![dir](https://assets.leetcode.com/uploads/2020/08/28/mdir.jpg)

Here, we have dir as the only directory in the root. dir contains two subdirectories, subdir1 and subdir2. subdir1 contains a file file1.ext and subdirectory subsubdir1. subdir2 contains a subdirectory subsubdir2, which contains a file file2.ext.

In text form, it looks like this (with ⟶ representing the tab character):

```text
dir
⟶ subdir1
⟶ ⟶ file1.ext
⟶ ⟶ subsubdir1
⟶ subdir2
⟶ ⟶ subsubdir2
⟶ ⟶ ⟶ file2.ext
```

If we were to write this representation in code, it will look like this: "dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext". Note that the '\n' and '\t' are the new-line and tab characters.

Every file and directory has a unique absolute path in the file system, which is the order of directories that must be opened to reach the file/directory itself, all concatenated by '/'s. Using the above example, the absolute path to file2.ext is "dir/subdir2/subsubdir2/file2.ext". Each directory name consists of letters, digits, and/or spaces. Each file name is of the form name.extension, where name and extension consist of letters, digits, and/or spaces.

Given a string input representing the file system in the explained format, return the length of the longest absolute path to a file in the abstracted file system. If there is no file in the system, return 0.

__Example:__

Example 1:

![dir 1](https://assets.leetcode.com/uploads/2020/08/28/dir1.jpg)

Input: input = "dir\n\tsubdir1\n\tsubdir2\n\t\tfile.ext"
Output: 20
Explanation: We have only one file, and the absolute path is "dir/subdir2/file.ext" of length 20.

Example 2:

![dir 2](https://assets.leetcode.com/uploads/2020/08/28/dir2.jpg)

Input: input = "dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext"
Output: 32
Explanation: We have two files:
"dir/subdir1/file1.ext" of length 21
"dir/subdir2/subsubdir2/file2.ext" of length 32.
We return 32 since it is the longest absolute path to a file.

Example 3:

Input: input = "a"
Output: 0
Explanation: We do not have any files, just a single directory named "a".

Example 4:

Input: input = "file1.txt\nfile2.txt\nlongfile.txt"
Output: 12
Explanation: There are 3 files at the root directory.
Since the absolute path for anything at the root directory is just the name itself, the answer is "longfile.txt" with length 12.

__Constraints:__

1 <= input.length <= 104
input may contain lowercase or uppercase English letters, a new line character '\n', a tab character '\t', a dot '.', a space ' ', and digits.

__题目描述__:
假设文件系统如下图所示：

![文件夹](https://assets.leetcode.com/uploads/2020/08/28/mdir.jpg)

这里将 dir 作为根目录中的唯一目录。dir 包含两个子目录 subdir1 和 subdir2 。subdir1 包含文件 file1.ext 和子目录 subsubdir1；subdir2 包含子目录 subsubdir2，该子目录下包含文件 file2.ext 。

在文本格式中，如下所示(⟶表示制表符)：

```text
dir
⟶ subdir1
⟶ ⟶ file1.ext
⟶ ⟶ subsubdir1
⟶ subdir2
⟶ ⟶ subsubdir2
⟶ ⟶ ⟶ file2.ext
```

如果是代码表示，上面的文件系统可以写为 "dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext" 。'\n' 和 '\t' 分别是换行符和制表符。

文件系统中的每个文件和文件夹都有一个唯一的 绝对路径 ，即必须打开才能到达文件/目录所在位置的目录顺序，所有路径用 '/' 连接。上面例子中，指向 file2.ext 的绝对路径是 "dir/subdir2/subsubdir2/file2.ext" 。每个目录名由字母、数字和/或空格组成，每个文件名遵循 name.extension 的格式，其中名称和扩展名由字母、数字和/或空格组成。

给定一个以上述格式表示文件系统的字符串 input ，返回文件系统中 指向文件的最长绝对路径 的长度。 如果系统中没有文件，返回 0。

__示例 :__

示例 1：

![文件夹 1](https://assets.leetcode.com/uploads/2020/08/28/dir1.jpg)

输入：input = "dir\n\tsubdir1\n\tsubdir2\n\t\tfile.ext"
输出：20
解释：只有一个文件，绝对路径为 "dir/subdir2/file.ext" ，路径长度 20
路径 "dir/subdir1" 不含任何文件

示例 2：

![文件夹 2](https://assets.leetcode.com/uploads/2020/08/28/dir2.jpg)

输入：input = "dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext"
输出：32
解释：存在两个文件：
"dir/subdir1/file1.ext" ，路径长度 21
"dir/subdir2/subsubdir2/file2.ext" ，路径长度 32
返回 32 ，因为这是最长的路径

示例 3：

输入：input = "a"
输出：0
解释：不存在任何文件

示例 4：

输入：input = "file1.txt\nfile2.txt\nlongfile.txt"
输出：12
解释：根目录下有 3 个文件。
因为根目录中任何东西的绝对路径只是名称本身，所以答案是 "longfile.txt" ，路径长度为 12

__提示：__

1 <= input.length <= 104
input 可能包含小写或大写的英文字母，一个换行符 '\n'，一个指表符 '\t'，一个点 '.'，一个空格 ' '，和数字。

__思路__:

按照换行符分割字符串
用一个字典记录层数及对应的长度, 每遇到一个 '\t', 层数加 1
每次需要找到字符 ‘.’才算找到文件
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int lengthLongestPath(string input) 
    {
        int result = 0, level = 1, count = 0, is_file = 0, i = 0, n = input.size();
        unordered_map<int, int> m;
        m[0] = 0;
        while (i < n)
        {
            while (input[i] == '\t')
            {
                ++level;
                ++i;
            }
            while (i < n and input[i] != '\n') 
            {
                if (input[i] == '.') is_file = 1;
                ++count;
                ++i;
            }
            if (is_file) result = max(result, m[level - 1] + count);
            else m[level] = m[level - 1] + count + 1;
            count = 0;
            level = 1;
            is_file = 0;
            ++i;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int lengthLongestPath(String input) {
        String files[] = input.split("\n");
        int result = 0, fileLens[] = new int[files.length + 1];
        fileLens[0] = -1;
        for (String file : files) {
            int level = file.lastIndexOf('\t') + 2;
            int cur = file.length() - level + 1;
            fileLens[level] = fileLens[level - 1] + 1 + cur;
            if (file.contains(".")) result = Math.max(result, fileLens[level]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def lengthLongestPath(self, input: str) -> int:
        dp, result = [0], 0
        for s in input.split('\n'):
            level = 0
            while s[level] == '\t':
                level += 1
            cur = len(s) - level
            if '.' in s:
                result = max(result, dp[level] + cur)
                continue
            if level + 1 < len(dp):
                dp[level + 1] = dp[level] + cur + 1
            else:
                dp.append(dp[-1] + cur + 1)
        return result
```
