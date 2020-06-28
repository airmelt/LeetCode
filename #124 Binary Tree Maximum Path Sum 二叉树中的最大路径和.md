__Description__:
Given a non-empty binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain at least one node and does not need to go through the root.

__Example:__
Example 1:

Input: [1,2,3]
```
       1
      / \
     2   3
```
Output: 6

Example 2:

Input: [-10,9,20,null,null,15,7]
```
   -10
   / \
  9  20
    /  \
   15   7
```
Output: 42

__题目描述__:
给定一个非空二叉树，返回其最大路径和。

本题中，路径被定义为一条从树中任意节点出发，达到任意节点的序列。该路径至少包含一个节点，且不一定经过根节点。

__示例 :__
示例 1:

输入: [1,2,3]
```
       1
      / \
     2   3
```
输出: 6

示例 2:

输入: [-10,9,20,null,null,15,7]
```
   -10
   / \
  9  20
    /  \
   15   7
```
输出: 42

__思路__:
递归法
维护一个全局变量记录最大值
只考虑节点对最大值的正的贡献值
时间复杂度O(n), 空间复杂度O(1), 不考虑栈的空间

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
    int maxPathSum(TreeNode* root) 
    {
        helper(root);
        return result;
    }
private:
    int result = INT_MIN;
    int helper(TreeNode* root)
    {
        if (!root) return 0;
        int l = max(helper(root -> left), 0), r = max(helper(root -> right), 0);
        result = max(root -> val + l + r, result);
        return root -> val + max(l, r);
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
    private int result = Integer.MIN_VALUE;
    
    public int maxPathSum(TreeNode root) {
        helper(root);
        return result;
    }
    
    private int helper(TreeNode root) {
        if (root == null) return 0;
        int l = helper(root.left), r = helper(root.right), v = root.val;
        result = Math.max(result, v + l + r);
        v += Math.max(l, r);
        return Math.max(v, 0);
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
    def maxPathSum(self, root: TreeNode, first=True) -> int:
        if not root: 
            return 0
        l, r = self.maxPathSum(root.left, False), self.maxPathSum(root.right, False)
        self.result = max(getattr(self, 'result', float('-inf')), l + root.val + r)
        return self.result if first else max(root.val + max(l, r), 0)
```