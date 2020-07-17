__Description__:
Given a binary tree, return the preorder traversal of its nodes' values.

__Example:__

Input: [1,null,2,3]
```
   1
    \
     2
    /
   3
```
Output: [1,2,3]

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
输出: [1,2,3]

__进阶：__
递归算法很简单，你可以通过迭代算法完成吗？

__思路__:
1. 递归法
处理 root
处理 root.left
处理 root.right
时间复杂度O(n), 空间复杂度O(n)
2. 迭代法
用栈存储
先压入 root.right
再压入 root.left
时间复杂度O(n), 空间复杂度O(n)
如果采用线索二叉树的方式遍历可以将空间复杂度降为 O(1)

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
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        TreeNode* cur = root;
        while (cur) 
        {
            if (!cur -> left) 
            {
                result.push_back(cur -> val);
                cur = cur -> right;
            }
            else 
            {
                TreeNode* pre = cur -> left;
                while (pre -> right and pre -> right != cur) pre = pre -> right;

                if (!pre -> right) 
                {
                  result.push_back(cur -> val);
                  pre -> right = cur;
                  cur = cur -> left;
                }
                else
                {
                    pre -> right = nullptr;
                    cur = cur -> right;
                }
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
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        preorder(root, result);
        return result;
    }
    
    private void preorder(TreeNode root, List<Integer> result) {
        if (root == null) return;
        result.add(root.val);
        preorder(root.left, result);
        preorder(root.right, result);
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
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        return [root.val] + self.preorderTraversal(root.left) + self.preorderTraversal(root.right) if root else []
```