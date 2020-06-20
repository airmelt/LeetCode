__Description__:
Given a binary tree, flatten it to a linked list in-place.

__Example:__
For example, given the following tree:
```
    1
   / \
  2   5
 / \   \
3   4   6
```
The flattened tree should look like:
```
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```

__题目描述__:
给定一个二叉树，原地将它展开为一个单链表。

__示例 :__
例如，给定二叉树
```
    1
   / \
  2   5
 / \   \
3   4   6
```
将其展开为：
```
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```

__思路__:
后序遍历
将右指针指向左子树
左子树的最后节点的右指针指向原来的右子树
将左指针置空
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
    void flatten(TreeNode* root) 
    {
        if (!root) return;
        flatten(root -> left);
        flatten(root -> right);
        TreeNode* temp = root -> right;
        root -> right = root -> left;
        root -> left = nullptr;
        while (root -> right) root = root -> right;
        root -> right = temp;
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
    public void flatten(TreeNode root) {
        if (root == null) return;
        flatten(root.left);
        flatten(root.right);
        TreeNode temp = root.right;
        root.right = root.left;
        root.left = null;
        while (root.right != null) root = root.right;
        root.right = temp;
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
    def flatten(self, root: TreeNode) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        s = []
        while root or s:
            while root:
                s.append(root)
                root = root.left
            if s:
                cur = s.pop()
                temp, cur.right, cur.left = cur.right, cur.left, None
                while cur.right:
                    cur = cur.right
                cur.right, root = temp, temp
```