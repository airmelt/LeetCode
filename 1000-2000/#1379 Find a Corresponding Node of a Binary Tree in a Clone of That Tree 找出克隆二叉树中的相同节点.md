# 1379 Find a Corresponding Node of a Binary Tree in a Clone of That Tree 找出克隆二叉树中的相同节点

__Description:__

Given two binary trees original and cloned and given a reference to a node target in the original tree.

The cloned tree is a copy of the original tree.

Return a reference to the same node in the cloned tree.

Note that you are not allowed to change any of the two trees or the target node and the answer must be a reference to a node in the cloned tree.

__Example:__

Example 1:

![1379-1](https://assets.leetcode.com/uploads/2020/02/21/e1.png)

Input: tree = [7,4,3,null,null,6,19], target = 3
Output: 3
Explanation: In all examples the original and cloned trees are shown. The target node is a green node from the original tree. The answer is the yellow node from the cloned tree.

Example 2:

![1379-2](https://assets.leetcode.com/uploads/2020/02/21/e2.png)

Input: tree = [7], target =  7
Output: 7

Example 3:

![1379-3](https://assets.leetcode.com/uploads/2020/02/21/e3.png)

Input: tree = [8,null,6,null,5,null,4,null,3,null,2,null,1], target = 4
Output: 4

__Constraints:__

The number of nodes in the tree is in the range [1, 10^4].
The values of the nodes of the tree are unique.
target node is a node from the original tree and is not null.

__Follow up:__
Could you solve the problem if repeated values on the tree are allowed?

__题目描述:__

给你两棵二叉树，原始树 original 和克隆树 cloned，以及一个位于原始树 original 中的目标节点 target。

其中，克隆树 cloned 是原始树 original 的一个 副本 。

请找出在树 cloned 中，与 target 相同 的节点，并返回对该节点的引用（在 C/C++ 等有指针的语言中返回 节点指针，其他语言返回节点本身）。

__注意：__

你 不能 对两棵二叉树，以及 target 节点进行更改。只能 返回对克隆树 cloned 中已有的节点的引用。

__示例:__

示例 1:

![1379-4](https://assets.leetcode.com/uploads/2020/02/21/e1.png)

输入: tree = [7,4,3,null,null,6,19], target = 3
输出: 3
解释: 上图画出了树 original 和 cloned。target 节点在树 original 中，用绿色标记。答案是树 cloned 中的黄颜色的节点（其他示例类似）。

示例 2:

![1379-5](https://assets.leetcode.com/uploads/2020/02/21/e2.png)

输入: tree = [7], target =  7
输出: 7

示例 3:

![1379-6](https://assets.leetcode.com/uploads/2020/02/21/e3.png)

输入: tree = [8,null,6,null,5,null,4,null,3,null,2,null,1], target = 4
输出: 4

__提示：__

树中节点的数量范围为 [1, 10^4] 。
同一棵树中，没有值相同的节点。
target 节点是树 original 中的一个节点，并且不会是 null 。

__进阶：__

如果树中允许出现值相同的节点，将如何解答？

__思路:__

DFS
由于树中结点值均不相同
要么在左子树中, 要么在右子树中
找到结点刚好与 target 相等的或者找到空结点就返回 cloned 树即可
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码:__

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
    TreeNode* getTargetCopy(TreeNode* original, TreeNode* cloned, TreeNode* target) 
    {
        if (!original or original == target) return cloned;
        TreeNode* left = getTargetCopy(original -> left, cloned -> left, target);
        if (left) return left;
        return getTargetCopy(original -> right, cloned -> right, target);
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
    public final TreeNode getTargetCopy(final TreeNode original, final TreeNode cloned, final TreeNode target) {
        if (original == null || original == target) return cloned;
        TreeNode left = getTargetCopy(original.left, cloned.left, target);
        if (left != null) return left;
        return getTargetCopy(original.right, cloned.right, target);
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
    def getTargetCopy(self, original: TreeNode, cloned: TreeNode, target: TreeNode) -> TreeNode:
        if not original or original == target:
            return cloned
        if left := self.getTargetCopy(original.left, cloned.left, target):
            return left
        return self.getTargetCopy(original.right, cloned.right, target)
```
