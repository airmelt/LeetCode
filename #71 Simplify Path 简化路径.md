# 71 Simplify Path 简化路径

__Description__:
Given an absolute path for a file (Unix-style), simplify it. Or in other words, convert it to the canonical path.

In a UNIX-style file system, a period . refers to the current directory. Furthermore, a double period .. moves the directory up a level.

Note that the returned canonical path must always begin with a slash /, and there must be only a single slash / between two directory names. The last directory name (if it exists) must not end with a trailing /. Also, the canonical path must be the shortest string representing the absolute path.

__Example:__

Example 1:

Input: "/home/"
Output: "/home"
Explanation: Note that there is no trailing slash after the last directory name.

Example 2:

Input: "/../"
Output: "/"
Explanation: Going one level up from the root directory is a no-op, as the root level is the highest level you can go.

Example 3:

Input: "/home//foo/"
Output: "/home/foo"
Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.

Example 4:

Input: "/a/./b/../../c/"
Output: "/c"

Example 5:

Input: "/a/../../b/../c//.//"
Output: "/c"

Example 6:

Input: "/a//b////c/d//././/.."
Output: "/a/b/c"

__题目描述__:
以 Unix 风格给出一个文件的绝对路径，你需要简化它。或者换句话说，将其转换为规范路径。

在 Unix 风格的文件系统中，一个点（.）表示当前目录本身；此外，两个点 （..） 表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。更多信息请参阅：Linux / Unix中的绝对路径 vs 相对路径

请注意，返回的规范路径必须始终以斜杠 / 开头，并且两个目录名之间必须只有一个斜杠 /。最后一个目录名（如果存在）不能以 / 结尾。此外，规范路径必须是表示绝对路径的最短字符串。

__示例 :__

示例 1：

输入："/home/"
输出："/home"
解释：注意，最后一个目录名后面没有斜杠。

示例 2：

输入："/../"
输出："/"
解释：从根目录向上一级是不可行的，因为根是你可以到达的最高级。

示例 3：

输入："/home//foo/"
输出："/home/foo"
解释：在规范路径中，多个连续斜杠需要用一个斜杠替换。

示例 4：

输入："/a/./b/../../c/"
输出："/c"

示例 5：

输入："/a/../../b/../c//.//"
输出："/c"

示例 6：

输入："/a//b////c/d//././/.."
输出："/a/b/c"

__思路__:

利用栈存储
以 '/'作为分隔符插入输入字符, 碰到 '..'且栈非空就弹出
碰到 '.'就跳过
其余的插入
最后将栈中的元素插入结果
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string simplifyPath(string path) 
    {
        vector<string> temp;
        string result;
        istringstream iss(path);
        while (getline(iss, result, '/'))
        {
            if (!result.empty() and result != "." and result != "..") temp.push_back(result);
            else if (!temp.empty() and result == "..") temp.pop_back();
        }
        result.clear();
        if (temp.empty()) result = "/";
        for (string &s: temp) result += "/" + s;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String simplifyPath(String path) {
        String paths[] = path.split("/");
        List<String> temp = new ArrayList<>();
        for (String s : paths) {
            if (!s.isEmpty() && !".".equals(s) && !"..".equals(s)) temp.add(s);
            else if (!temp.isEmpty() && "..".equals(s)) temp.remove(temp.size() - 1);
        }
        if (temp.isEmpty()) return "/";
        return "/" + String.join("/", temp);
    }
}
```

__Python__:

```Python
class Solution:
    def simplifyPath(self, path: str) -> str:
        return "/" + "/".join(functools.reduce(lambda x, y: x[:-1] if y == ".." else x + [y] if y and y != "." else x, path.split("/"), []))
```
