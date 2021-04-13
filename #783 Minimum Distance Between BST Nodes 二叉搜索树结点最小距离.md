# 783 Minimum Distance Between BST Nodes 二叉搜索树结点最小距离

__Description__:
Given a Binary Search Tree (BST) with the root node root, return the minimum difference between the values of any two different nodes in the tree.

__Example:__

Input: root = [4,2,6,1,3,null,null]
Output: 1
Explanation:
Note that root is a TreeNode object, not an array.

The given tree [4,2,6,1,3,null,null] is represented by the following diagram:

```text
          4
        /   \
      2      6
     / \    
    1   3  
```

while the minimum difference in this tree is 1, it occurs between node 1 and node 2, also between node 3 and node 2.

__Note:__

The size of the BST will be between 2 and 100.
The BST is always valid, each node's value is an integer, and each node's value is different.

__题目描述__:
给定一个二叉搜索树的根结点 root, 返回树中任意两节点的差的最小值。

__示例 :__

输入: root = [4,2,6,1,3,null,null]
输出: 1
解释:
注意，root是树结点对象(TreeNode object)，而不是数组。

给定的树 [4,2,6,1,3,null,null] 可表示为下图:

```text
          4
        /   \
      2      6
     / \    
    1   3  
```

最小的差值是 1, 它是节点1和节点2的差值, 也是节点3和节点2的差值。

__注意：__

二叉树的大小范围在 2 到 100。
二叉树总是有效的，每个节点的值都是整数，且不重复。

__思路__:

本题与[LeetCode #530 Minimum Absolute Difference in BST 二叉搜索树的最小绝对差](https://www.jianshu.com/p/b3b3dc64be1d)重复
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
class Solution 
{
public:
    int minDiffInBST(TreeNode* root) 
    {
        stack<TreeNode*> s;
        TreeNode* pre = NULL;
        int result = INT_MAX;
        while (root or s.size()) 
        {
            if (root) 
            {
                s.push(root);
                root = root -> left;
            } 
            else 
            {
                root = s.top();
                s.pop();
                if (pre) result = min(result, abs(root -> val - pre -> val));
                pre = root;
                root = root -> right;
            }
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
    int result = Integer.MAX_VALUE;
    TreeNode pre = null;
    public int minDiffInBST(TreeNode root) {
        inOrder(root);
        return result;
    }
    
    private void inOrder(TreeNode root) {
        if (root == null) return;
        inOrder(root.left);
        if (pre != null) result = Math.min(result, root.val - pre.val);
        pre = root;
        inOrder(root.right);
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
    def minDiffInBST(self, root: TreeNode) -> int:
        result, pre = (1 << 31) - 1, 1 - (1 << 31)
        def in_order(root: TreeNode):
            nonlocal pre, result
            if not root:
                return
            in_order(root.left)
            result = min(root.val - pre, result)
            pre = root.val
            in_order(root.right)
        in_order(root)
        return result
```
