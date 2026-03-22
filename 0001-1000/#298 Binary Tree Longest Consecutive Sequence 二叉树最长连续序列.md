# 298 Binary Tree Longest Consecutive Sequence 二叉树最长连续序列

__Description:__

Given the `root` of a binary tree, return _the length of the longest __consecutive sequence path__._

A __consecutive sequence path__ is a path where the values __increase by one__ along the path.

Note that the path can start at __any node__ in the tree, and you cannot go from a node to its parent in the path.

__Example:__

Example 1:

![298-1](https://assets.leetcode.com/uploads/2021/03/14/consec1-1-tree.jpg)

```text
Input: root = [1,null,3,2,4,null,null,null,5]
Output: 3
Explanation: Longest consecutive sequence path is 3-4-5, so return 3.
```

Example 2:

![298-2](https://assets.leetcode.com/uploads/2021/03/14/consec1-2-tree.jpg)

```text
Input: root = [2,null,3,2,null,1]
Output: 2
Explanation: Longest consecutive sequence path is 2-3, not 3-2-1, so return 2.
```

__Constraints:__

- The number of nodes in the tree is in the range `[1, 3 * 10 ^ 4]`.
- `-3 * 10 ^ 4 <= Node.val <= 3 * 10 ^ 4`

__题目描述:__

给你一棵指定的二叉树的根节点 `root` ，请你计算其中 __最长连续序列路径__ 的长度。

__最长连续序列路径__ 是依次递增 1 的路径。该路径，可以是从某个初始节点到树中任意节点，通过「父 - 子」关系连接而产生的任意路径。且必须从父节点到子节点，反过来是不可以的。

__示例:__

示例 1：

![298-3](https://assets.leetcode.com/uploads/2021/03/14/consec1-1-tree.jpg)

```text
输入：root = [1,null,3,2,4,null,null,null,5]
输出：3
解释：当中，最长连续序列是 3-4-5 ，所以返回结果为 3 。
```

示例 2：

![298-4](https://assets.leetcode.com/uploads/2021/03/14/consec1-2-tree.jpg)

```text
输入：root = [2,null,3,2,null,1]
输出：2
解释：当中，最长连续序列是 2-3 。注意，不是 3-2-1，所以返回 2 。
```

__提示：__

- 树中节点的数目在范围 `[1, 3 * 10 ^ 4]` 内
- `-3 * 10 ^ 4 <= Node.val <= 3 * 10 ^ 4`

__思路:__

```text
递归
使用 dfs 从根节点向下搜索
如果当前节点为空, 则返回当前的深度
否则, 递归搜索左右子树
如果当前节点的值等于父节点的值加一, 则深度加一, 否则深度重置为 1
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

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
    int longestConsecutive(TreeNode* root) 
    {
        return dfs(root, nullptr, 0);
    }
private:
    int dfs(TreeNode* p, TreeNode* parent, int level) 
    {
        return !p ? level : max(level, max(dfs(p -> left, p, (parent and p -> val == parent -> val + 1) ? level + 1 : 1), dfs(p -> right, p, (parent and p -> val == parent -> val + 1) ? level + 1 : 1)));
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
    public int longestConsecutive(TreeNode root) {
        return dfs(root, null, 0);
    }

    private int dfs(TreeNode p, TreeNode parent, int level) {
        return p == null ? level : Math.max(level, Math.max(dfs(p.left, p, (parent != null && p.val == parent.val + 1) ? level + 1 : 1), dfs(p.right, p, (parent != null && p.val == parent.val + 1) ? level + 1 : 1)));
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
    def longestConsecutive(self, root: Optional[TreeNode]) -> int:
        return f(root, None, 0) if (f := lambda p, parent, level : level if p is None else max(level, max(f(p.left, p, level + 1 if parent and p.val == parent.val + 1 else 1), f(p.right, p, level + 1 if parent and p.val == parent.val + 1 else 1)))) else 0
```
