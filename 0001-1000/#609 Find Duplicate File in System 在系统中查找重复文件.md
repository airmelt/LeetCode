# 609 Find Duplicate File in System 在系统中查找重复文件

__Description__:
Given a list paths of directory info, including the directory path, and all the files with contents in this directory, return all the duplicate files in the file system in terms of their paths. You may return the answer in any order.

A group of duplicate files consists of at least two files that have the same content.

A single directory info string in the input list has the following format:

"root/d1/d2/.../dm f1.txt(f1_content) f2.txt(f2_content) ... fn.txt(fn_content)"
It means there are n files (f1.txt, f2.txt ... fn.txt) with content (f1_content, f2_content ... fn_content) respectively in the directory "root/d1/d2/.../dm". Note that n >= 1 and m >= 0. If m = 0, it means the directory is just the root directory.

The output is a list of groups of duplicate file paths. For each group, it contains all the file paths of the files that have the same content. A file path is a string that has the following format:

"directory_path/file_name.txt"

__Example:__

Example 1:

Input: paths = ["root/a 1.txt(abcd) 2.txt(efgh)","root/c 3.txt(abcd)","root/c/d 4.txt(efgh)","root 4.txt(efgh)"]
Output: [["root/a/2.txt","root/c/d/4.txt","root/4.txt"],["root/a/1.txt","root/c/3.txt"]]

Example 2:

Input: paths = ["root/a 1.txt(abcd) 2.txt(efgh)","root/c 3.txt(abcd)","root/c/d 4.txt(efgh)"]
Output: [["root/a/2.txt","root/c/d/4.txt"],["root/a/1.txt","root/c/3.txt"]]

__Constraints:__

1 <= paths.length <= 2 \* 10^4
1 <= paths[i].length <= 3000
1 <= sum(paths[i].length) <= 5 \* 10^5
paths[i] consist of English letters, digits, '/', '.', '(', ')', and ' '.
You may assume no files or directories share the same name in the same directory.
You may assume each given directory info represents a unique directory. A single blank space separates the directory path and file info.

__Follow up:__

Imagine you are given a real file system, how will you search files? DFS or BFS?
If the file content is very large (GB level), how will you modify your solution?
If you can only read the file by 1kb each time, how will you modify your solution?
What is the time complexity of your modified solution? What is the most time-consuming part and memory-consuming part of it? How to optimize?
How to make sure the duplicated files you find are not false positive?

__题目描述__:
给定一个目录信息列表，包括目录路径，以及该目录中的所有包含内容的文件，您需要找到文件系统中的所有重复文件组的路径。一组重复的文件至少包括二个具有完全相同内容的文件。

输入列表中的单个目录信息字符串的格式如下：

"root/d1/d2/.../dm f1.txt(f1_content) f2.txt(f2_content) ... fn.txt(fn_content)"

这意味着有 n 个文件（f1.txt, f2.txt ... fn.txt 的内容分别是 f1_content, f2_content ... fn_content）在目录 root/d1/d2/.../dm 下。注意：n>=1 且 m>=0。如果 m=0，则表示该目录是根目录。

该输出是重复文件路径组的列表。对于每个组，它包含具有相同内容的文件的所有文件路径。文件路径是具有下列格式的字符串：

"directory_path/file_name.txt"

__示例 :__

示例 1：

输入：
["root/a 1.txt(abcd) 2.txt(efgh)", "root/c 3.txt(abcd)", "root/c/d 4.txt(efgh)", "root 4.txt(efgh)"]
输出：  
[["root/a/2.txt","root/c/d/4.txt","root/4.txt"],["root/a/1.txt","root/c/3.txt"]]

__注：__

最终输出不需要顺序。
您可以假设目录名、文件名和文件内容只有字母和数字，并且文件内容的长度在 [1，50] 的范围内。
给定的文件数量在 [1，20000] 个范围内。
您可以假设在同一目录中没有任何文件或目录共享相同的名称。
您可以假设每个给定的目录信息代表一个唯一的目录。目录路径和文件信息用一个空格分隔。

__超越竞赛的后续行动：__

假设您有一个真正的文件系统，您将如何搜索文件？广度搜索还是宽度搜索？
如果文件内容非常大（GB级别），您将如何修改您的解决方案？
如果每次只能读取 1 kb 的文件，您将如何修改解决方案？
修改后的解决方案的时间复杂度是多少？其中最耗时的部分和消耗内存的部分是什么？如何优化？
如何确保您发现的重复文件不是误报？

__思路__:

用一个 hash 表记录文件中的内容及对应的文件绝对路径
将文件路径数大于 1 的加入结果
时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<string>> findDuplicate(vector<string>& paths) 
    {
        unordered_map<string, vector<string>> m;
        for (auto &path : paths)
        {
            stringstream ssin(path);
            string root, file, name, context;
            ssin >> root;
            while(ssin >> file)
            {
                int x = file.find('('), y = file.find(')');
                name = file.substr(0, x);
                context = file.substr(x + 1, y - x - 1);
                m[context].emplace_back(root + '/' + name);
            }
        }
        vector<vector<string>> result;
        for (auto &[k, v] : m) if (v.size() > 1) result.emplace_back(v);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<String>> findDuplicate(String[] paths) {
        List<List<String>> list = new ArrayList<>(), result = new ArrayList<>();
        Map<String, Integer> map = new HashMap<>();
        int index = 0;
        for (String path : paths) {
            String[] files = path.split(" ");
            for (int i = 1; i < files.length; i++) {
                String content = files[i].substring(files[i].indexOf("(") + 1, files[i].indexOf(")"));
                if (!map.containsKey(content)) {
                    map.put(content, index++);
                    list.add(new ArrayList<String>());
                }
                list.get(map.get(content)).add(files[0] + "/" + files[i].substring(0, files[i].indexOf("(")));
            }
        }
        for (int i = 0; i < list.size(); i++) if (list.get(i).size() > 1) result.add(list.get(i));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findDuplicate(self, paths: List[str]) -> List[List[str]]:
        d = defaultdict(list)
        for path in paths:
            arr = path.split()
            for i in range(1, len(arr)):
                t = arr[i].split('(')
                d[t[1]].append(arr[0] + '/' + t[0])
        return [i for i in d.values() if len(i) > 1]
```
