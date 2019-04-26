__Description__:
Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

Note: A leaf is a node with no children.

__Example__:

Given the below binary tree and sum = 22,
```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
```
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.

__题目描述__:
给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

说明: 叶子节点是指没有子节点的节点。

 __示例__：
给定如下二叉树，以及目标和 sum = 22，
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```
返回 true, 因为存在目标和为 22 的根节点到叶子节点的路径 5->4->11->2。

__思路__:
参考[LeetCode #104 Maximum Depth of Binary Tree 二叉树的最大深度](https://www.jianshu.com/p/8db39ff5b800)
注意下递归结束的条件
1. 递归
2. 迭代
时间复杂度O(n), 空间复杂度O(n), n为树中结点数

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
    bool hasPathSum(TreeNode* root, int sum) {
        if (!root) return false;
        if (!root -> left && !root -> right) return root -> val == sum;
        return hasPathSum(root -> left, sum - root -> val) || hasPathSum(root -> right, sum - root -> val);
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
    public boolean hasPathSum(TreeNode root, int sum) {
        if (root == null) return false;
        if (root.left == null && root.right == null) return root.val == sum;
        return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
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
    def hasPathSum(self, root: TreeNode, sum: int) -> bool:
        if not root:
            return False
        stack = [(root, sum)]
        while stack:
            cur, temp = stack.pop()
            if not cur.left and not cur.right and cur.val == temp:
                return True
            if cur.left:
                stack.append([cur.left, temp - cur.val])
            if cur.right:
                stack.append([cur.right, temp - cur.val])
        return False
```
