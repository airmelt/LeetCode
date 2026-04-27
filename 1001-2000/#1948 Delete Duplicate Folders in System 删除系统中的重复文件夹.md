# 1948 Delete Duplicate Folders in System 删除系统中的重复文件夹

__Description:__

Due to a bug, there are many duplicate folders in a file system. You are given a 2D array `paths`, where `paths[i]` is an array representing an absolute path to the `i ^ th` folder in the file system.

- For example, `["one", "two", "three"]` represents the path `"/one/two/three"`.

Two folders (not necessarily on the same level) are identical if they contain the same non-empty set of identical subfolders and underlying subfolder structure. The folders do not need to be at the root level to be identical. If two or more folders are identical, then mark the folders as well as all their subfolders.

- For example, folders `"/a"` and `"/b"` in the file structure below are identical. They (as well as their subfolders) should __all__ be marked:

- `/a`
- `/a/x`
- `/a/x/y`
- `/a/z`
- `/b`
- `/b/x`
- `/b/x/y`
- `/b/z`
- However, if the file structure also included the path `"/b/w"`, then the folders `"/a"` and `"/b"` would not be identical. Note that `"/a/x"` and `"/b/x"` would still be considered identical even with the added folder.

Once all the identical folders and their subfolders have been marked, the file system will delete all of them. The file system only runs the deletion once, so any folders that become identical after the initial deletion are not deleted.

Return _the 2D array_ `ans` _containing the paths of the __remaining__ folders after deleting all the marked folders. The paths may be returned in __any__ order_.

__Example:__

Example 1:

