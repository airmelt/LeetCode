__Description__:
Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

__Example__:
Example 1:
Input:   
```
           1         1
          / \       / \
         2   3     2   3
        [1,2,3],   [1,2,3]
```
Output: true

Example 2:
Input:     
```
          1          1
          /           \
         2             2
        [1,2],     [1,null,2]
```
Output: false

Example 3:
Input:     
```
           1         1
          / \       / \
         2   1     1   2
        [1,2,1],   [1,1,2]
```
Output: false

__题目描述__:
给定两个二叉树，编写一个函数来检验它们是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

 __示例__:
示例 1:
输入:
```
           1         1
          / \       / \
         2   3     2   3
        [1,2,3],   [1,2,3]
```
输出: true

示例 2:
输入:
```
          1          1
          /           \
         2             2
        [1,2],     [1,null,2]
```
输出: false

示例 3:
输入:
```
           1         1
          / \       / \
         2   1     1   2
        [1,2,1],   [1,1,2]
```
输出: false

__思路__:
先判断两个树是否空, 两棵树都为空才返回true
否则比较两棵树的左右两边, 可以用递归/迭代方式
时间复杂度O(n), 空间复杂度O(n), n为树中结点数量

__代码__:
__C++__:
```
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
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (!p && !q) return true;
        if (p && q && p -> val == q -> val) return isSameTree(p -> left, q -> left) && isSameTree(p -> right, q -> right);
        else return false;
    }
};
```

__Java__:
```
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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) return true;
        if (p != null && q != null && p.val == q.val) return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
        else return false;
    }
}
```

__Python__:
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        stack = [(p, q)]
        while stack:
            m, n = stack.pop(0)
            if m and n and m.val == n.val:
                stack.append((m.left, n.left))
                stack.append((m.right, n.right))
            else:
                if m != n:
                    return False
        return True
```
