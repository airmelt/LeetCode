# 687 Longest Univalue Path 最长同值路径

__Description__:
Given a binary tree, find the length of the longest path where each node in the path has the same value. This path may or may not pass through the root.

The length of path between two nodes is represented by the number of edges between them.

__Example:__

Example 1:

Input:

```text
              5
             / \
            4   5
           / \   \
          1   1   5
```

Output: 2

Example 2:

Input:

```text
              1
             / \
            4   5
           / \   \
          4   4   5
```

Output: 2

__Note:__
The given binary tree has not more than 10000 nodes. The height of the tree is not more than 1000.

__题目描述__:
给定一个二叉树，找到最长的路径，这个路径中的每个节点具有相同值。 这条路径可以经过也可以不经过根节点。

注意：两个节点之间的路径长度由它们之间的边数表示。

__示例 :__

示例 1:

输入:

```text
              5
             / \
            4   5
           / \   \
          1   1   5
```

输出:

2
示例 2:

输入:

```text
              1
             / \
            4   5
           / \   \
          4   4   5
```

输出:

2

__注意:__
给定的二叉树不超过10000个结点。 树的高度不超过1000

__思路__:

采用递归的思路, 对每一个结点判断, 只要不和当前结点的值相等或者为空就返回长度 0, 否则返回左子树和右子树的长度之和 + 1, 返回所有节点的最长的长度即可
时间复杂度O(n ^ 2), 空间复杂度O(n)

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
    int longestUnivaluePath(TreeNode* root) 
    {
        return !root ? 0 : max(get_length(root -> left, root -> val) + get_length(root -> right, root -> val), max(longestUnivaluePath(root -> left), longestUnivaluePath(root -> right)));
    }
private:
    int get_length(TreeNode* root, int val) 
    {
        return (!root or root -> val != val) ? 0 : 1 + max(get_length(root -> left, val), get_length(root -> right, val));
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
    public int longestUnivaluePath(TreeNode root) {
        return root == null ? 0 : Math.max(getLength(root.left, root.val) + getLength(root.right, root.val), Math.max(longestUnivaluePath(root.left), longestUnivaluePath(root.right)));
    }
    
    private int getLength(TreeNode root, int val) {
        return (root == null || root.val != val) ? 0 : 1 + Math.max(getLength(root.left, val), getLength(root.right, val));
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
    def longestUnivaluePath(self, root: TreeNode) -> int:
        def get_length(root: TreeNode, val: int) -> int:
            return 0 if not root or root.val != val else max(get_length(root.left, val), get_length(root.right, val)) + 1 
        return max(get_length(root.left, root.val) + get_length(root.right, root.val), max(self.longestUnivaluePath(root.left), self.longestUnivaluePath(root.right))) if root else 0
```
