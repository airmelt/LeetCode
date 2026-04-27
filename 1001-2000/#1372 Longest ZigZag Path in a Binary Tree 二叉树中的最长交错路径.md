# 1372 Longest ZigZag Path in a Binary Tree 二叉树中的最长交错路径

__Description:__

You are given the root of a binary tree.

A ZigZag path for a binary tree is defined as follow:

Choose any node in the binary tree and a direction (right or left).
If the current direction is right, move to the right child of the current node; otherwise, move to the left child.
Change the direction from right to left or from left to right.
Repeat the second and third steps until you can't move in the tree.
Zigzag length is defined as the number of nodes visited - 1. (A single node has a length of 0).

Return the longest ZigZag path contained in that tree.

__Example:__

Example 1:

![二叉树 1](https://assets.leetcode.com/uploads/2020/01/22/sample_1_1702.png)

Input: root = [1,null,1,1,1,null,null,1,1,null,1,null,null,null,1,null,1]
Output: 3
Explanation: Longest ZigZag path in blue nodes (right -> left -> right).

Example 2:

![二叉树 2](https://assets.leetcode.com/uploads/2020/01/22/sample_2_1702.png)

Input: root = [1,1,1,null,1,null,null,1,1,null,1]
Output: 4
Explanation: Longest ZigZag path in blue nodes (left -> right -> left -> right).

Example 3:

Input: root = [1]
Output: 0

__Constraints:__

The number of nodes in the tree is in the range [1, 5 * 10^4].
1 <= Node.val <= 100

__题目描述：__
给你一棵以 root 为根的二叉树，二叉树中的交错路径定义如下：

选择二叉树中 任意 节点和一个方向（左或者右）。
如果前进方向为右，那么移动到当前节点的的右子节点，否则移动到它的左子节点。
改变前进方向：左变右或者右变左。
重复第二步和第三步，直到你在树中无法继续移动。
交错路径的长度定义为：访问过的节点数目 - 1（单个节点的路径长度为 0 ）。

请你返回给定树中最长 交错路径 的长度。

__示例：__

示例 1：

![Binary Tree 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/07/sample_1_1702.png)

输入：root = [1,null,1,1,1,null,null,1,1,null,1,null,null,null,1,null,1]
输出：3
解释：蓝色节点为树中最长交错路径（右 -> 左 -> 右）。

示例 2：

![Binary Tree 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/07/sample_2_1702.png)

输入：root = [1,1,1,null,1,null,null,1,1,null,1]
输出：4
解释：蓝色节点为树中最长交错路径（左 -> 右 -> 左 -> 右）。
示例 3：

输入：root = [1]
输出：0

__提示：__

每棵树最多有 50000 个节点。
每个节点的值在 [1, 100] 之间。

__思路：__

DFS
分别从每个结点开始向左和向右搜索, 用 left 和 right 记录向左和向右移动的次数
向左搜索时, left 设置为 right + 1, right 设置为 0
向右搜索时, right 设置为 left + 1, left 设置为 0
用一个全局变量记录最值
时间复杂度为 O(n), 空间复杂度为 O(1)

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
private:
    int result;
    
    void dfs(TreeNode* root, int left, int right) 
    {
        if (!root) return;
        result = max({result, left, right});
        dfs(root -> left, right + 1, 0);
        dfs(root -> right, 0, left + 1);
    }
public:
    int longestZigZag(TreeNode* root) 
    {
        result = 0;
        dfs(root, 0, 0);
        return result;
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
    private int result = 0;
    
    public int longestZigZag(TreeNode root) {
        dfs(root, 0, 0);
        return result;
    }
    
    private void dfs(TreeNode root, int left, int right) {
        if (root == null) return;
        result = Math.max(result, Math.max(left, right));
        dfs(root.left, right + 1, 0);
        dfs(root.right, 0, left + 1);
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
    def longestZigZag(self, root: Optional[TreeNode]) -> int:
        self.result = 0
        
        def dfs(cur: Optional[TreeNode], left: int, right: int) -> None:
            if cur is None:
                return
            self.result = max(self.result, left, right)
            dfs(cur.left, right + 1, 0)
            dfs(cur.right, 0, left + 1)
        dfs(root, 0, 0)
        return self.result
```
