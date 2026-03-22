# 450 Delete Node in a BST 删除二叉搜索树中的节点

__Description__:
Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

Search for a node to remove.
If the node is found, delete the node.

__Follow up:__
Can you solve it with time complexity O(height of tree)?

__Example:__

Example 1:

![Delete Node 1](https://assets.leetcode.com/uploads/2020/09/04/del_node_1.jpg)

Input: root = [5,3,6,2,4,null,7], key = 3
Output: [5,4,6,2,null,null,7]
Explanation: Given key to delete is 3. So we find the node with value 3 and delete it.
One valid answer is [5,4,6,2,null,null,7], shown in the above BST.
Please notice that another valid answer is [5,2,6,null,4,null,7] and it's also accepted.

Example 2:

![Delete Node 2](https://assets.leetcode.com/uploads/2020/09/04/del_node_supp.jpg)

Input: root = [5,3,6,2,4,null,7], key = 0
Output: [5,3,6,2,4,null,7]
Explanation: The tree does not contain a node with value = 0.

Example 3:

Input: root = [], key = 0
Output: []

__Constraints:__

The number of nodes in the tree is in the range [0, 10^4].
-10^5 <= Node.val <= 10^5
Each node has a unique value.
root is a valid binary search tree.
-10^5 <= key <= 10^5

__题目描述__:
给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的 key 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：

首先找到需要删除的节点；
如果找到了，删除它。

__说明：__
要求算法时间复杂度为 O(h)，h 为树的高度。

__示例 :__

root = [5,3,6,2,4,null,7]
key = 3

```text
    5
   / \
  3   6
 / \   \
2   4   7
```

给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。

一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。

```text
    5
   / \
  4   6
 /     \
2       7
```

另一个正确答案是 [5,2,6,null,4,null,7]。

```text
    5
   / \
  2   6
   \   \
    4   7
```

__思路__:

递归
先通过二叉搜索树的性质查找到需要删去的节点

1. 如果该节点左右孩子中有一个为空, 则用另一个子树代替删除节点即可
2. 如果该节点为叶子节点, 直接删除即可
3. 如果该节点两个孩子都存在, 则用右子树中的最小值来代替该节点, 因为右子树中的节点都大于待删除节点, 所以这个最小值一定大于左子树所有节点的值, 不会破坏二叉搜索树的性质

查找右子树的最小值只需要一直往左走到底即可
时间复杂度O(h), 空间复杂度O(h), 其中 h为二叉搜索树的高度, 最小为平衡二叉树时 h = lgn

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
    TreeNode* deleteNode(TreeNode* root, int key) 
    {
        if (!root) return root;
        if (root -> val > key) root -> left = deleteNode(root -> left, key);
        else if (root -> val < key) root -> right = deleteNode(root -> right, key);
        else
        {
            if (!root -> left or !root -> right) root = root -> left ? root -> left : root -> right;
            else
            {
                TreeNode *cur = root -> right;
                while (cur -> left) cur = cur -> left;
                root -> val = cur -> val;
                root -> right = deleteNode(root -> right, cur -> val);
            }
        }
        return root;
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
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return root;
        if (root.val > key) root.left = deleteNode(root.left, key);
        else if (root.val < key) root.right = deleteNode(root.right, key);
        else {
            if (root.left == null || root.right == null) root = root.left == null ? root.right : root.left;
            else {
                TreeNode cur = root.right;
                while (cur.left != null) cur = cur.left;
                root.val = cur.val;
                root.right = deleteNode(root.right, cur.val);
            }
        }
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
    def deleteNode(self, root: TreeNode, key: int) -> TreeNode:
        if not root:
            return None
        if root.val > key:
            root.left = self.deleteNode(root.left, key)
        elif root.val < key:
            root.right = self.deleteNode(root.right, key)
        else:
            if not root.left or not root.right:
                root = root.left if root.left else root.right
            else:
                cur = root.right
                while cur.left:
                    cur = cur.left
                root.val = cur.val
                root.right = self.deleteNode(root.right, cur.val)
        return root
```
