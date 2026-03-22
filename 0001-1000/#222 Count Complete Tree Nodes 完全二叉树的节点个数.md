# 222 Count Complete Tree Nodes 完全二叉树的节点个数

__Description__:
Given a complete binary tree, count the number of nodes.

__Note:__

Definition of a complete binary tree from [Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees):
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

__Example:__

Input:

```text
    1
   / \
  2   3
 / \  /
4  5 6
```

Output: 6

__题目描述__:
给出一个完全二叉树，求出该树的节点个数。

__说明：__

[完全二叉树](https://baike.baidu.com/item/%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91/7773232?fr=aladdin)的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2h 个节点。

__示例 :__

输入:

```text
    1
   / \
  2   3
 / \  /
4  5 6
```

输出: 6

__思路__:

1. 求结点数, 简单递归
当根结点为空返回 0
否则返回 1 + 左子树结点 + 右子树结点
时间复杂度O(n), 空间复杂度O(lgn), lgn表示树的深度
2. 由于该题已经明确是完全二叉树, 只需要求最大深度即可
计算左子树和右子树的左子树的高度(因为是完全二叉树, 子树的高度取决与最后一层是否填满, 两者最多相差 1)

- 若左右高度相等, 说明左子树一定是满二叉树, 结点数等于2 ^ (左子树的高度) + 递归计算右子树
- 若左右高度不相等, 说明右子树一定是满二叉树, 结点数等于2 ^ (右子树的高度) + 递归计算左子树
时间复杂度O(lgn * lgn), 空间复杂度O(1), 时间复杂度推导: T(n) = T(n / 2) + logn

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
    int countNodes(TreeNode* root) 
    {
        if (!root) return 0;
        int left = height(root -> left);
        int right = height(root -> right);
        return left == right ? countNodes(root -> right) + (1 << left) : countNodes(root -> left) + (1 << right);
    }
private:
    int height(TreeNode* root)
    {
        int result = 0;
        while (root)
        {
            result++;
            root = root -> left;
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
    public int countNodes(TreeNode root) {
        return root == null ? 0 : 1 + countNodes(root.left) + countNodes(root.right);
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
    def countNodes(self, root: TreeNode) -> int:
        return 0 if not root else 1 + self.countNodes(root.left) + self.countNodes(root.right)
```
