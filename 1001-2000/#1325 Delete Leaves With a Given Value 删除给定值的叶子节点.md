# 1325 Delete Leaves With a Given Value 删除给定值的叶子节点

__Description:__

Given a binary tree root and an integer target, delete all the leaf nodes with value target.

Note that once you delete a leaf node with value target, if its parent node becomes a leaf node and has the value target, it should also be deleted (you need to continue doing that until you cannot).

__Example:__

Example 1:

![Binary Tree 1](https://assets.leetcode.com/uploads/2020/01/09/sample_1_1684.png)

Input: root = [1,2,3,2,null,2,4], target = 2
Output: [1,null,3,null,4]
Explanation: Leaf nodes in green with value (target = 2) are removed (Picture in left).
After removing, new nodes become leaf nodes with value (target = 2) (Picture in center).

Example 2:

![Binary Tree 2](https://assets.leetcode.com/uploads/2020/01/09/sample_2_1684.png)

Input: root = [1,3,3,3,2], target = 3
Output: [1,3,null,null,2]

Example 3:

![Binary Tree 3](https://assets.leetcode.com/uploads/2020/01/15/sample_3_1684.png)

Input: root = [1,2,null,2,null,2], target = 2
Output: [1]
Explanation: Leaf nodes in green with value (target = 2) are removed at each step.

__Constraints:__

The number of nodes in the tree is in the range [1, 3000].
1 <= Node.val, target <= 1000

__题目描述：__

给你一棵以 root 为根的二叉树和一个整数 target ，请你删除所有值为 target 的 叶子节点 。

注意，一旦删除值为 target 的叶子节点，它的父节点就可能变成叶子节点；如果新叶子节点的值恰好也是 target ，那么这个节点也应该被删除。

也就是说，你需要重复此过程直到不能继续删除。

__示例：__

示例 1：

![二叉树 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/16/sample_1_1684.png)

输入：root = [1,2,3,2,null,2,4], target = 2
输出：[1,null,3,null,4]
解释：
上面左边的图中，绿色节点为叶子节点，且它们的值与 target 相同（同为 2 ），它们会被删除，得到中间的图。
有一个新的节点变成了叶子节点且它的值与 target 相同，所以将再次进行删除，从而得到最右边的图。

示例 2：

![二叉树 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/16/sample_2_1684.png)

输入：root = [1,3,3,3,2], target = 3
输出：[1,3,null,null,2]

示例 3：

![二叉树 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/16/sample_3_1684.png)

输入：root = [1,2,null,2,null,2], target = 2
输出：[1]
解释：每一步都删除一个绿色的叶子节点（值为 2）。

示例 4：

输入：root = [1,1,1], target = 1
输出：[]

示例 5：

输入：root = [1,2,3], target = 1
输出：[1,2,3]

__提示：__

1 <= target <= 1000
每一棵树最多有 3000 个节点。
每一个节点值的范围是 [1, 1000] 。

__思路：__

递归
当前结点为空返回空
否则后序遍历二叉树
找到所有叶子结点的值为 target 的置空
时间复杂度为 O(n), 空间复杂度为 O(n)

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
    TreeNode* removeLeafNodes(TreeNode* root, int target) 
    {
        if (!root) return nullptr;
        root -> left = removeLeafNodes(root -> left, target);
        root -> right = removeLeafNodes(root -> right, target);
        return (root -> val == target and !root -> left and !root -> right) ? nullptr : root;
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
    public TreeNode removeLeafNodes(TreeNode root, int target) {
        if (root == null) return null;
        root.left = removeLeafNodes(root.left, target);
        root.right = removeLeafNodes(root.right, target);
        return (root.val == target && root.left == null && root.right == null) ? null : root;
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
    def removeLeafNodes(self, root: Optional[TreeNode], target: int) -> Optional[TreeNode]:
        if not root:
            return None
        root.left, root.right = self.removeLeafNodes(root.left, target), self.removeLeafNodes(root.right, target)
        return None if root.val == target and not root.left and not root.right else root
```
