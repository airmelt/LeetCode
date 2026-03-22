# 333 Largest BST Subtree 最大BST子树

__Description:__

Given the root of a binary tree, find the largest subtree, which is also a Binary Search Tree (BST), where the largest means subtree has the largest number of nodes.

A Binary Search Tree (BST) is a tree in which all the nodes follow the below-mentioned properties:

- The left subtree values are less than the value of their parent (root) node's value.
- The right subtree values are greater than the value of their parent (root) node's value.

Note: A subtree must include all of its descendants.

__Example:__

Example 1:

![333-1](https://assets.leetcode.com/uploads/2020/10/17/tmp.jpg)

```text
Input: root = [10,5,15,1,8,null,7]
Output: 3
Explanation: The Largest BST Subtree in this case is the highlighted one. The return value is the subtree's size, which is 3.
```

Example 2:

```text
Input: root = [4,2,7,2,3,5,null,2,null,null,null,null,null,1]
Output: 2
```

__Constraints:__

- The number of nodes in the tree is in the range `[0, 10 ^ 4]`.
- `-10 ^ 4 <= Node.val <= 10 ^ 4`

__Follow up:__ Can you figure out ways to solve it with `O(n)` time complexity?

__题目描述:__

给定一个二叉树，找到其中最大的二叉搜索树（BST）子树，并返回该子树的大小。其中，最大指的是子树节点数最多的。

二叉搜索树（BST）中的所有节点都具备以下属性：

- 左子树的值小于其父（根）节点的值。
- 右子树的值大于其父（根）节点的值。

左子树的值小于其父（根）节点的值。

右子树的值大于其父（根）节点的值。

注意：子树必须包含其所有后代。

__示例:__

示例 1：

![333-2](https://assets.leetcode.com/uploads/2020/10/17/tmp.jpg)

```text
输入：root = [10,5,15,1,8,null,7]
输出：3
解释：本例中最大的 BST 子树是高亮显示的子树。返回值是子树的大小，即 3 。
```

示例 2：

```text
输入：root = [4,2,7,2,3,5,null,2,null,null,null,null,null,1]
输出：2
```

__提示：__

- 树上节点数目的范围是 `[0, 10 ^ 4]`
- `-10 ^ 4 <= Node.val <= 10 ^ 4`

进阶:  你能想出 O(n) 时间复杂度的解法吗？

__思路:__

```text
1. 两次 DFS
对每个结点进行递归
检查是否为 BST
如果为 BST, 求出结点数
时间复杂度为 O(N ^ 2), 空间复杂度为 O(logN)
2. 一次 DFS
对每个结点进行递归的同时记录下子树上的最大值和最小值以及结点个数
时间复杂度为 O(N), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    bool check(TreeNode* root, int left, int right)
    {
        return !root or (left < root -> val and right > root -> val and check(root -> left, left, root -> val) and check(root -> right, root -> val, right));
    }
    
    int children(TreeNode* root)
    {
        return !root ? 0 : 1 + children(root -> left) + children(root -> right);
    }
public:
    int largestBSTSubtree(TreeNode* root) 
    {
        return !root ? 0 : (check(root, INT_MIN, INT_MAX) ? children(root) : max(largestBSTSubtree(root -> left), largestBSTSubtree(root -> right)));
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
    public int largestBSTSubtree(TreeNode root) {
        return root == null ? 0 : (check(root, Integer.MIN_VALUE, Integer.MAX_VALUE) ? children(root) : Math.max(largestBSTSubtree(root.left), largestBSTSubtree(root.right)));
    }
    
    private boolean check(TreeNode root, int left, int right) {
        return root == null || (left < root.val && right > root.val && check(root.left, left, root.val) && check(root.right, root.val, right));
    }
    
    private int children(TreeNode root) {
        return root == null ? 0 : 1 + children(root.left) + children(root.right);
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
    def largestBSTSubtree(self, root: Optional[TreeNode]) -> int:
        def dfs(cur: Optional[TreeNode]):
            if not cur:
                return float('inf'), float('-inf'), 0
            left, right = dfs(cur.left), dfs(cur.right)
            if left[1] < cur.val and cur.val < right[0]:
                nonlocal result
                result = max(result, count := left[2] + right[2] + 1)
                return min(cur.val, left[0]), max(cur.val, right[1]), count
            return float('-inf'), float('inf'), 0
        result = 0
        dfs(root)
        return result
```
