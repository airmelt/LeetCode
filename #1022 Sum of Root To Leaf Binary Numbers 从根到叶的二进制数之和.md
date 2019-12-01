__Description__:
Given a binary tree, each node has value 0 or 1.  Each root-to-leaf path represents a binary number starting with the most significant bit.  For example, if the path is 0 -> 1 -> 1 -> 0 -> 1, then this could represent 01101 in binary, which is 13.

For all leaves in the tree, consider the numbers represented by the path from the root to that leaf.

Return the sum of these numbers.

__Example:__
Example 1:
![Binary Tree](https://assets.leetcode.com/uploads/2019/04/04/sum-of-root-to-leaf-binary-numbers.png)
Input: [1,0,1,0,1,0,1]
Output: 22
Explanation: (100) + (101) + (110) + (111) = 4 + 5 + 6 + 7 = 22
 
__Note:__

The number of nodes in the tree is between 1 and 1000.
node.val is 0 or 1.
The answer will not exceed 2^31 - 1.

__题目描述__:
给出一棵二叉树，其上每个结点的值都是 0 或 1 。每一条从根到叶的路径都代表一个从最高有效位开始的二进制数。例如，如果路径为 0 -> 1 -> 1 -> 0 -> 1，那么它表示二进制数 01101，也就是 13 。

对树上的每一片叶子，我们都要找出从根到该叶子的路径所表示的数字。

以 10^9 + 7 为模，返回这些数字之和。

__示例 :__
![二叉树](https://assets.leetcode.com/uploads/2019/04/04/sum-of-root-to-leaf-binary-numbers.png)
输入：[1,0,1,0,1,0,1]
输出：22
解释：(100) + (101) + (110) + (111) = 4 + 5 + 6 + 7 = 22
 
__提示：__

树中的结点数介于 1 和 1000 之间。
node.val 为 0 或 1 。

__思路__:
按照 10进制转化为 2进制的思路, 每一层将上一层的值乘 2(即 << 1), 然后再加上遍历的结点的值, 用递归的方法遍历完整棵树即可
时间复杂度O(n), 空间复杂度O(1)

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
    int sumRootToLeaf(TreeNode* root) 
    {
        return root ? helper(root, 0) : 0;
    }
private:
    int helper(TreeNode* root, int sum)
    {
        if (!root) return sum;
        sum <<= 1;
        sum += root -> val;
        if (!root -> left && !root -> right) return sum;
        int count = 0;
        if (root -> left) count += helper(root -> left, sum);
        if (root -> right) count += helper(root -> right, sum);
        return count;
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
    public int sumRootToLeaf(TreeNode root) {
        return root == null ? 0 : helper(root, 0);
    }
    
    private int helper(TreeNode root, int sum) {
        if (root == null) return sum;
        sum <<= 1;
        sum += root.val;
        if (root.left == null && root.right == null) return sum;
        int count = 0;
        if (root.left != null) count += helper(root.left, sum);
        if (root.right != null) count += helper(root.right, sum);
        return count;
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
    def sumRootToLeaf(self, root: TreeNode) -> int:
        if not root:
            return 0
        def helper(root: TreeNode, sum: int) -> int:
            if not root:
                return sum
            sum <<= 1
            sum += root.val
            if not root.left and not root.right:
                return sum
            c = 0
            if root.left:
                c += helper(root.left, sum)
            if root.right:
                c += helper(root.right, sum)
            return c
        return helper(root, 0)
```