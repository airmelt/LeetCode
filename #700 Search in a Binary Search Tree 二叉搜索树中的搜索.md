__Description__:
Given the root node of a binary search tree (BST) and a value. You need to find the node in the BST that the node's value equals the given value. Return the subtree rooted with that node. If such node doesn't exist, you should return NULL.

__Example:__
For example, 

Given the tree:
```
        4
       / \
      2   7
     / \
    1   3
```
And the value to search: 2
You should return this subtree:
```
      2     
     / \   
    1   3
```
In the example above, if we want to search the value 5, since there is no node with value 5, we should return NULL.

Note that an empty tree is represented by NULL, therefore you would see the expected output (serialized tree format) as [], not null.

__题目描述__:
给定二叉搜索树（BST）的根节点和一个值。 你需要在BST中找到节点值等于给定值的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 NULL。

__示例 :__
例如，

给定二叉搜索树:
```
        4
       / \
      2   7
     / \
    1   3
```
和值: 2
你应该返回如下子树:
```
      2     
     / \   
    1   3
```
在上述示例中，如果要找的值是 5，但因为没有节点值为 5，我们应该返回 NULL。

__思路__:
根据 BST的定义向两边查找即可
1. 递归
2. 迭代
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        while (root) {
            if (root -> val == val) return root;
            else if (root -> val > val) root = root -> left;
            else if (root -> val < val) root = root -> right;
        }
        return NULL;
    }
};
```

__Java__:
```
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
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null) return null;
        if (root.val > val) return searchBST(root.left, val);
        if (root.val < val) return searchBST(root.right, val);
        return root;
    }
}
```

__Python__:
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def searchBST(self, root: TreeNode, val: int) -> TreeNode:
        while root:
            if root.val == val:
                return root
            elif root.val > val:
                root = root.left
            else:
                root = root.right
```