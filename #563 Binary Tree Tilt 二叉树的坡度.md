__Description__:
Given a binary tree, return the tilt of the whole tree.

The tilt of a tree node is defined as the absolute difference between the sum of all left subtree node values and the sum of all right subtree node values. Null node has tilt 0.

The tilt of the whole tree is defined as the sum of all nodes' tilt.

__Example:__
Input:
```
         1
       /   \
      2     3
```
Output: 1
Explanation:
Tilt of node 2 : 0
Tilt of node 3 : 0
Tilt of node 1 : |2-3| = 1
Tilt of binary tree : 0 + 0 + 1 = 1

__Note:__

The sum of node values in any subtree won't exceed the range of 32-bit integer.
All the tilt values won't exceed the range of 32-bit integer.

__题目描述__:
给定一个二叉树，计算整个树的坡度。

一个树的节点的坡度定义即为，该节点左子树的结点之和和右子树结点之和的差的绝对值。空结点的的坡度是0。

整个树的坡度就是其所有节点的坡度之和。

__示例 :__

输入:
```
         1
       /   \
      2     3
```
输出: 1
解释:
结点的坡度 2 : 0
结点的坡度 3 : 0
结点的坡度 1 : |2-3| = 1
树的坡度 : 0 + 0 + 1 = 1

__注意:__

任何子树的结点的和不会超过32位整数的范围。
坡度的值不会超过32位整数的范围。

__思路__:
递归, 遍历每个结点求出结点的坡度
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```
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
    int findTilt(TreeNode* root) {
        traverse(root);
        return result;
    }
private:
    int result = 0;
    int traverse(TreeNode* root) {
        if (!root) return 0;
        int left = traverse(root -> left);
        int right = traverse(root -> right);
        result += abs(left - right);
        return left + right + root -> val;
    }
};
```

__Java__:
```
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

    public int findTilt(TreeNode root) {
        if (root == null) return 0;
        return Math.abs(total(root.left) - total(root.right)) + findTilt(root.left) + findTilt(root.right);
    }

    private int total(TreeNode root) {
        if (root == null) return 0;
        return total(root.left) + total(root.right) + root.val;
    }
}
```

__Python__:
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def findTilt(self, root: TreeNode) -> int:
        result = 0
        def traverse(root: TreeNode) -> int:
            if not root:
                return 0
            nonlocal result
            left = traverse(root.left)
            right = traverse(root.right)
            result += abs(left - right)
            return left + right + root.val
        traverse(root)
        return result
```
