# 2096 Step-By-Step Directions From a Binary Tree Node to Another 从二叉树一个节点到另一个节点每一步的方向

__Description:__

You are given the `root` of a __binary tree__ with `n` nodes. Each node is uniquely assigned a value from `1` to `n`. You are also given an integer `startValue` representing the value of the start node `s`, and a different integer `destValue` representing the value of the destination node `t`.

Find the __shortest path__ starting from node `s` and ending at node `t`. Generate step-by-step directions of such path as a string consisting of only the __uppercase__ letters `'L'`, `'R'`, and `'U'`. Each letter indicates a specific direction:

- `'L'` means to go from a node to its __left child__ node.
- `'R'` means to go from a node to its __right child__ node.
- `'U'` means to go from a node to its __parent__ node.

Return _the step-by-step directions of the __shortest path__ from node_ `s` _to node_ `t`.

__Example:__

Example 1:

![2096-1](https://assets.leetcode.com/uploads/2021/11/15/eg1.png)

```text
Input: root = [5,1,2,3,null,6,4], startValue = 3, destValue = 6
Output: "UURL"
Explanation: The shortest path is: 3 → 1 → 5 → 2 → 6.
```

Example 2:

![2096-2](https://assets.leetcode.com/uploads/2021/11/15/eg2.png)

```text
Input: root = [2,1], startValue = 2, destValue = 1
Output: "L"
Explanation: The shortest path is: 2 → 1.
```

__Constraints:__

- The number of nodes in the tree is `n`.
- `2 <= n <= 10 ^ 5`
- `1 <= Node.val <= n`
- All the values in the tree are __unique__.
- `1 <= startValue, destValue <= n`
- `startValue != destValue`

__题目描述:__

给你一棵 __二叉树__ 的根节点 `root` ，这棵二叉树总共有 `n` 个节点。每个节点的值为 `1` 到 `n` 中的一个整数，且互不相同。给你一个整数 `startValue` ，表示起点节点 `s` 的值，和另一个不同的整数 `destValue` ，表示终点节点 `t` 的值。

请找到从节点 `s` 到节点 `t` 的 __最短路径__ ，并以字符串的形式返回每一步的方向。每一步用 __大写__ 字母 `'L'` ， `'R'` 和 `'U'` 分别表示一种方向:

- `'L'` 表示从一个节点前往它的 __左孩子__ 节点。
- `'R'` 表示从一个节点前往它的 __右孩子__ 节点。
- `'U'` 表示从一个节点前往它的 __父__ 节点。

请你返回从 `s` 到 `t` __最短路径__ 每一步的方向。

__示例:__

示例 1：

![2096-3](https://assets.leetcode.com/uploads/2021/11/15/eg1.png)

```text
输入：root = [5,1,2,3,null,6,4], startValue = 3, destValue = 6
输出："UURL"
解释：最短路径为：3 → 1 → 5 → 2 → 6 。
```

示例 2：

![2096-4](https://assets.leetcode.com/uploads/2021/11/15/eg2.png)

```text
输入：root = [2,1], startValue = 2, destValue = 1
输出："L"
解释：最短路径为：2 → 1 。
```

__提示：__

- 树中节点数目为 `n` 。
- `2 <= n <= 10 ^ 5`
- `1 <= Node.val <= n`
- 树中所有节点的值 __互不相同__ 。
- `1 <= startValue, destValue <= n`
- `startValue != destValue`

__思路:__

```text
模拟
最短路径为开始节点到最近父节点再到目标节点
先找到从根节点到两个节点的路径
利用路径找到两个节点的最近父节点, 记录下标为 index
两条路径长度分别为 m 和 n
则从开始节点出发需要回到父节点, 需要 m - index 个 'U'
然后再加上目标节点的路径即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution 
{
public:
    string getDirections(TreeNode* root, int startValue, int destValue) {
        string path2 = "";
        dfs(root, startValue, path2);
        string path1(path);
        path = "";
        path2 = "";
        dfs(root, destValue, path2);
        int index = 0, m = path1.size(), n = path.size();
        while (index < m and index < n and path1[index] == path[index]) ++index;
        string result(m - index, 'U');
        return result + path.substr(index);
    }
private:
    string path;

    bool dfs(TreeNode* root, int target, string& cur) 
    {
        if (root -> val == target) 
        {
            path = cur;
            return true;
        }
        if (root -> left) 
        {
            cur += "L";
            if (dfs(root -> left, target, cur)) return true;
            cur.erase(cur.end() - 1);
        }
        if (root -> right) 
        {
            cur += "R";
            if (dfs(root -> right, target, cur)) return true;
            cur.erase(cur.end() - 1);
        }
        return false; 
    }
};
```

__Java__:

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    
    private String path = "";
    
    public String getDirections(TreeNode root, int startValue, int destValue) {
        dfs(root, startValue, new StringBuilder());
        char[] c1 = path.toCharArray();
        path = "";
        dfs(root, destValue, new StringBuilder());
        char[] c2 = path.toCharArray();
        int index = 0, m = c1.length, n = c2.length;
        while (index < m && index < n && c1[index] == c2[index]) ++index;
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < m - index; i++) sb.append("U");
        sb.append(path.substring(index));
        return sb.toString();
    }
    
    private boolean dfs(TreeNode root, int target, StringBuilder sb) {
        if (root.val == target) {
            path = sb.toString();
            return true;
        }
        if (root.left != null) {
            sb.append("L");
            if (dfs(root.left, target, sb)) return true;
            sb.deleteCharAt(sb.length() - 1);
        }
        if (root.right != null) {
            sb.append("R");
            if (dfs(root.right, target, sb)) return true;
            sb.deleteCharAt(sb.length()-1);
        }
        return false; 
    }
}
```

__Python__:

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getDirections(self, root: Optional[TreeNode], startValue: int, destValue: int) -> str:
        paths = []

        def dfs(cur: Optional[TreeNode], target: int, path: list[str]) -> bool:
            if cur.val == target:
                nonlocal paths
                paths.append(path)
                return True
            if cur.left:
                path.append("L")
                if dfs(cur.left, target, path):
                    return True
                path.pop()
            if cur.right:
                path.append("R")
                if dfs(cur.right, target, path):
                    return True
                path.pop()
            return False
        
        dfs(root, startValue, [])
        dfs(root, destValue, [])
        idx, m, n = 0, len(c1 := paths[0]), len(c2 := paths[1])
        while idx < m and idx < n and c1[idx] == c2[idx]:
            idx += 1
        return 'U' * (m - idx) + ''.join(c2[idx:])
```
