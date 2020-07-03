__Description__:
Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

__Note:__
A leaf is a node with no children.

__Example:__
Example 1:

Input: [1,2,3]
```
    1
   / \
  2   3
```
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.

Example 2:

Input: [4,9,0,5,1]
```
    4
   / \
  9   0
 / \
5   1
```
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.

__题目描述__:
给定一个二叉树，它的每个结点都存放一个 0-9 的数字，每条从根到叶子节点的路径都代表一个数字。

例如，从根到叶子节点路径 1->2->3 代表数字 123。

计算从根到叶子节点生成的所有数字之和。

__说明：__
叶子节点是指没有子节点的节点。

__示例 :__
示例 1:

输入: [1,2,3]
```
    1
   / \
  2   3
```
输出: 25
解释:
从根到叶子节点路径 1->2 代表数字 12.
从根到叶子节点路径 1->3 代表数字 13.
因此，数字总和 = 12 + 13 = 25.

示例 2:

输入: [4,9,0,5,1]
```
    4
   / \
  9   0
 / \
5   1
```
输出: 1026
解释:
从根到叶子节点路径 4->9->5 代表数字 495.
从根到叶子节点路径 4->9->1 代表数字 491.
从根到叶子节点路径 4->0 代表数字 40.
因此，数字总和 = 495 + 491 + 40 = 1026.

__思路__:
参考[LeetCode #112 Path Sum 路径总和](https://www.jianshu.com/p/28393f816dab)
不过这里的路径需要每次递归到新的一层时✖️10
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
    int sumNumbers(TreeNode* root) 
    {
        return helper(root, 0);
    }
private:
    int helper(TreeNode* root, int path)
    {
        if (!root) return 0;
        int result = path * 10 + root -> val;
        if (!root -> left && !root -> right) return result;
        return helper(root -> left, result) + helper(root -> right, result);
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
    public int sumNumbers(TreeNode root) {
        return helper(root, 0);
    }
    
    private int helper(TreeNode root, int path) {
        if (root == null) return 0;
        int result = path * 10 + root.val;
        if (root.left == null && root.right == null) return result;
        return helper(root.left, result) + helper(root.right, result);
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
    def sumNumbers(self, root: TreeNode) -> int:
        f = lambda root: [] if not root else [root.val] if not root.left and not root.right else [str(root.val) + str(i) for i in f(root.left) + f(root.right)]
        return sum(map(int, f(root)))
```