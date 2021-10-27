# 889 Construct Binary Tree from Preorder and Postorder Traversal 根据前序和后序遍历构造二叉树

__Description__:
Given two integer arrays, preorder and postorder where preorder is the preorder traversal of a binary tree of distinct values and postorder is the postorder traversal of the same tree, reconstruct and return the binary tree.

If there exist multiple answers, you can return any of them.

__Example:__

Example 1:

![prepost](https://assets.leetcode.com/uploads/2021/07/24/lc-prepost.jpg)

Input: preorder = [1,2,4,5,3,6,7], postorder = [4,5,2,6,7,3,1]
Output: [1,2,3,4,5,6,7]

Example 2:

Input: preorder = [1], postorder = [1]
Output: [1]

__Constraints:__

1 <= preorder.length <= 30
1 <= preorder[i] <= preorder.length
All the values of preorder are unique.
postorder.length == preorder.length
1 <= postorder[i] <= postorder.length
All the values of postorder are unique.
It is guaranteed that preorder and postorder are the preorder traversal and postorder traversal of the same binary tree.

__题目描述__:
返回与给定的前序和后序遍历匹配的任何二叉树。

pre 和 post 遍历中的值是不同的正整数。

__示例 :__

输入：pre = [1,2,4,5,3,6,7], post = [4,5,2,6,7,3,1]
输出：[1,2,3,4,5,6,7]

__提示:__

1 <= pre.length == post.length <= 30
pre[] 和 post[] 都是 1, 2, ..., pre.length 的排列
每个输入保证至少有一个答案。如果有多个答案，可以返回其中一个。

__思路__:

递归
前序遍历为根左右, 后序遍历为左右根
所以前序遍历的第一个即为树的根结点
找到前序遍历的第二个结点在后序遍历数组中的位置即可分出左子树
递归解析右子树即可
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)

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
    TreeNode* constructFromPrePost(vector<int>& preorder, vector<int>& postorder) 
    {
        int cur = 0, count = 0;
        return dfs(preorder, postorder, cur, count);
    }
private:
    TreeNode* dfs(vector<int>& pre, vector<int>& post, int &cur, int &count)
    {
        TreeNode* root = new TreeNode(pre[cur++]);
        if (post[count] != root -> val) root -> left = dfs(pre, post, cur, count);
        if (post[count] != root -> val) root -> right = dfs(pre, post, cur, count);
        ++count;
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
    public TreeNode constructFromPrePost(int[] preorder, int[] postorder) {
        if (preorder.length == 0) return null;
        else if (preorder.length == 1) return new TreeNode(preorder[0]);
        TreeNode root = new TreeNode(preorder[0]);
        int i;
        for (i = 0; i < preorder.length; i++) if (preorder[i] == postorder[postorder.length - 2]) break;
        return new TreeNode(preorder[0], constructFromPrePost(Arrays.copyOfRange(preorder, 1, i), Arrays.copyOfRange(postorder, 0, i - 1)), constructFromPrePost(Arrays.copyOfRange(preorder, i, postorder.length), Arrays.copyOfRange(postorder, i - 1, postorder.length-1)));
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
    def constructFromPrePost(self, preorder: List[int], postorder: List[int]) -> TreeNode:
        return None if not preorder else TreeNode(preorder[0]) if len(preorder) == 1 else TreeNode(preorder[0], self.constructFromPrePost(preorder[1:(n := postorder.index(preorder[1])) + 2], postorder[:n + 1]), self.constructFromPrePost(preorder[n + 2:], postorder[n + 1:-1]))
```
