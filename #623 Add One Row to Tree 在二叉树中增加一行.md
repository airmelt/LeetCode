# 623 Add One Row to Tree 在二叉树中增加一行

__Description__:
Given the root of a binary tree and two integers val and depth, add a row of nodes with value val at the given depth depth.

Note that the root node is at depth 1.

The adding rule is:

Given the integer depth, for each not null tree node cur at the depth depth - 1, create two tree nodes with value val as cur's left subtree root and right subtree root.
cur's original left subtree should be the left subtree of the new left subtree root.
cur's original right subtree should be the right subtree of the new right subtree root.
If depth == 1 that means there is no depth depth - 1 at all, then create a tree node with value val as the new root of the whole original tree, and the original tree is the new root's left subtree.

__Example:__

Example 1:

![addrow-tree 1](https://assets.leetcode.com/uploads/2021/03/15/addrow-tree.jpg)

Input: root = [4,2,6,3,1,5], val = 1, depth = 2
Output: [4,1,1,2,null,null,6,3,1,5]

Example 2:

![addrow-tree 2](https://assets.leetcode.com/uploads/2021/03/11/add2-tree.jpg)

Input: root = [4,2,null,3,1], val = 1, depth = 3
Output: [4,2,null,1,1,3,null,null,1]

__Constraints:__

The number of nodes in the tree is in the range [1, 10^4].
The depth of the tree is in the range [1, 10^4].
-100 <= Node.val <= 100
-10^5 <= val <= 10^5
1 <= depth <= the depth of tree + 1

__题目描述__:
给定一个二叉树，根节点为第1层，深度为 1。在其第 d 层追加一行值为 v 的节点。

添加规则：给定一个深度值 d （正整数），针对深度为 d-1 层的每一非空节点 N，为 N 创建两个值为 v 的左子树和右子树。

将 N 原先的左子树，连接为新节点 v 的左子树；将 N 原先的右子树，连接为新节点 v 的右子树。

如果 d 的值为 1，深度 d - 1 不存在，则创建一个新的根节点 v，原先的整棵树将作为 v 的左子树。

__示例 :__

示例 1:

输入:
二叉树如下所示:

```text
       4
     /   \
    2     6
   / \   / 
  3   1 5   
```

v = 1

d = 2

输出:

```text
       4
      / \
     1   1
    /     \
   2       6
  / \     / 
 3   1   5   
```

示例 2:

输入:
二叉树如下所示:

```text
      4
     /   
    2    
   / \   
  3   1    
```

v = 1

d = 3

输出:

```text
      4
     /   
    2
   / \    
  1   1
 /     \  
3       1
```

__注意:__

输入的深度值 d 的范围是：[1，二叉树最大深度 + 1]。
输入的二叉树至少有一个节点。

__思路__:

1. 递归
分为左子树和右子树递归
2. BFS
按照层序遍历到 depth 深度之后给队列中的每一个节点加上 val 的节点
时间复杂度 O(n), 空间复杂度 O(n)

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
    TreeNode* addOneRow(TreeNode* root, int val, int depth) 
    {
        if (depth == 1) return new TreeNode(val, root, nullptr);
        queue<TreeNode*> q{{root}};
        while (--depth > 1)
        {
            int s = q.size();
            while (s--)
            {
                auto cur = q.front();
                q.pop();
                if (cur -> left) q.emplace(cur -> left);
                if (cur -> right) q.emplace(cur -> right);
            }
        }
        while (!q.empty())
        {
            auto item = q.front();
            item -> left = new TreeNode(val, item -> left, nullptr);
            item -> right = new TreeNode(val, nullptr, item -> right);
            q.pop();
        }
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
    public TreeNode addOneRow(TreeNode root, int v, int d) {
        if (d == 0 || d == 1) {
            TreeNode t = new TreeNode(v);
            if (d == 1) t.left = root;
            else t.right = root;
            return t;
        }
        if (root != null && d > 1) {
            root.left = addOneRow(root.left, v, d > 2 ? d - 1 : 1);
            root.right = addOneRow(root.right, v, d > 2 ? d - 1 : 0);
        }
        return root;
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
    def addOneRow(self, root: TreeNode, val: int, depth: int) -> TreeNode:
        if depth == 1:
            return TreeNode(val, root, None)
        queue = [root]
        while (depth := depth - 1) > 1:
            s = len(queue) + 1
            while (s := s - 1):
                cur = queue.pop(0)
                (cur.left and queue.append(cur.left)) or (cur.right and queue.append(cur.right))
        for item in queue:
            item.left, item.right = TreeNode(val, left=item.left), TreeNode(val, right=item.right)
        return root
```
