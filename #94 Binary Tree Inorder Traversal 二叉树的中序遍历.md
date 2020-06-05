__Description__:
Given a binary tree, return the inorder traversal of its nodes' values.

__Example:__

Input: [1,null,2,3]
```
   1
    \
     2
    /
   3
```
Output: [1,3,2]

__Follow up:__
 Recursive solution is trivial, could you do it iteratively?

__题目描述__:
给定一个二叉树，返回它的中序 遍历。

__示例 :__

输入: [1,null,2,3]
```
   1
    \
     2
    /
   3
```
输出: [1,3,2]

__进阶: __
递归算法很简单，你可以通过迭代算法完成吗？

__思路__:
1. 递归法
按照左中右递归加入即可
2. 迭代法
使用栈
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
    vector<int> inorderTraversal(TreeNode* root) 
    {
        vector<int> result;
        stack<TreeNode*> s;
        while (!s.empty() || root) 
        {
            while (root) 
            {
                s.push(root);
                root = root -> left;
            }
            auto cur = s.top();
            s.pop();
            result.push_back(cur -> val);
            root = cur -> right;
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
    private List<Integer> result = new ArrayList<>();
    public List<Integer> inorderTraversal(TreeNode root) {
        if (root != null) {
            inorderTraversal(root.left);
            result.add(root.val);
            inorderTraversal(root.right);
        }
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
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        return [] if not root else self.inorderTraversal(root.left) + [root.val] + self.inorderTraversal(root.right)
```