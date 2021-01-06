# 106 Construct Binary Tree from Inorder and Postorder Traversal 从中序与后序遍历序列构造二叉树

__Description__:
Given inorder and postorder traversal of a tree, construct the binary tree.

__Note:__
You may assume that duplicates do not exist in the tree.

__Example:__

For example, given

inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
Return the following binary tree:

```text
    3
   / \
  9  20
    /  \
   15   7
```

__题目描述__:
根据一棵树的中序遍历与后序遍历构造二叉树。

__注意:__
你可以假设树中没有重复的元素。

__示例 :__

例如，给出

中序遍历 inorder = [9,3,15,20,7]
后序遍历 postorder = [9,15,7,20,3]
返回如下的二叉树：

```text
    3
   / \
  9  20
    /  \
   15   7
```

__思路__:

递归法
后序遍历最后一个为根节点
找到后序遍历最后一个元素在中序遍历的下标, 将中序遍历列表分为 2个部分, 前半部分为左子树, 后半部分为右子树
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
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) 
    {
        if (inorder.empty()) return nullptr;
        TreeNode *root = new TreeNode(postorder.back());
        int index = distance(inorder.begin(), find(inorder.begin(), inorder.end(), root -> val));
        vector<int> a(inorder.begin(), inorder.begin() + index);
        vector<int> b(postorder.begin(), postorder.begin() + index);
        root -> left = buildTree(a, b);
        a.assign(inorder.begin() + index + 1, inorder.end());
        b.assign(postorder.begin() + index, postorder.end() - 1);
        root -> right = buildTree(a, b);
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
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if (inorder.length == 0) return null;
        TreeNode root = new TreeNode(postorder[postorder.length - 1]);
        int index = IntStream.range(0, inorder.length).filter(i -> inorder[i] == root.val).findFirst().orElse(-1);
        root.left = buildTree(Arrays.copyOfRange(inorder, 0, index), Arrays.copyOfRange(postorder, 0, index));
        root.right = buildTree(Arrays.copyOfRange(inorder, index + 1, inorder.length), Arrays.copyOfRange(postorder, index, postorder.length - 1));
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
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        if not postorder:
            return None
        root, index = TreeNode(postorder[-1]), inorder.index(postorder[-1])
        root.left = self.buildTree(inorder[:index], postorder[:index])
        root.right = self.buildTree(inorder[index + 1:], postorder[index:-1])
        return root
```
