__Description__:
Given a binary tree, return the postorder traversal of its nodes' values.

__Example:__

Input: [1,null,2,3]
```
   1
    \
     2
    /
   3
```
Output: [3,2,1]

__Follow up:__
Recursive solution is trivial, could you do it iteratively?

__题目描述__:
给定一个二叉树，返回它的 前序 遍历。

__示例 :__

输入: [1,null,2,3]  
```
   1
    \
     2
    /
   3 
```
输出: [3,2,1]

__进阶：__
递归算法很简单，你可以通过迭代算法完成吗？

__思路__:
1. 递归法
处理 root.left
处理 root.right
处理 root
时间复杂度O(n), 空间复杂度O(n)
2. 迭代法
用栈存储
先压入 root.left
再压入 root.right
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
    vector<int> postorderTraversal(TreeNode* root) 
    {
        stack<TreeNode*> s;
        vector<int> result;
        if (!root) return result;
        s.push(root);
        while (s.size())
        {
            TreeNode* cur = s.top();
            s.pop();
            if (cur -> left) s.push(cur -> left);
            if (cur -> right) s.push(cur -> right);
            result.insert(result.begin(), cur -> val);
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
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        postorder(root, result);
        return result;
    }
    
    private void postorder(TreeNode root, List<Integer> result) {
        if (root == null) return;
        postorder(root.left, result);
        postorder(root.right, result);
        result.add(root.val);
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
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        return self.postorderTraversal(root.left) + self.postorderTraversal(root.right) + [root.val] if root else []
```