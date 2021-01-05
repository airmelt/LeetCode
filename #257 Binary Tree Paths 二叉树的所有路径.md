# 257 Binary Tree Paths 二叉树的所有路径

__Description__:
Given a binary tree, return all root-to-leaf paths.

Note: A leaf is a node with no children.

Example:

Input:

```text
   1
 /   \
2     3
 \
  5
```

Output: ["1->2->5", "1->3"]

Explanation: All root-to-leaf paths are: 1->2->5, 1->3

__题目描述__:
给定一个二叉树，返回所有从根节点到叶子节点的路径。

说明: 叶子节点是指没有子节点的节点。

示例:

输入:

```text
   1
 /   \
2     3
 \
  5
```

输出: ["1->2->5", "1->3"]

解释: 所有根节点到叶子节点的路径为: 1->2->5, 1->3

__思路__:

采用深度优先遍历(dfs)
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution 
{
public:
    vector<string> binaryTreePaths(TreeNode* root) 
    {
        vector<string> result;
        if (!root) return result;
        vector<string> left = binaryTreePaths(root -> left);
        vector<string> right = binaryTreePaths(root -> right);
        string temp = to_string(root -> val);
        if (!left.empty()) for (int i = 0; i < left.size(); i++) result.push_back(temp + "->" + left[i]);
        if (!right.empty()) for (int i = 0; i < right.size(); i++) result.push_back(temp + "->" + right[i]);
        if (left.empty() && right.empty()) result.push_back(temp);
        return result;
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
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        String temp = "";
        dfs(root, temp, result);
        return result;
    }

    private void dfs(TreeNode root, String temp, List<String> list) {
        if (root == null) return;
        temp += root.val;
        if (root.left == null && root.right == null) {
            list.add(temp);
            return;
        } else {
            dfs(root.left, temp + "->", list);
            dfs(root.right, temp + "->", list);
        }
    }
}
```

__Python__:

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def binaryTreePaths(self, root: TreeNode) -> List[str]:
        result = []
        def dfs(root: TreeNode, path: List[str]) -> None:
            if not root:
                return
            if not root.left and not root.right:
                path.append(str(root.val))
                result.append('->'.join(path))
            dfs(root.left, path + [str(root.val)])
            dfs(root.right, path + [str(root.val)])
        dfs(root, [])
        return result
```
