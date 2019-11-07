__Description__:
Given the root node of a binary search tree, return the sum of values of all nodes with value between L and R (inclusive).

The binary search tree is guaranteed to have unique values.

__Example:__
Example 1:

Input: root = [10,5,15,3,7,null,18], L = 7, R = 15
Output: 32

Example 2:

Input: root = [10,5,15,3,7,13,18,1,null,6], L = 6, R = 10
Output: 23
 
__Note:__

The number of nodes in the tree is at most 10000.
The final answer is guaranteed to be less than 2^31.

__题目描述__:
给定二叉搜索树的根结点 root，返回 L 和 R（含）之间的所有结点的值的和。

二叉搜索树保证具有唯一的值。

__示例 :__
示例 1：

输入：root = [10,5,15,3,7,null,18], L = 7, R = 15
输出：32

示例 2：

输入：root = [10,5,15,3,7,13,18,1,null,6], L = 6, R = 10
输出：23
 
__提示：__

树中的结点数量最多为 10000 个。
最终的答案保证小于 2^31。

__思路__:
注意到题目给的是 BST(二叉搜索树), 比 root小的在 root的左子树, 比 root大的在 root的右子树
使用中序遍历, 落在 L和 R的范围中的就加入结果
1. 递归
2. 迭代
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
class Solution {
public:
    int rangeSumBST(TreeNode* root, int L, int R) 
    {
        stack<TreeNode*> s;
        int result = 0;
        while (s.size() || root)
        {
            if (root)
            {
                s.push(root);
                root = root -> left;
            }
            else
            {
                TreeNode* cur = s.top();
                s.pop();
                root = cur -> right;
                if (cur -> val >= L && cur -> val <= R) result += cur -> val;
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
    public int rangeSumBST(TreeNode root, int L, int R) {
        if (root == null) return 0;
        else if (root.val > R) return rangeSumBST(root.left, L, R);
        else if (root.val < L) return rangeSumBST(root.right, L, R);
        else return root.val + rangeSumBST(root.left, L, R) + rangeSumBST(root.right, L, R);
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
    def rangeSumBST(self, root: TreeNode, L: int, R: int) -> int:
        if not root:
            return 0
        elif root.val > R:
            return self.rangeSumBST(root.left, L, R);
        elif root.val < L:
            return self.rangeSumBST(root.right, L, R);
        else:
            return root.val + self.rangeSumBST(root.left, L, R) + self.rangeSumBST(root.right, L, R);
```