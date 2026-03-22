# 543 Diameter of Binary Tree 二叉树的直径

__Description__:
Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

__Example:__

Given a binary tree

```text
          1
         / \
        2   3
       / \
      4   5
```

Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].

__Note:__
The length of path between two nodes is represented by the number of edges between them.

__题目描述__:
给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。

__示例 :__
给定二叉树

```text
          1
         / \
        2   3
       / \
      4   5
```

返回 3, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

__注意：__
两结点之间的路径长度是以它们之间边的数目表示。

__思路__:

对每个结点求左子树和右子树深度, 取左右子树深度和的最大值
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
    int diameterOfBinaryTree(TreeNode* root) 
    {
        if (!root) return 0;
        int result = 0;
        unordered_map<TreeNode*, int> depths;
        stack<TreeNode*> s;
        s.push(root);
        while (s.size()) 
        {
            TreeNode* cur = s.top();
            if (cur -> left and depths.find(cur -> left) == depths.end()) s.push(cur -> left);
            else if (cur -> right and depths.find(cur -> right) == depths.end()) s.push(cur -> right);
            else 
            {
                s.pop();
                int l = depths[cur -> left], r = depths[cur -> right];
                depths[cur] = max(l, r) + 1;
                result = max(result, l + r);
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
    private int result = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        depth(root);
        return result;
    }

    private int depth(TreeNode root) {
        if (root == null) return 0;
        int l = depth(root.left);
        int r = depth(root.right);
        result = Math.max(l + r, result);
        return Math.max(l, r) + 1;
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
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        if not root:
            return 0
        result = 0
        def depth(root: TreeNode) -> int:
            nonlocal result
            if not root:
                return 0
            l, r = depth(root.left), depth(root.right)
            result = max(l + r, result)
            return max(l, r) + 1
        depth(root)
        return result
```
