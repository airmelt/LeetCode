# 156 Binary Tree Upside Down 上下翻转二叉树

__Description:__

Given the root of a binary tree, turn the tree upside down and return the new root.

You can turn a binary tree upside down with the following steps:

The original left child becomes the new root.
The original root becomes the new right child.
The original right child becomes the new left child.

The mentioned steps are done level by level. It is guaranteed that every right node has a sibling (a left node with the same parent) and has no children.

![Binary Tree 1](https://assets.leetcode.com/uploads/2020/08/29/main.jpg)

__Example:__

Example 1:

![Binary Tree 2](https://assets.leetcode.com/uploads/2020/08/29/updown.jpg)

Input: root = [1,2,3,4,5]
Output: [4,5,2,null,null,3,1]

Example 2:

Input: root = []
Output: []

Example 3:

Input: root = [1]
Output: [1]

__Constraints:__

The number of nodes in the tree will be in the range [0, 10].
1 <= Node.val <= 10
Every right node in the tree has a sibling (a left node that shares the same parent).
Every right node in the tree has no children.

__题目描述：__

给你一个二叉树的根节点 root ，请你将此二叉树上下翻转，并返回新的根节点。

你可以按下面的步骤翻转一棵二叉树：

原来的左子节点变成新的根节点
原来的根节点变成新的右子节点
原来的右子节点变成新的左子节点

上面的步骤逐层进行。题目数据保证每个右节点都有一个同级节点（即共享同一父节点的左节点）且不存在子节点。

![二叉树 1](https://assets.leetcode.com/uploads/2020/08/29/main.jpg)

__示例：__

示例 1：

![二叉树 2](https://assets.leetcode.com/uploads/2020/08/29/updown.jpg)

输入：root = [1,2,3,4,5]
输出：[4,5,2,null,null,3,1]

示例 2：

输入：root = []
输出：[]

示例 3：

输入：root = [1]
输出：[1]

__提示：__

树中节点数目在范围 [0, 10] 内
1 <= Node.val <= 10
树中的每个右节点都有一个同级节点（即共享同一父节点的左节点）
树中的每个右节点都没有子节点

__思路：__

模拟
记录最后返回的结点为 parent 也是根结点, 这个结点实际上应该是二叉树最左下角的结点
记录 parent 的右结点为 parent_right
先记录 root 的左结点 root_left
将 parent_right 赋予 root.left, 可从示例 1 的第三层和第二层理解
然后更新 parent_right 为 root -> right 和 root -> right 为 parent
最后 root 向左移动
时间复杂度为 O(n), 空间复杂度为 O(1)

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
    TreeNode* upsideDownBinaryTree(TreeNode* root) 
    {
        TreeNode *parent = nullptr, *parent_right = nullptr;
        while (root) 
        {
            TreeNode* root_left = root -> left;
            root -> left = parent_right;
            parent_right = root -> right;
            root -> right = parent;
            parent = root;
            root = root_left;
        }
        return parent;
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
    public TreeNode upsideDownBinaryTree(TreeNode root) {
        TreeNode parent = null, parentRight = null;
        while (root != null) {
            TreeNode rootLeft = root.left;
            root.left = parentRight;
            parentRight = root.right;
            root.right = parent;
            parent = root;
            root = rootLeft;
        }
        return parent;
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
    def upsideDownBinaryTree(self, root: TreeNode) -> TreeNode:
        parent = parent_right = None
        while root:
            root_left = root.left
            root.left = parent_right
            parent_right = root.right
            root.right = parent
            parent = root
            root = root_left
        return parent
```
