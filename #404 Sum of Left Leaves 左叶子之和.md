# 404 Sum of Left Leaves 左叶子之和

__Description__:
Find the sum of all left leaves in a given binary tree.

__Example:__

```text
    3
   / \
  9  20
    /  \
   15   7
```

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.

__题目描述__:
计算给定二叉树的所有左叶子之和。

__示例：__

```text
    3
   / \
  9  20
    /  \
   15   7
```

在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24

__思路__:

1. 递归, 找到结点的左子树非空且左子树的子树为空即为左叶子
2. 迭代, 空间复杂度略优于递归方法
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
    int sumOfLeftLeaves(TreeNode* root) 
    {
        int result = 0;
        if (!root) return 0;
        vector<TreeNode*> nodes;
        nodes.push_back(root);
        while (nodes.size()) 
        {
            TreeNode* temp = nodes[0];
            nodes.erase(nodes.begin());
            if (temp -> left and !temp -> left -> left && !temp -> left -> right) result += temp -> left -> val;
            if (temp -> left) nodes.push_back(temp -> left);
            if (temp -> right) nodes.push_back(temp -> right);
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
    public int sumOfLeftLeaves(TreeNode root) {
        return root == null ? 0 : (root.left != null && root.left.left == null && root.left.right == null) ? root.left.val + sumOfLeftLeaves(root.right) : sumOfLeftLeaves(root.left) + sumOfLeftLeaves(root.right);
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
    def sumOfLeftLeaves(self, root: TreeNode) -> int:
        return 0 if not root else root.left.val + self.sumOfLeftLeaves(root.right) if root.left and not root.left.left and not root.left.right else return self.sumOfLeftLeaves(root.left) + self.sumOfLeftLeaves(root.right)
```
