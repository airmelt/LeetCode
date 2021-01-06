# 236 Lowest Common Ancestor of a Binary Tree 二叉树的最近公共祖先

__Description__:
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

Given the following binary tree:  root = [3,5,1,6,2,0,8,null,null,7,4]

![Binary Tree](https://upload-images.jianshu.io/upload_images/16639143-ae43f8ddab3b8b3d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

__Example:__

Example 1:

Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.

Example 2:

Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.

__Note:__

All of the nodes' values will be unique.
p and q are different and both values will exist in the binary tree.

__题目描述__:
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近[公共祖先](https://baike.baidu.com/item/%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88/8918834?fr=aladdin)的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

![二叉树](https://upload-images.jianshu.io/upload_images/16639143-ae43f8ddab3b8b3d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

__示例 :__

示例 1:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。

示例 2:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。

__说明：__

所有节点的值都是唯一的。
p、q 为不同节点且均存在于给定的二叉树中。

__思路__:

递归

1. 若 root为空, 或者 root == p or root == q, 直接返回 root, 这时, 要么找不到公共祖先, 要么 p或者 q就是公共祖先
2. 进入递归, 从 root的左子树和右子树分别搜索公共祖先
3. 如果左子树为空, 说明公共祖先一定在右子树, 直接返回右子树
4. 如果右子树为空, 说明公共祖先一定在左子树, 直接返回左子树
5. 如果两个子树都非空, 说明根结点就是公共祖先
6. 如果两个子树都空, 这种情况就是 1中的 root为空
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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) 
    {
        if (!root or root == p or root == q) return root;
        TreeNode *left = lowestCommonAncestor(root -> left, p, q), *right = lowestCommonAncestor(root -> right, p, q);
        if (!left) return right;
        if (!right) return left;
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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q), right = lowestCommonAncestor(root.right, p, q);
        if (left == null) return right;
        if (right == null) return left;
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
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if root in (None, p, q): 
            return root
        l, r =  self.lowestCommonAncestor(root.left, p, q), self.lowestCommonAncestor(root.right, p, q)
        return r if l is None else l if r is None else root
```
