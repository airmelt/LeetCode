# 951 Flip Equivalent Binary Trees 翻转等价二叉树

__Description__:
For a binary tree T, we can define a flip operation as follows: choose any node, and swap the left and right child subtrees.

A binary tree X is flip equivalent to a binary tree Y if and only if we can make X equal to Y after some number of flip operations.

Given the roots of two binary trees root1 and root2, return true if the two trees are flip equivalent or false otherwise.

__Example:__

Example 1:

![Tree](https://assets.leetcode.com/uploads/2018/11/29/tree_ex.png)

Input: root1 = [1,2,3,4,5,6,null,null,null,7,8], root2 = [1,3,2,null,6,4,5,null,null,null,null,8,7]
Output: true
Explanation: We flipped at nodes with values 1, 3, and 5.

Example 2:

Input: root1 = [], root2 = []
Output: true
Example 3:

Input: root1 = [], root2 = [1]
Output: false

Example 4:

Input: root1 = [0,null,1], root2 = []
Output: false

Example 5:

Input: root1 = [0,null,1], root2 = [0,1]
Output: true

__Constraints:__

The number of nodes in each tree is in the range [0, 100].
Each tree will have unique node values in the range [0, 99].

__题目描述__:
我们可以为二叉树 T 定义一个翻转操作，如下所示：选择任意节点，然后交换它的左子树和右子树。

只要经过一定次数的翻转操作后，能使 X 等于 Y，我们就称二叉树 X 翻转等价于二叉树 Y。

编写一个判断两个二叉树是否是翻转等价的函数。这些树由根节点 root1 和 root2 给出。

__示例 :__

![树](https://assets.leetcode.com/uploads/2018/11/29/tree_ex.png)

输入：root1 = [1,2,3,4,5,6,null,null,null,7,8], root2 = [1,3,2,null,6,4,5,null,null,null,null,8,7]
输出：true
解释：我们翻转值为 1，3 以及 5 的三个节点。

__提示:__

每棵树最多有 100 个节点。
每棵树中的每个值都是唯一的、在 [0, 99] 范围内的整数。

__思路__:

递归
参考[LeetCode #226 Invert Binary Tree 翻转二叉树](https://www.jianshu.com/p/2ce88bab129e)
如果都为空返回 true
如果有一个为空返回 false
如果两个根节点值不相等返回 false
然后按照 root1.left, root2.left 和 root1.right, root2.right 或者 root1.right, root2.left 和 root1.left, root2.right 分别比较是否相等
时间复杂度为 O(n), 空间复杂度为 O(h), 其中 n 表示两棵树的最大结点数, h 表示两棵树的最大高度 h <= n

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
    bool flipEquiv(TreeNode* root1, TreeNode* root2) 
    {
        return (root1 == root2) ? true : (!root1 or !root2 or root1 -> val != root2 -> val) ? false : (flipEquiv(root1 -> left, root2 -> left) and flipEquiv(root1 -> right, root2 -> right) or flipEquiv(root1 -> left, root2 -> right) and flipEquiv(root1 -> right, root2 -> left));
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
    public boolean flipEquiv(TreeNode root1, TreeNode root2) {
        return (root1 == root2) ? true : (root1 == null || root2 == null || root1.val != root2.val) ? false : (flipEquiv(root1.left, root2.left) && flipEquiv(root1.right, root2.right) || flipEquiv(root1.left, root2.right) && flipEquiv(root1.right, root2.left));
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
    def flipEquiv(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> bool:
        return True if (root1 is None and root2 is None) else False if (root1 is None or root2 is None) else True if (root1.val == root2.val and (self.flipEquiv(root1.left, root2.left) and self.flipEquiv(root1.right, root2.right) or self.flipEquiv(root1.left, root2.right) and self.flipEquiv(root1.right, root2.left))) else False
```
