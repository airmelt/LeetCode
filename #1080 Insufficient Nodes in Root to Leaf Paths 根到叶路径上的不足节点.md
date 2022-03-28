# 1080 Insufficient Nodes in Root to Leaf Paths 根到叶路径上的不足节点

__Description__:
Given the root of a binary tree and an integer limit, delete all insufficient nodes in the tree simultaneously, and return the root of the resulting binary tree.

A node is insufficient if every root to leaf path intersecting this node has a sum strictly less than limit.

A leaf is a node with no children.

__Example:__

Example 1:

![binary tree 1](https://assets.leetcode.com/uploads/2019/06/05/insufficient-11.png)

Input: root = [1,2,3,4,-99,-99,7,8,9,-99,-99,12,13,-99,14], limit = 1
Output: [1,2,3,4,null,null,7,8,9,null,14]

Example 2:

![binary tree 1](https://assets.leetcode.com/uploads/2019/06/05/insufficient-3.png)

Input: root = [5,4,8,11,null,17,4,7,1,null,null,5,3], limit = 22
Output: [5,4,8,11,null,17,4,7,null,null,null,5]

Example 3:

![binary tree 1](https://assets.leetcode.com/uploads/2019/06/11/screen-shot-2019-06-11-at-83301-pm.png)

Input: root = [1,2,-3,-5,null,4,null], limit = -1
Output: [1,null,-3,4]

__Constraints:__

The number of nodes in the tree is in the range [1, 5000].
-10^5 <= Node.val <= 10^5
-10^9 <= limit <= 10^9

__题目描述__:
给定一棵二叉树的根 root，请你考虑它所有 从根到叶的路径：从根到任何叶的路径。（所谓一个叶子节点，就是一个没有子节点的节点）

假如通过节点 node 的每种可能的 “根-叶” 路径上值的总和全都小于给定的 limit，则该节点被称之为「不足节点」，需要被删除。

请你删除所有不足节点，并返回生成的二叉树的根。

__示例 :__

示例 1：

![二叉树 1-1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/06/08/insufficient-1.png)

输入：root = [1,2,3,4,-99,-99,7,8,9,-99,-99,12,13,-99,14], limit = 1

![二叉树 1-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/06/08/insufficient-2.png)

输出：[1,2,3,4,null,null,7,8,9,null,14]

示例 2：

![二叉树 2-1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/06/08/insufficient-3.png)

输入：root = [5,4,8,11,null,17,4,7,1,null,null,5,3], limit = 22

![二叉树 2-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/06/08/insufficient-4.png)

输出：[5,4,8,11,null,17,4,7,null,null,null,5]

示例 3：

![二叉树 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/06/08/insufficient-5.png)

输入：root = [5,-6,-6], limit = 0
输出：[]

__提示:__

给定的树有 1 到 5000 个节点
-10^5 <= node.val <= 10^5
-10^9 <= limit <= 10^9

__思路__:

递归
如果为叶子结点, 比较 limit 大小直接返回
计算左右结点, 如果左右结点都被丢弃, 父结点也丢弃
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
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution 
{
public:
    TreeNode* sufficientSubset(TreeNode* root, int limit) 
    {
        if (!root) return root;
        if (!root -> left and !root -> right) return root -> val < limit ? nullptr : root;
        root -> left = sufficientSubset(root -> left, limit - root -> val);
        root -> right = sufficientSubset(root -> right, limit - root -> val);
        return !root -> left and !root -> right ? nullptr : root;
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
    public TreeNode sufficientSubset(TreeNode root, int limit) {
        if (root == null) return null;
        if (root.left == null && root.right == null) return root.val < limit ? null : root;
        root.left = sufficientSubset(root.left, limit - root.val);
        root.right = sufficientSubset(root.right, limit - root.val);
        return root.left == null && root.right == null ? null : root;
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
    def sufficientSubset(self, root: TreeNode, limit: int) -> TreeNode:
        if not root:
            return root
        if not root.left and not root.right:
            return None if root.val < limit else root
        root.left, root.right = self.sufficientSubset(root.left, limit - root.val), self.sufficientSubset(root.right, limit - root.val)
        return None if not root.left and not root.right else root
```
