# 250 Count Univalue Subtrees 统计同值子树

__Description:__

Given the root of a binary tree, return the number of uni-value subtrees.

A uni-value subtree means all nodes of the subtree have the same value.

__Example:__

Example 1:

![Binary Tree](https://assets.leetcode.com/uploads/2020/08/21/unival_e1.jpg)

Input: root = [5,1,5,5,5,null,5]
Output: 4

Example 2:

Input: root = []
Output: 0

Example 3:

Input: root = [5,5,5,5,5,null,5]
Output: 6

__Constraints:__

The number of the node in the tree will be in the range [0, 1000].
-1000 <= Node.val <= 1000

__题目描述：__

给定一个二叉树，统计该二叉树数值相同的子树个数。

同值子树是指该子树的所有节点都拥有相同的数值。

__示例：__

输入: root = [5,1,5,5,5,null,5]

```text
              5
             / \
            1   5
           / \   \
          5   5   5
```

输出: 4

__思路：__

递归后序遍历
当遍历到空结点时返回 true
先遍历左子树, 再遍历右子树, 最后遍历根结点
如果为叶子结点为 true, 计数器加 1
从叶子结点往上遍历, 子树值完全相同的计数器加 1
即左子树右子树和根结点值相等
时间复杂度为 O(n), 空间复杂度为 O(n), 最差情况树退化为链表

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
    int countUnivalSubtrees(TreeNode* root) 
    {
        result = 0;
        dfs(root);
        return result;
    }
private:
    int result;
    
    bool dfs(TreeNode* root) 
    {
        if (!root) return true;
        bool left = dfs(root -> left), right = dfs(root -> right), cur = (!root -> left or root -> val == root -> left -> val) and (!root -> right or root -> val == root -> right -> val);
        if (cur and left and right) ++result;
        return cur and left and right;
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
    private int result = 0;
    
    public int countUnivalSubtrees(TreeNode root) {
        dfs(root);
        return result;
    }
    
    private boolean dfs(TreeNode root) {
        if (root == null) return true;
        boolean left = dfs(root.left), right = dfs(root.right), cur = (root.left == null || root.val == root.left.val) && (root.right == null || root.val == root.right.val);
        if (cur && left && right) ++result;
        return cur && left && right;
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
    def countUnivalSubtrees(self, root: Optional[TreeNode]) -> int:
        count = 0
        def dfs(root: Optional[TreeNode]) -> bool:
            nonlocal count
            if not root:
                return True
            left, right = dfs(root.left), dfs(root.right)
            count += (cur := (not root.left or root.val == root.left.val) and (not root.right or root.val == root.right.val)) and left and right
            return cur and left and right
        dfs(root)
        return count
```
