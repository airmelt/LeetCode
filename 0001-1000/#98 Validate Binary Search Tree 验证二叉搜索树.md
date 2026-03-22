# 98 Validate Binary Search Tree 验证二叉搜索树

__Description__:
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.

__Example:__

Example 1:

```text
    2
   / \
  1   3
```

Input: [2,1,3]
Output: true
Example 2:

```text
    5
   / \
  1   4
     / \
    3   6
```

Input: [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.

__题目描述__:
给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。

__示例 :__

示例 1:

输入:

```text
    2
   / \
  1   3
```

输出: true

示例 2:

输入:

```text
    5
   / \
  1   4
     / \
    3   6
```

输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。

__思路__:

注意这里的 BST树是对应全局递归定义的, 不能只比较当前节点的左右孩子的大小

1. 递归法
给定一个范围, 左右子树都必须在该范围中
2. 迭代法
相当于进行中序遍历, 要求中序遍历的结果是递增的

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
    bool isValidBST(TreeNode* root) 
    {
        stack<TreeNode*> s;
        long last = (long)INT_MIN - 1;
        while (s.size() or root) 
        {
            while (root) 
            {
                s.push(root);
                root = root -> left;
            }
            root = s.top();
            s.pop();
            if (root -> val <= last) return false;
            last = root -> val;
            root = root -> right;
        }
        return true;
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
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, null, null);
    }
    
    private boolean isValidBST(TreeNode root, Integer lower, Integer upper) {
        return (root == null) || ((lower == null || root.val > lower) && (upper == null || root.val < upper) && isValidBST(root.right, root.val, upper) && isValidBST(root.left, lower, root.val));
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
    def isValidBST(self, root: TreeNode) -> bool:
        def helper(root: TreeNode, lower: int, upper: int) -> bool:
            if not root:
                return True
            return (lower < root.val < upper) and helper(root.left, lower, root.val) and helper(root.right, root.val, upper)
        return helper(root, -(1 << 33), 1 << 33)
```
