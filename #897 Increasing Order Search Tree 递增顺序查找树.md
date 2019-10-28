__Description__:
Given a binary search tree, rearrange the tree in in-order so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only 1 right child.

__Example:__
Example 1:
Input: [5,3,6,2,4,null,8,1,null,null,null,7,9]
```
       5
      / \
    3    6
   / \    \
  2   4    8
 /        / \ 
1        7   9
```

Output: [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]
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
            \
             7
              \
               8
                \
                 9  
```

__Note:__

The number of nodes in the given tree will be between 1 and 100.
Each node will have a unique integer value from 0 to 1000.

__题目描述__:
给定一个树，按中序遍历重新排列树，使树中最左边的结点现在是树的根，并且每个结点没有左子结点，只有一个右子结点。

__示例 :__

输入：[5,3,6,2,4,null,8,1,null,null,null,7,9]
```
       5
      / \
    3    6
   / \    \
  2   4    8
 /        / \ 
1        7   9
```

输出：[1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]
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
            \
             7
              \
               8
                \
                 9  
```
 
__提示：__

给定树中的结点数介于 1 和 100 之间。
每个结点都有一个从 0 到 1000 范围内的唯一整数值。

__思路__:
1. 使用栈按中序遍历存储结点
2. 递归法, 记录子树的父结点
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
    TreeNode* increasingBST(TreeNode* root) 
    {
        stack<TreeNode*> s;
        TreeNode* result = new TreeNode(0);
        TreeNode* p = result;
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
                cur -> left = nullptr;
                p -> right = cur;
                p = p -> right;
            }
        }
        return result -> right;
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
    public TreeNode increasingBST(TreeNode root) {
        return helper(root, null);
    }
    
    private TreeNode helper(TreeNode root, TreeNode temp) {
        if (root == null) return temp;
        TreeNode result = helper(root.left, root);
        root.left = null;
        root.right = helper(root.right, temp);
        return result;
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
    def increasingBST(self, root: TreeNode) -> TreeNode:
        def helper(root: TreeNode, temp: TreeNode=None) -> TreeNode:
            if not root:
                return temp
            result, root.left, root.right = helper(root.left, root), None, helper(root.right, temp)
            return result
        return helper(root)
```