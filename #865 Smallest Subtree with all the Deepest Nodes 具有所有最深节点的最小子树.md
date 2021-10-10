# 865 Smallest Subtree with all the Deepest Nodes 具有所有最深节点的最小子树

__Description__:
Given the root of a binary tree, the depth of each node is the shortest distance to the root.

Return the smallest subtree such that it contains all the deepest nodes in the original tree.

A node is called the deepest if it has the largest depth possible among any node in the entire tree.

The subtree of a node is a tree consisting of that node, plus the set of all descendants of that node.

__Example:__

Example 1:

![binary tree](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png)

Input: root = [3,5,1,6,2,0,8,null,null,7,4]
Output: [2,7,4]
Explanation: We return the node with value 2, colored in yellow in the diagram.
The nodes coloured in blue are the deepest nodes of the tree.
Notice that nodes 5, 3 and 2 contain the deepest nodes in the tree but node 2 is the smallest subtree among them, so we return it.

Example 2:

Input: root = [1]
Output: [1]
Explanation: The root is the deepest node in the tree.

Example 3:

Input: root = [0,1,3,null,2]
Output: [2]
Explanation: The deepest node in the tree is 2, the valid subtrees are the subtrees of nodes 2, 1 and 0 but the subtree of node 2 is the smallest.

__Constraints:__

The number of nodes in the tree will be in the range [1, 500].
0 <= Node.val <= 500
The values of the nodes in the tree are unique.

__Note:__
This question is the same as [1123](https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/)

__题目描述__:
给定一个根为 root 的二叉树，每个节点的深度是 该节点到根的最短距离 。

如果一个节点在 整个树 的任意节点之间具有最大的深度，则该节点是 最深的 。

一个节点的 子树 是该节点加上它的所有后代的集合。

返回能满足 以该节点为根的子树中包含所有最深的节点 这一条件的具有最大深度的节点。

__注意：__
本题与力扣 [1123](https://leetcode-cn.com/problems/lowest-common-ancestor-of-deepest-leaves/) 重复：

__示例 :__

示例 1：

![二叉树](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png)

输入：root = [3,5,1,6,2,0,8,null,null,7,4]
输出：[2,7,4]
解释：
我们返回值为 2 的节点，在图中用黄色标记。
在图中用蓝色标记的是树的最深的节点。
注意，节点 5、3 和 2 包含树中最深的节点，但节点 2 的子树最小，因此我们返回它。

示例 2：

输入：root = [1]
输出：[1]
解释：根节点是树中最深的节点。

示例 3：

输入：root = [0,1,3,null,2]
输出：[2]
解释：树中最深的节点为 2 ，有效子树为节点 2、1 和 0 的子树，但节点 2 的子树最小。

__提示:__

树中节点的数量介于 1 和 500 之间。
0 <= Node.val <= 500
每个节点的值都是独一无二的。

__思路__:

DFS
这题目也太绕了, 题意是找到一棵树, 这棵树上所有的结点或者子结点包含深度最深的叶子结点
其实就是找到左子树和右子树高度相等的根结点
对每个结点查找高度即可
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
    TreeNode* subtreeWithAllDeepest(TreeNode* root) 
    {
        if (!root) return root;
        int left = depth(root -> left), right = depth(root -> right);
        return left > right ? subtreeWithAllDeepest(root -> left) : (left < right ? subtreeWithAllDeepest(root -> right) : root);
    }
private:
    int depth(TreeNode* root) 
    {
        return !root ? 0 : max(depth(root -> left), depth(root -> right)) + 1;
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
    public TreeNode subtreeWithAllDeepest(TreeNode root) {
        if (root == null) return root;
        int left = depth(root.left), right = depth(root.right);
        return left > right ? subtreeWithAllDeepest(root.left) : (left < right ? subtreeWithAllDeepest(root.right) : root);
    }
    
    private int depth(TreeNode root) {
        return root == null ? 0 : Math.max(depth(root.left), depth(root.right)) + 1;
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
    def subtreeWithAllDeepest(self, root: TreeNode) -> TreeNode:
        def depth(node: TreeNode) -> int:
            return 0 if not node else max(depth(node.left), depth(node.right)) + 1
        
        return root if not root or (l := depth(root.left)) ==  (r := depth(root.right)) else self.subtreeWithAllDeepest(root.left) if l > r else self.subtreeWithAllDeepest(root.right)
```
