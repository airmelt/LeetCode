# 105 Construct Binary Tree from Preorder and Inorder Traversal 从前序与中序遍历序列构造二叉树

__Description__:
Given preorder and inorder traversal of a tree, construct the binary tree.

__Note:__
You may assume that duplicates do not exist in the tree.

__Example:__

For example, given

preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
Return the following binary tree:

```text
    3
   / \
  9  20
    /  \
   15   7
```

__题目描述__:
根据一棵树的前序遍历与中序遍历构造二叉树。

__注意:__
你可以假设树中没有重复的元素。

__示例 :__

例如，给出

前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：

```text
    3
   / \
  9  20
    /  \
   15   7
```

__思路__:

1. 递归法
前序遍历第一个为根节点
找到前序遍历第一个元素在中序遍历的下标, 将中序遍历列表分为 2个部分, 前半部分为左子树, 后半部分为右子树
2. 迭代法
用栈记录前序遍历的节点不等于中序遍历的节点的值的节点
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
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) 
    {
        if (preorder.empty()) return nullptr;
        TreeNode* root = new TreeNode(preorder.front());
        int index = 0;
        while (index < inorder.size() and preorder.front() != inorder[index]) ++index;
        vector<int> pre(preorder.begin() + 1, preorder.begin() + 1 + index);
        vector<int> in(inorder.begin(), inorder.begin() + index);
        root -> left = buildTree(pre, in);
        pre.assign(preorder.begin() + 1 + index, preorder.end());
        in.assign(inorder.begin() + index + 1, inorder.end());
        root -> right = buildTree(pre, in);
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
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder == null || preorder.length == 0) return null;
        TreeNode root = new TreeNode(preorder[0]);
        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.push(root);
        int index = 0;
        for (int i = 1; i < preorder.length; i++) {
            int temp = preorder[i];
            TreeNode node = stack.peek();
            if (node.val != inorder[index]) {
                node.left = new TreeNode(temp);
                stack.push(node.left);
            } else {
                while (!stack.isEmpty() && stack.peek().val == inorder[index]) {
                    node = stack.pop();
                    index++;
                }
                node.right = new TreeNode(temp);
                stack.push(node.right);
            }
        }
        return root;
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
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        if not preorder:
            return
        root, i = TreeNode(preorder[0]), inorder.index(preorder.pop(0))
        root.left = self.buildTree(preorder[:i], inorder[:i])
        root.right = self.buildTree(preorder[i:], inorder[i + 1:])
        return root
```
