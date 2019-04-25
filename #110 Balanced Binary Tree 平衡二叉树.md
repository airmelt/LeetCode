__Description__:
Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

__Example__:
Example 1:

Given the following tree [3,9,20,null,null,15,7]:
```
    3
   / \
  9  20
    /  \
   15   7
```
Return true.

Example 2:

Given the following tree [1,2,2,3,3,null,null,4,4]:
```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```
Return false.

__题目描述__:
给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。

 __示例__：
示例 1:

给定二叉树 [3,9,20,null,null,15,7]
```
    3
   / \
  9  20
    /  \
   15   7
```
返回 true 。

示例 2:

给定二叉树 [1,2,2,3,3,null,null,4,4]
```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```
返回 false 。

__思路__:
递归, 本身平衡二叉树就是基于递归定义的, 可以用剪枝优化递归过程
当子树已经是不平衡的时候中断其子树的递归
树的深度(高度)参考[LeetCode #104 Maximum Depth of Binary Tree 二叉树的最大深度](https://www.jianshu.com/p/8db39ff5b800)
时间复杂度O(n), 空间复杂度O(n), n为树中结点数

[平衡二叉搜索树 Self-balancing binary search tree-wiki](https://en.wikipedia.org/wiki/Self-balancing_binary_search_tree)
> **平衡二叉搜索树**（英语：Balanced Binary Tree）是一种结构平衡的[二叉搜索树](https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91 "二叉搜索树")，即叶节点高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。它能在O(*logn*)内完成插入、查找和删除操作，最早被发明的平衡二叉搜索树为[AVL树](https://zh.wikipedia.org/wiki/AVL%E6%A0%91 "AVL树")。
> 常见的平衡二叉搜索树有：
> * [AVL树](https://zh.wikipedia.org/wiki/AVL%E6%A0%91 "AVL树")
> *   [红黑树](https://zh.wikipedia.org/wiki/%E7%B4%85%E9%BB%91%E6%A8%B9)
> *   [Treap](https://zh.wikipedia.org/wiki/Treap)
> *   节点大小平衡树

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
    bool isBalanced(TreeNode* root) {
        return depth(root) != -1;
    }
private:
    int depth(TreeNode* root) {
        if (!root) return 0;
        int ld = depth(root -> left);
        int rd = depth(root -> right);
        if (ld >= 0 && rd >= 0 && abs(ld - rd) < 2) return ld > rd ? ld + 1 : rd + 1;
        return -1;
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
    public boolean isBalanced(TreeNode root) {
        return depth(root) != -1;
    }

    private int depth(TreeNode root) {
        if (root == null) return 0;
        int ld = depth(root.left);
        int rd = depth(root.right);
        if (ld >= 0 && rd >= 0 && Math.abs(ld - rd) < 2) return ld > rd ? ld + 1 : rd + 1;
        return -1;
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
    def isBalanced(self, root: TreeNode) -> bool:
        if not root:
            return True
        if abs(self.depth(root.left) - self.depth(root.right)) > 1:
            return False
        return self.isBalanced(root.left) and self.isBalanced(root.right)

    def depth(self, root: TreeNode) -> int:
        if not root:
            return 0
        return max(self.depth(root.left), self.depth(root.right)) + 1
```
