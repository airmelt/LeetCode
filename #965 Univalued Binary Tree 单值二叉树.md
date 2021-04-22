# 965 Univalued Binary Tree 单值二叉树

__Description__:
A binary tree is univalued if every node in the tree has the same value.

Return true if and only if the given tree is univalued.

__Example:__

Example 1:

![Binary Tree 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/29/screen-shot-2018-12-25-at-50104-pm.png)

Input: [1,1,1,1,1,null,1]
Output: true

Example 2:

![Binary Tree 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/29/screen-shot-2018-12-25-at-50050-pm.png)

Input: [2,2,2,5,2]
Output: false

__Note:__

The number of nodes in the given tree will be in the range [1, 100].
Each node's value will be an integer in the range [0, 99].

__题目描述__:
如果二叉树每个节点都具有相同的值，那么该二叉树就是单值二叉树。

只有给定的树是单值二叉树时，才返回 true；否则返回 false。

__示例 :__

示例 1：

![二叉树1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/29/screen-shot-2018-12-25-at-50104-pm.png)

输入：[1,1,1,1,1,null,1]
输出：true

示例 2：

![二叉树2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/29/screen-shot-2018-12-25-at-50050-pm.png)

输入：[2,2,2,5,2]
输出：false

__提示：__

给定树的节点数范围是 [1, 100]。
每个节点的值都是整数，范围为 [0, 99] 。

__思路__:

1. 迭代, 用任何一种遍历遍历二叉树即可
2. 递归
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
    bool isUnivalTree(TreeNode* root) 
    {
        int result = root -> val;
        queue<TreeNode*> q;
        q.push(root);
        while (q.size())
        {
            TreeNode* cur = q.front();
            q.pop();
            if (cur -> val != result) return false;
            if (cur -> left) q.push(cur -> left);
            if (cur -> right) q.push(cur -> right);
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
    public boolean isUnivalTree(TreeNode root) {
        return root == null ? true : (root.left == null ? true : root.val == root.left.val) && (root.right == null ? true : root.val == root.right.val) && isUnivalTree(root.left) && isUnivalTree(root.right);
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
    def isUnivalTree(self, root: TreeNode) -> bool:
        return True if not root else ((True if not root.left else root.val == root.left.val) and (True if not root.right else root.val == root.right.val) and (self.isUnivalTree(root.left) and self.isUnivalTree(root.right)))
```
