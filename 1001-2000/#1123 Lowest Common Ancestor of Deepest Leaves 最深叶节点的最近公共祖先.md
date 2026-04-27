# 1123 Lowest Common Ancestor of Deepest Leaves 最深叶节点的最近公共祖先

__Description__:
Given the root of a binary tree, return the lowest common ancestor of its deepest leaves.

Recall that:

The node of a binary tree is a leaf if and only if it has no children
The depth of the root of the tree is 0. if the depth of a node is d, the depth of each of its children is d + 1.
The lowest common ancestor of a set S of nodes, is the node A with the largest depth such that every node in S is in the subtree with root A.

__Example:__

Example 1:

![binary tree](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png)

Input: root = [3,5,1,6,2,0,8,null,null,7,4]
Output: [2,7,4]
Explanation: We return the node with value 2, colored in yellow in the diagram.
The nodes coloured in blue are the deepest leaf-nodes of the tree.
Note that nodes 6, 0, and 8 are also leaf nodes, but the depth of them is 2, but the depth of nodes 7 and 4 is 3.

Example 2:

Input: root = [1]
Output: [1]
Explanation: The root is the deepest node in the tree, and it's the lca of itself.

Example 3:

Input: root = [0,1,3,null,2]
Output: [2]
Explanation: The deepest leaf node in the tree is 2, the lca of one node is itself.

__Constraints:__

The number of nodes in the tree will be in the range [1, 1000].
0 <= Node.val <= 1000
The values of the nodes in the tree are unique.

__Note:__
This question is the same as [865](https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes/)

__题目描述__:
给你一个有根节点 root 的二叉树，返回它 最深的叶节点的最近公共祖先 。

回想一下：

叶节点 是二叉树中没有子节点的节点
树的根节点的 深度 为 0，如果某一节点的深度为 d，那它的子节点的深度就是 d+1
如果我们假定 A 是一组节点 S 的 最近公共祖先，S 中的每个节点都在以 A 为根节点的子树中，且 A 的深度达到此条件下可能的最大值。

__示例 :__

示例 1：

![二叉树](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png)

输入：root = [3,5,1,6,2,0,8,null,null,7,4]
输出：[2,7,4]
解释：我们返回值为 2 的节点，在图中用黄色标记。
在图中用蓝色标记的是树的最深的节点。
注意，节点 6、0 和 8 也是叶节点，但是它们的深度是 2 ，而节点 7 和 4 的深度是 3 。

示例 2：

输入：root = [1]
输出：[1]
解释：根节点是树中最深的节点，它是它本身的最近公共祖先。

示例 3：

输入：root = [0,1,3,null,2]
输出：[2]
解释：树中最深的叶节点是 2 ，最近公共祖先是它自己。

__提示:__

树中的节点数将在 [1, 1000] 的范围内。
0 <= Node.val <= 1000
每个节点的值都是 独一无二 的。

__注意：__
本题与力扣 [865](https://leetcode-cn.com/problems/smallest-subtree-with-all-the-deepest-nodes/) 重复

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
    TreeNode* lcaDeepestLeaves(TreeNode* root) 
    {
        if (!root) return root;
        int left = depth(root -> left), right = depth(root -> right);
        return left > right ? lcaDeepestLeaves(root -> left) : (left < right ? lcaDeepestLeaves(root -> right) : root);
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
    public TreeNode lcaDeepestLeaves(TreeNode root) {
        if (root == null) return root;
        int left = depth(root.left), right = depth(root.right);
        return left > right ? lcaDeepestLeaves(root.left) : (left < right ? lcaDeepestLeaves(root.right) : root);
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
    def lcaDeepestLeaves(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def depth(node: TreeNode) -> int:
            return 0 if not node else max(depth(node.left), depth(node.right)) + 1
        
        return root if not root or (l := depth(root.left)) ==  (r := depth(root.right)) else self.lcaDeepestLeaves(root.left) if l > r else self.lcaDeepestLeaves(root.right)
```
