# 669 Trim a Binary Search Tree 修剪二叉搜索树

__Description__:
Given a binary search tree and the lowest and highest boundaries as L and R, trim the tree so that all its elements lies in [L, R] (R >= L). You might need to change the root of the tree, so the result should return the new root of the trimmed binary search tree.

__Example:__

Example 1:

Input:

```text
    1
   / \
  0   2
```

  L = 1
  R = 2

Output:

```text
    1
      \
       2
```

Example 2:

Input:

```text
    3
   / \
  0   4
   \
    2
   /
  1
```

  L = 1
  R = 3

Output:

```text
      3
     / 
   2   
  /
 1
```

__题目描述__:
给定一个二叉搜索树，同时给定最小边界L 和最大边界 R。通过修剪二叉搜索树，使得所有节点的值在[L, R]中 (R>=L) 。你可能需要改变树的根节点，所以结果应当返回修剪好的二叉搜索树的新的根节点。

__示例 :__

示例 1:

输入:

```text
    1
   / \
  0   2
```

  L = 1
  R = 2

输出:

```text
    1
      \
       2
```

示例 2:

输入:

```text
    3
   / \
  0   4
   \
    2
   /
  1
```

  L = 1
  R = 3

输出:

```text
      3
     / 
   2   
  /
 1
```

__思路__:

注意给的树是 BST, 要利用其性质

1. 迭代, 先搜索根节点的值, 满足 root -> val在 (L, R)之间, 然后分别修改左子树和右子树即可
2. 递归, 思路与迭代类似
时间复杂度O(n), 空间复杂度O(1)

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
    TreeNode* trimBST(TreeNode* root, int L, int R) 
    {
        if (root) 
        {
            while (root -> val < L or root -> val > R) 
            {
                if (root -> val < L) root = root -> right;
                else root = root -> left;
            }
            TreeNode* cur = root;
            // 修剪左子树
            while (cur) 
            {
                while (cur -> left and cur -> left -> val < L) cur -> left = cur -> left -> right;
                cur = cur -> left;
            }
            cur = root;
            // 修剪右子树
            while (cur) 
            {
                while (cur -> right and cur -> right -> val > R) cur -> right = cur -> right -> left;
                cur = cur -> right;
            }
        }
        return root;
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
    public TreeNode trimBST(TreeNode root, int L, int R) {
        if (root == null) return root;
        if (root.val < L) return trimBST(root.right, L, R);
        if (root.val > R) return trimBST(root.left, L, R);
        root.left = trimBST(root.left, L, R);
        root.right = trimBST(root.right, L, R);
        return root;
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
    def trimBST(self, root: TreeNode, L: int, R: int) -> TreeNode:
        if not root:
            return root
        if root.val < L:
            return self.trimBST(root.right, L, R)
        if root.val > R:
            return self.trimBST(root.left, L, R)
        root.left, root.right = self.trimBST(root.left, L, R), self.trimBST(root.right, L, R)
        return root
```
