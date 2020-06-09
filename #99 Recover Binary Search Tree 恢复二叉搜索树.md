__Description__:
Two elements of a binary search tree (BST) are swapped by mistake.

Recover the tree without changing its structure.

__Example:__
Example 1:

Input: [1,3,null,null,2]
```
   1
  /
 3
  \
   2
```
Output: [3,1,null,null,2]
```
   3
  /
 1
  \
   2
```

Example 2:

Input: [3,1,4,null,null,2]
```
  3
 / \
1   4
   /
  2
```
Output: [2,1,4,null,null,3]
```
  2
 / \
1   4
   /
  3
```

__Follow up:__

A solution using O(n) space is pretty straight forward.
Could you devise a constant space solution?

__题目描述__:
二叉搜索树中的两个节点被错误地交换。

请在不改变其结构的情况下，恢复这棵树。

__示例 :__
示例 1:

输入: [1,3,null,null,2]
```
   1
  /
 3
  \
   2
```
输出: [3,1,null,null,2]
```
   3
  /
 1
  \
   2
```

示例 2:

输入: [3,1,4,null,null,2]
```
  3
 / \
1   4
   /
  2
```
输出: [2,1,4,null,null,3]
```
  2
 / \
1   4
   /
  3
```
__进阶:__

使用 O(n) 空间复杂度的解法很容易实现。
你能想出一个只使用常数空间的解决方案吗？

__思路__:
使用 morris遍历 BST(即线索二叉树)
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
    void recoverTree(TreeNode* root) 
    {
        TreeNode *p = nullptr, *t1 = nullptr, *t2 = nullptr;
        while (root)
        {
            if (root -> left)
            {
                TreeNode *pre = get_predecessor(root);
                if (!pre -> right)
                {
                    pre -> right = root;
                    root = root -> left;
                    continue;
                }
                else if (pre -> right == root) pre -> right = nullptr;
            }
            if (p && p -> val > root -> val)
            {
                if (!t1) t1 = p;
                t2 = root;
            }
            p = root;
            root = root -> right;
        }
        swap(t1 -> val, t2 -> val);
    }
private:
    TreeNode* get_predecessor(TreeNode* &root)
    {
        TreeNode* pre = root;
        if (root)
        {
            pre = pre -> left;
            while (pre -> right && pre -> right != root) pre = pre -> right;
        }
        return pre;
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
    private TreeNode p, t1, t2;
    public void recoverTree(TreeNode root) {
        helper(root);
        t1.val ^= t2.val;
        t2.val ^= t1.val;
        t1.val ^= t2.val;
    }
    
    private void helper(TreeNode root) {
        if (root == null) return;
        helper(root.left);
        if (p != null && p.val > root.val) {
            if (t1 == null) t1 = p;
            t2 = root;
        }
        p = root;
        helper(root.right);
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
    def recoverTree(self, root: TreeNode) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        self.p = self.t1 = self.t2 = None
        def helper(root: TreeNode) -> None:
            if not root:
                return
            helper(root.left)
            if self.p and self.p.val > root.val:
                if not self.t1:
                    self.t1 = self.p
                self.t2 = root
            self.p = root
            helper(root.right)
        helper(root)
        self.t1.val, self.t2.val = self.t2.val, self.t1.val
```