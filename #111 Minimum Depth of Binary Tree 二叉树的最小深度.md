# 111 Minimum Depth of Binary Tree 二叉树的最小深度

__Description__:
Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Note: A leaf is a node with no children.

__Example__:

Given binary tree [3,9,20,null,null,15,7],

```text
    3
   / \
  9  20
    /  \
   15   7
```

return its minimum depth = 2.

__题目描述__:
给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

说明: 叶子节点是指没有子节点的节点。

__示例__：

给定二叉树 [3,9,20,null,null,15,7],

```text
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最小深度  2.

__思路__:

参考[LeetCode #104 Maximum Depth of Binary Tree 二叉树的最大深度](https://www.jianshu.com/p/8db39ff5b800)
跟最大深度略有不同, 找到叶结点即可返回叶结点所在层数, 注意根结点不是叶结点, 所以[1, 2]应当返回 2, 而不是1.

1. 递归
2. 迭代
时间复杂度O(n), 空间复杂度O(n), n为树中结点数

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
    int minDepth(TreeNode* root) 
    {
        if (!root) return 0;
        queue<TreeNode*> q;
        q.push(root);
        int result = 0;
        while (!q.empty()) 
        {
            int nums = q.size();
            result++;
            while (nums) 
            {
                TreeNode* cur = q.front();
                q.pop();
                if (!cur -> left and !cur -> right) return result;
                if (cur -> left) q.push(cur -> left);
                if (cur -> right) q.push(cur -> right);
                nums--;
            }
        }
        return result;
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
    public int minDepth(TreeNode root) {
        if (root == null) return 0;
        int left = minDepth(root.left);
        int right = minDepth(root.right);
        if (root.left == null || root.right == null) return 1 + left + right;
        return Math.min(left, right) + 1;
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
    def minDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        left = self.minDepth(root.left)
        right = self.minDepth(root.right)
        if not left or not right:
            return 1 + left + right
        return min(left, right) + 1
```
