# 1038 Binary Search Tree to Greater Sum Tree 从二叉搜索树到更大和树

__Description__:
Given the root of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus the sum of all keys greater than the original key in BST.

As a reminder, a binary search tree is a tree that satisfies these constraints:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.

__Example:__

Example 1:

![Binary Search Tree](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/05/03/tree.png)

Input: root = [4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
Output: [30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]

Example 2:

Input: root = [0,null,1]
Output: [1,null,1]

__Constraints:__

The number of nodes in the tree is in the range [1, 100].
0 <= Node.val <= 100
All the values in the tree are unique.

__Note:__
This question is the same as [538](https://leetcode.com/problems/convert-bst-to-greater-tree/)

__题目描述__:
给定一个二叉搜索树 root (BST)，请将它的每个节点的值替换成树中大于或者等于该节点值的所有节点值之和。

提醒一下， 二叉搜索树 满足下列约束条件：

节点的左子树仅包含键 小于 节点键的节点。
节点的右子树仅包含键 大于 节点键的节点。
左右子树也必须是二叉搜索树。

__示例 :__

示例 1：

![二叉搜索树](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/05/03/tree.png)

输入：[4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
输出：[30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]

示例 2：

输入：root = [0,null,1]
输出：[1,null,1]

__提示:__

树中的节点数在 [1, 100] 范围内。
0 <= Node.val <= 100
树中的所有值均 不重复 。

__注意：__
该题目与 [538](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)  相同

__思路__:

遍历

1. 先计算所有数字之和, 然后按照先序遍历给各个结点赋值
2. 按照右 -> 根 -> 左的顺序记录结点的值并累加
可以用递归和迭代解决
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
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution 
{
public:
    TreeNode* bstToGst(TreeNode* root) 
    {
        int pre = 0;
        order(root, pre);
        return root;
    }
private:
    void order(TreeNode* root, int &pre) 
    {
        if (!root) return;
        stack<TreeNode*> s;
        while (root or s.size()) 
        {
            if (root) 
            {
                s.push(root);
                root = root -> right;
            } 
            else 
            {
                root = s.top();
                s.pop();
                root -> val += pre;
                pre = root -> val;
                root = root -> left;
            }
        }
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
    private int pre = 0;
    public TreeNode bstToGst(TreeNode root) {
        if (root == null) return root;
        bstToGst(root.right);
        pre += root.val;
        root.val = pre;
        bstToGst(root.left);
        return root;
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
    pre = 0
    def bstToGst(self, root: TreeNode) -> TreeNode:
        if not root:
            return root
        self.bstToGst(root.right)
        self.pre += root.val
        root.val = self.pre
        self.bstToGst(root.left)
        return root
```
