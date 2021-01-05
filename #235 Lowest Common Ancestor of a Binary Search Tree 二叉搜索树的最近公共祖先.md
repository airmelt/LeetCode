# 235 Lowest Common Ancestor of a Binary Search Tree 二叉搜索树的最近公共祖先

__Description__:
Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow **a node to be a descendant of itself**).”

Given binary search tree:  root = [6,2,8,0,4,7,9,null,null,3,5]

![Binary Search Tree](http://upload-images.jianshu.io/upload_images/16639143-c196720af1f6ce69.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**Example:**

**Example 1:**
**Input:** root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
**Output:** 6
**Explanation:** The LCA of nodes `2` and `8` is `6`.

**Example 2:**
**Input:** root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
**Output:** 2
**Explanation:** The LCA of nodes `2` and `4` is `2`, since a node can be a descendant of itself according to the LCA definition.

**Note:**

* All of the nodes' values will be unique.
* p and q are different and both values will exist in the BST.

__题目描述__:
给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]

![二叉搜索树](http://upload-images.jianshu.io/upload_images/16639143-1c89c3214d035cd5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**示例 1:**
**输入:** root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
**输出:** 6
**解释:** 节点 `2` 和节点 `8` 的最近公共祖先是 `6。`

**示例 2:**
**输入:** root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
**输出:** 2
**解释:** 节点 `2` 和节点 `4` 的最近公共祖先是 `2`, 因为根据定义最近公共祖先节点可以为节点本身。

**说明:**

* 所有节点的值都是唯一的。
* p、q 为不同节点且均存在于给定的二叉搜索树中。

__思路__:

利用二叉搜索树的性质, 找到第一个分叉点即为所求结果
时间复杂度O(lgn), 空间复杂度O(1), 每次查找范围减半

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
        while (true) 
        {
            if (root -> val > p -> val and root -> val > q -> val) root = root -> left;
            else if (root -> val < p -> val and root -> val < q -> val) root = root -> right;
            else return root;
        }
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
        if (p.val > q.val) {
            TreeNode temp = p;
            p = q;
            q = temp;
        }
        if (root.val < p.val) return lowestCommonAncestor(root.right, p, q);
        if (root.val > q.val) return lowestCommonAncestor(root.left, p, q);
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
        while True:
            if root.val > p.val and root.val > q.val:
                root = root.left
            elif root.val < p.val and root.val < q.val:
                root = root.right
            else:
                return root
```
