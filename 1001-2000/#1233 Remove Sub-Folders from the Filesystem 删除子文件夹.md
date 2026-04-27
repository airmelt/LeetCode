# 1233 Remove Sub-Folders from the Filesystem 删除子文件夹

__Description:__

Given a list of folders folder, return the folders after removing all sub-folders in those folders. You may return the answer in any order.

If a folder[i] is located within another folder[j], it is called a sub-folder of it.

The format of a path is one or more concatenated strings of the form: '/' followed by one or more lowercase English letters.

For example, "/leetcode" and "/leetcode/problems" are valid paths while an empty string and "/" are not.

__Example:__

Example 1:

Input: folder = ["/a","/a/b","/c/d","/c/d/e","/c/f"]
Output: ["/a","/c/d","/c/f"]
Explanation: Folders "/a/b" is a subfolder of "/a" and "/c/d/e" is inside of folder "/c/d" in our filesystem.

Example 2:

Input: folder = ["/a","/a/b/c","/a/b/d"]
Output: ["/a"]
Explanation: Folders "/a/b/c" and "/a/b/d" will be removed because they are subfolders of "/a".

Example 3:

Input: folder = ["/a/b/c","/a/b/ca","/a/b/d"]
Output: ["/a/b/c","/a/b/ca","/a/b/d"]

__Constraints:__

1 <= folder.length <= 4 * 10^4
2 <= folder[i].length <= 100
folder[i] contains only lowercase letters and '/'.
folder[i] always starts with the character '/'.
Each folder name is unique.

__题目描述：__

你是一位系统管理员，手里有一份文件夹列表 folder，你的任务是要删除该列表中的所有 子文件夹，并以 任意顺序 返回剩下的文件夹。

如果文件夹 folder[i] 位于另一个文件夹 folder[j] 下，那么 folder[i] 就是 folder[j] 的 子文件夹 。

文件夹的「路径」是由一个或多个按以下格式串联形成的字符串：'/' 后跟一个或者多个小写英文字母。

例如，"/leetcode" 和 "/leetcode/problems" 都是有效的路径，而空字符串和 "/" 不是。

__示例：__

示例 1：

输入：folder = ["/a","/a/b","/c/d","/c/d/e","/c/f"]
输出：["/a","/c/d","/c/f"]
解释："/a/b/" 是 "/a" 的子文件夹，而 "/c/d/e" 是 "/c/d" 的子文件夹。

示例 2：

输入：folder = ["/a","/a/b/c","/a/b/d"]
输出：["/a"]
解释：文件夹 "/a/b/c" 和 "/a/b/d/" 都会被删除，因为它们都是 "/a" 的子文件夹。

示例 3：

输入: folder = ["/a/b/c","/a/b/ca","/a/b/d"]
输出: ["/a/b/c","/a/b/ca","/a/b/d"]

__提示：__

1 <= folder.length <= 4 * 10^4
2 <= folder[i].length <= 100
folder[i] 只包含小写字母和 '/'
folder[i] 总是以字符 '/' 起始
每个文件夹名都是 唯一 的

__思路：__

排序
由于 'z' > '/', 并且所有文件都是唯一的, 所以排序之后一定有父文件夹在子文件夹前面
然后再比较字符串开头即可
时间复杂度为 O(nlgn), 空间复杂度为 O(1), n 表示 folder 数组的大小, 因为字符串的长度比较短, 主要开销在排序上

__代码__:

__C++__:

```C++
class Solution 
{
public:
    vector<string> removeSubfolders(vector<string>& folder) 
    {
        vector<string> result;
        sort(folder.begin(), folder.end());
        string pre = "/";
        for (const auto& f : folder) if (f.find(pre) or f[pre.size()] != '/') result.emplace_back(pre = f);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> removeSubfolders(String[] folder) {
        List<String> result = new ArrayList<>();
        Arrays.sort(folder);
        String pre = "/";
        for (String f : folder) if (f.indexOf(pre) != 0 || f.charAt(pre.length()) != '/') result.add(pre = f);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def removeSubfolders(self, folder: List[str]) -> List[str]:
        result, pre = [], '//'
        for f in sorted(folder):
            if not f.startswith(pre):
                result.append((pre := f + '/')[:-1])
        return result
```
