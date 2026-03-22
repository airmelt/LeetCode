# 814 Binary Tree Pruning 二叉树剪枝

__Description__:
Given the root of a binary tree, return the same tree where every subtree (of the given tree) not containing a 1 has been removed.

A subtree of a node node is node plus every node that is a descendant of node.

__Example:__

Example 1:

![Bi Tree 1](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/06/1028_2.png)

Input: root = [1,null,0,0,1]
Output: [1,null,0,null,1]
Explanation:
Only the red nodes satisfy the property "every subtree not containing a 1".
The diagram on the right represents the answer.

Example 2:

![Bi Tree 2](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/06/1028_1.png)

Input: root = [1,0,1,0,0,0,1]
Output: [1,null,1,null,1]

Example 3:

![Bi Tree 3](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/05/1028.png)

Input: root = [1,1,0,1,1,0,1,0]
Output: [1,1,0,1,1,null,1]

__Constraints:__

The number of nodes in the tree is in the range [1, 200].
Node.val is either 0 or 1.

__题目描述__:
给定二叉树根结点 root ，此外树的每个结点的值要么是 0，要么是 1。

返回移除了所有不包含 1 的子树的原二叉树。

( 节点 X 的子树为 X 本身，以及所有 X 的后代。)

__示例 :__

示例1:

![二叉树 1](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/06/1028_2.png)

输入: [1,null,0,0,1]
输出: [1,null,0,null,1]

解释:
只有红色节点满足条件“所有不包含 1 的子树”。
右图为返回的答案。

示例2:

![二叉树 2](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/06/1028_1.png)

输入: [1,0,1,0,0,0,1]
输出: [1,null,1,null,1]

示例3:

![二叉树 3](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/05/1028.png)

输入: [1,1,0,1,1,0,1,0]
输出: [1,1,0,1,1,null,1]

__说明:__

给定的二叉树最多有 200 个节点。
每个节点的值只会为 0 或 1 。

__思路__:

后序遍历
要删除 root 首先要删除 root -> left, root -> right
先判断 root 是否为空指针, 是的话直接返回自身
然后判断 root 是否为叶结点且 root -> val == 0, 直接进行删除操作即可
时间复杂度为 O(n), 空间复杂度为 O(h), 其中 h 为树的高度

__代码__:
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
    TreeNode* pruneTree(TreeNode* root) 
    {
        if (!root) return root;
        root -> left = pruneTree(root -> left);
        root -> right = pruneTree(root -> right);
        return !root -> left and !root -> right and !root -> val ? nullptr : root;
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
    public TreeNode pruneTree(TreeNode root) {
        if (root == null) return root;
        root.left = pruneTree(root.left);
        root.right = pruneTree(root.right);
        return root.left == null && root.right == null && root.val == 0 ? null : root;
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
    def pruneTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return root
        root.left, root.right = self.pruneTree(root.left), self.pruneTree(root.right)
        return None if not root.left and not root.right and not root.val else root
```