![1948-1](https://assets.leetcode.com/uploads/2021/07/19/lc-dupfolder1.jpg)

```text
Input: paths = [["a"],["c"],["d"],["a","b"],["c","b"],["d","a"]]
Output: [["d"],["d","a"]]
Explanation: The file structure is as shown.
Folders "/a" and "/c" (and their subfolders) are marked for deletion because they both contain an empty
folder named "b".
```

Example 2:

![1948-2](https://assets.leetcode.com/uploads/2021/07/19/lc-dupfolder2.jpg)

```text
Input: paths = [["a"],["c"],["a","b"],["c","b"],["a","b","x"],["a","b","x","y"],["w"],["w","y"]]
Output: [["c"],["c","b"],["a"],["a","b"]]
Explanation: The file structure is as shown. 
Folders "/a/b/x" and "/w" (and their subfolders) are marked for deletion because they both contain an empty folder named "y".
Note that folders "/a" and "/c" are identical after the deletion, but they are not deleted because they were not marked beforehand.
```

Example 3:

![1948-3](https://assets.leetcode.com/uploads/2021/07/19/lc-dupfolder3.jpg)

```text
Input: paths = [["a","b"],["c","d"],["c"],["a"]]
Output: [["c"],["c","d"],["a"],["a","b"]]
Explanation: All folders are unique in the file system.
Note that the returned array can be in a different order as the order does not matter.
```

__Constraints:__

- `1 <= paths.length <= 2 * 10 ^ 4`
- `1 <= paths[i].length <= 500`
- `1 <= paths[i][j].length <= 10`
- `1 <= sum(paths[i][j].length) <= 2 * 10 ^ 5`
- `path[i][j]` consists of lowercase English letters.
- No two paths lead to the same folder.
- For any folder not at the root level, its parent folder will also be in the input.

__题目描述:__

由于一个漏洞，文件系统中存在许多重复文件夹。给你一个二维数组 `paths`，其中 `paths[i]` 是一个表示文件系统中第 `i` 个文件夹的绝对路径的数组。

- 例如， `["one", "two", "three"]` 表示路径 `"/one/two/three"` 。

如果两个文件夹（不需要在同一层级）包含 非空且相同的 子文件夹 集合 并具有相同的子文件夹结构，则认为这两个文件夹是相同文件夹。相同文件夹的根层级 不 需要相同。如果存在两个（或两个以上）相同 文件夹，则需要将这些文件夹和所有它们的子文件夹 标记 为待删除。

- 例如，下面文件结构中的文件夹 `"/a"` 和 `"/b"` 相同。它们（以及它们的子文件夹）应该被 __全部__ 标记为待删除:

- `/a`
- `/a/x`
- `/a/x/y`
- `/a/z`
- `/b`
- `/b/x`
- `/b/x/y`
- `/b/z`
- 然而，如果文件结构中还包含路径 `"/b/w"` ，那么文件夹 `"/a"` 和 `"/b"` 就不相同。注意，即便添加了新的文件夹 `"/b/w"` ，仍然认为 `"/a/x"` 和 `"/b/x"` 相同。

一旦所有的相同文件夹和它们的子文件夹都被标记为待删除，文件系统将会 删除 所有上述文件夹。文件系统只会执行一次删除操作。执行完这一次删除操作后，不会删除新出现的相同文件夹。

返回二维数组 `ans` ，该数组包含删除所有标记文件夹之后剩余文件夹的路径。路径可以按 __任意顺序__ 返回。

__示例:__

示例 1：

![1948-4](https://assets.leetcode.com/uploads/2021/07/19/lc-dupfolder1.jpg)

```text
输入：paths = [["a"],["c"],["d"],["a","b"],["c","b"],["d","a"]]
输出：[["d"],["d","a"]]
解释：文件结构如上所示。
文件夹 "/a" 和 "/c"（以及它们的子文件夹）都会被标记为待删除，因为它们都包含名为 "b" 的空文件夹。
```

示例 2：

![1948-5](https://assets.leetcode.com/uploads/2021/07/19/lc-dupfolder2.jpg)

```text
输入：paths = [["a"],["c"],["a","b"],["c","b"],["a","b","x"],["a","b","x","y"],["w"],["w","y"]]
输出：[["c"],["c","b"],["a"],["a","b"]]
解释：文件结构如上所示。
文件夹 "/a/b/x" 和 "/w"（以及它们的子文件夹）都会被标记为待删除，因为它们都包含名为 "y" 的空文件夹。
注意，文件夹 "/a" 和 "/c" 在删除后变为相同文件夹，但这两个文件夹不会被删除，因为删除只会进行一次，且它们没有在删除前被标记。
```

示例 3：

![1948-6](https://assets.leetcode.com/uploads/2021/07/19/lc-dupfolder3.jpg)

```text
输入：paths = [["a","b"],["c","d"],["c"],["a"]]
输出：[["c"],["c","d"],["a"],["a","b"]]
解释：文件系统中所有文件夹互不相同。
注意，返回的数组可以按不同顺序返回文件夹路径，因为题目对顺序没有要求。
```

示例 4：

![1948-7](https://assets.leetcode.com/uploads/2021/07/19/lc-dupfolder4_.jpg)

```text
输入：paths = [["a"],["a","x"],["a","x","y"],["a","z"],["b"],["b","x"],["b","x","y"],["b","z"]]
输出：[]
解释：文件结构如上所示。
文件夹 "/a/x" 和 "/b/x"（以及它们的子文件夹）都会被标记为待删除，因为它们都包含名为 "y" 的空文件夹。
文件夹 "/a" 和 "/b"（以及它们的子文件夹）都会被标记为待删除，因为它们都包含一个名为 "z" 的空文件夹以及上面提到的文件夹 "x" 。
```

示例 5：

![1948-8](https://assets.leetcode.com/uploads/2021/07/19/lc-dupfolder5_.jpg)

```text
输入：paths = [["a"],["a","x"],["a","x","y"],["a","z"],["b"],["b","x"],["b","x","y"],["b","z"],["b","w"]]
输出：[["b"],["b","w"],["b","z"],["a"],["a","z"]]
解释：本例与上例的结构基本相同，除了新增 "/b/w" 文件夹。
文件夹 "/a/x" 和 "/b/x" 仍然会被标记，但 "/a" 和 "/b" 不再被标记，因为 "/b" 中有名为 "w" 的空文件夹而 "/a" 没有。
注意，"/a/z" 和 "/b/z" 不会被标记，因为相同子文件夹的集合必须是非空集合，但这两个文件夹都是空的。
```

__提示：__

- `1 <= paths.length <= 2 * 10 ^ 4`
- `1 <= paths[i].length <= 500`
- `1 <= paths[i][j].length <= 10`
- `1 <= sum(paths[i][j].length) <= 2 * 10 ^ 5`
- `path[i][j]` 由小写英文字母组成
- 不会存在两个路径都指向同一个文件夹的情况
- 对于不在根层级的任意文件夹，其父文件夹也会包含在输入中

__思路:__

```text
字典树 ➕ 序列化 ➕ 哈希表
将文件夹路径构建成字典树
序列化成字符串, 用哈希表记录每个字符串, 比如 (a(b(c))) 格式
为了保证结构相同, 需要按照字典序存放
如果哈希表中已经存在了, 则将当前节点和哈希表中的节点都标记为删除
时间复杂度为 O(N), 空间复杂度为 O(N), N 为 sum(paths[i][j].length)
```

__代码:__

__C++__:

```C++
struct TrieNode
{
    map<string, TrieNode*> children;
    bool deleted;
};
class Solution 
{
public:
    vector<vector<string>> deleteDuplicateFolder(vector<vector<string>>& paths) 
    {
        TrieNode* root = new TrieNode();
        for (const auto& path : paths)
        {
            TrieNode* cur = root;
            for (const auto& folder : path)
            {
                if (!cur -> children.count(folder)) cur -> children[folder] = new TrieNode();
                cur = cur -> children[folder];
            }
        }
        unordered_map<string, TrieNode*> m;
        helper(root, m);
        vector<vector<string>> result;
        vector<string> path;
        dfs(root, path, result);
        return result;
    }
private:
    void dfs(TrieNode* root, vector<string>& path, vector<vector<string>>& result)
    {
        for (const auto& [folder, child] : root -> children)
        {
            if (!child -> deleted)
            {
                path.emplace_back(folder);
                dfs(child, path, result);
                path.pop_back();
            }
        }
        if (!path.empty()) result.emplace_back(vector<string>(path));
    }

    string helper(TrieNode* root, unordered_map<string, TrieNode*>& m)
    {
        string cur = "";
        if (root -> children.empty()) return cur;
        for (const auto& [folder, child] : root -> children) cur += "(" + folder + helper(child, m) + ")";
        if (m.count(cur))
        {
            m[cur] -> deleted = true;
            root -> deleted = true;
        }
        else m[cur] = root;
        return cur;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<String>> deleteDuplicateFolder(List<List<String>> paths) {
        TrieNode root = new TrieNode();
        for (List<String> path : paths) {
            TrieNode cur = root;
            for (String folder : path) {
                if (!cur.children.containsKey(folder)) cur.children.put(folder, new TrieNode());
                cur = cur.children.get(folder);
            }
        }
        helper(root, new HashMap<>());
        List<List<String>> result = new ArrayList<>();
        dfs(root, new ArrayList<>(), result);
        return result;
    }

    String helper(TrieNode root, Map<String, TrieNode> map) {
        if (root.children.isEmpty()) return "";
        StringBuilder sb = new StringBuilder();
        for (Map.Entry<String, TrieNode> e : root.children.entrySet()) sb.append('(').append(e.getKey()).append(helper(e.getValue(), map)).append(')');
        String serialized = sb.toString();
        if (map.containsKey(serialized)) {
            map.get(serialized).deleted = true;
            root.deleted = true;
        } else map.put(serialized, root);
        return serialized;
    }

    void dfs(TrieNode root, List<String> path, List<List<String>> result) {
        for (Map.Entry<String, TrieNode> e : root.children.entrySet()) {
            String folder = e.getKey();
            TrieNode child = e.getValue();
            if (!child.deleted) {
                path.add(folder);
                dfs(child, path, result);
                path.remove(path.size() - 1);
            }
        }
        if (!path.isEmpty()) result.add(new ArrayList<>(path));
    }
}

class TrieNode {
    Map<String, TrieNode> children = new TreeMap<>();
    boolean deleted;
}
```

__Python__:

```Python
from sortedcontainers import SortedDict
class Solution:
    def deleteDuplicateFolder(self, paths: List[List[str]]) -> List[List[str]]:
        class TrieNode:
            def __init__(self) -> NoReturn:
                self.children = SortedDict()
                self.deleted = False

        root, result, m = TrieNode(), [], {}

        def dfs(cur: TrieNode, path: List[str]) -> NoReturn:
            for folder, child in cur.children.items():
                if not child.deleted:
                    path.append(folder)
                    dfs(child, path)
                    path.pop()
            path and result.append(path[:])
        
        def helper(cur: TrieNode) -> str:
            node = ""
            if not cur.children:
                return node
            for folder, child in cur.children.items():
                node += '(' + folder + helper(child) + ')'
            if node in m:
                m[node].deleted = cur.deleted = True
            else:
                m[node] = cur
            return node

        for path in paths:
            cur = root
            for folder in path:
                if folder not in cur.children:
                    cur.children[folder] = TrieNode()
                cur = cur.children[folder]
        helper(root)
        dfs(root, [])
        return result
```
