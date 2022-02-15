# 1008 Construct Binary Search Tree from Preorder Traversal 前序遍历构造二叉搜索树

__Description__:
Given an array of integers preorder, which represents the preorder traversal of a BST (i.e., binary search tree), construct the tree and return its root.

It is guaranteed that there is always possible to find a binary search tree with the given requirements for the given test cases.

A binary search tree is a binary tree where for every node, any descendant of Node.left has a value strictly less than Node.val, and any descendant of Node.right has a value strictly greater than Node.val.

A preorder traversal of a binary tree displays the value of the node first, then traverses Node.left, then traverses Node.right.

__Example:__

Example 1:

![Binary Search Tree](https://assets.leetcode.com/uploads/2019/03/06/1266.png)

Input: preorder = [8,5,1,7,10,12]
Output: [8,5,10,1,7,null,12]

Example 2:

Input: preorder = [1,3]
Output: [1,null,3]

__Constraints:__

1 <= preorder.length <= 100
1 <= preorder[i] <= 1000
All the values of preorder are unique.

__题目描述__:
给定一个整数数组，它表示BST(即 二叉搜索树 )的 先序遍历 ，构造树并返回其根。

保证 对于给定的测试用例，总是有可能找到具有给定需求的二叉搜索树。

二叉搜索树 是一棵二叉树，其中每个节点， Node.left 的任何后代的值 严格小于 Node.val , Node.right 的任何后代的值 严格大于 Node.val。

二叉树的 前序遍历 首先显示节点的值，然后遍历Node.left，最后遍历Node.right。

__示例 :__

示例 1：

![二叉搜索树](https://assets.leetcode.com/uploads/2019/03/06/1266.png)

输入：preorder = [8,5,1,7,10,12]
输出：[8,5,10,1,7,null,12]

示例 2:

输入: preorder = [1,3]
输出: [1,null,3]

__提示:__

1 <= preorder.length <= 100
1 <= preorder[i] <= 10^8
preorder 中的值 互不相同

__思路__:

1. 二分递归
第一个元素即为根
比第一个元素小的所有元素为左子树
比第一个元素大的所有元素为右子树
递归用数组存储新元素
时间复杂度为 O(nlgn), 空间复杂度为 O(n)
2. 单调栈
类似前序遍历逆操作
第一个元素为根, 将根入栈
以栈顶为父结点, 当前遍历的结点为子结点
如果当前栈顶的值大于当前结点, 直接将子节点作为左结点
否则当前栈顶的值小于当前结点, 弹出栈中元素作为新的父节点直到栈空或者栈顶元素大于子结点元素值, 这时子结点为新的父结点的右结点
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
    TreeNode* bstFromPreorder(vector<int>& preorder) 
    {
        TreeNode* root = new TreeNode(preorder.front());
        stack<TreeNode*> s;
        s.push(root);
        for (int i = 1, n = preorder.size(); i < n; i++) 
        {
            TreeNode* cur = new TreeNode(preorder[i]), *parent = cur;
            if (preorder[i] < s.top() -> val) s.top() -> left = cur;
            else 
            {
                while (!s.empty() and preorder[i] > s.top() -> val) 
                {
                    parent = s.top(); 
                    s.pop();
                }
                parent -> right = cur;
            }
            s.push(cur);
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
    public TreeNode bstFromPreorder(int[] preorder) {
        TreeNode root = new TreeNode(preorder[0]);
        for (int i = 1; i < preorder.length; i++) create(root, preorder[i]);
        return root;
    }

    private TreeNode create(TreeNode root, int val) {
        if (root == null) return new TreeNode(val);
        if (val < root.val) root.left = create(root.left, val);
        else if (val > root.val) root.right = create(root.right, val);
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
    def bstFromPreorder(self, preorder: List[int]) -> Optional[TreeNode]:
        stack, n = [(root := TreeNode(preorder[0]))], len(preorder)
        for i in range(1, n):
            cur = TreeNode(preorder[i])
            if stack[-1].val > cur.val:
                stack[-1].left = cur
            else:
                while stack and stack[-1].val < cur.val:
                    parent = stack.pop()
                parent.right = cur
            stack.append(cur)
        return root
```
