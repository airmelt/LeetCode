# 530 Minimum Absolute Difference in BST 二叉搜索树的最小绝对差

__Description__:
Given a binary search tree with non-negative values, find the minimum absolute difference between values of any two nodes.

__Example:__

Input:

```text
   1
    \
     3
    /
   2
```

Output:
1

Explanation:
The minimum absolute difference is 1, which is the difference between 2 and 1 (or between 2 and 3).

__Note:__
There are at least two nodes in this BST.

__题目描述__:
给定一个所有节点为非负值的二叉搜索树，求树中任意两节点的差的绝对值的最小值。

示例 :

输入:

```text
   1
    \
     3
    /
   2
```

输出:
1

解释:
最小绝对差为1，其中 2 和 1 的差的绝对值为 1（或者 2 和 3）。

__注意:__
树中至少有2个节点。

__思路__:

二叉搜索树的中序遍历为递增的有序数组, 比较两个相邻结点差的最小值即可
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
    int getMinimumDifference(TreeNode* root) 
    {
        stack<TreeNode*> s;
        TreeNode* pre = nullptr;
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
    public int getMinimumDifference(TreeNode root) {
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
    def getMinimumDifference(self, root: TreeNode) -> int:
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
