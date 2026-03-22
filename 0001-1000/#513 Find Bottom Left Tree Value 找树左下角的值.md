# 513 Find Bottom Left Tree Value 找树左下角的值

__Description__:
Given the root of a binary tree, return the leftmost value in the last row of the tree.

__Example:__

Example 1:

![tree1](https://assets.leetcode.com/uploads/2020/12/14/tree1.jpg)

Input: root = [2,1,3]
Output: 1

Example 2:

![tree2](https://assets.leetcode.com/uploads/2020/12/14/tree2.jpg)

Input: root = [1,2,3,4,null,5,6,null,null,7]
Output: 7

__Constraints:__

The number of nodes in the tree is in the range [1, 104].
-2^31 <= Node.val <= 2^31 - 1

__题目描述__:
给定一个二叉树，在树的最后一行找到最左边的值。

__示例 :__

示例 1:

输入:

```text
    2
   / \
  1   3
```

输出:
1

示例 2:

输入:

```text
        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7
```

输出:
7

__注意:__
您可以假设树（即给定的根节点）不为 NULL。

__思路__:

层序遍历
用一个队列记录, 先加入右节点, 再加入左节点, 这样得到最终的节点就是左下角的节点
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
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution 
{
public:
    int findBottomLeftValue(TreeNode* root) 
    {
        queue<TreeNode*> q;
        q.push(root);
        TreeNode* result = root;
        while (!q.empty()) 
        {
            result = q.front();
            q.pop();
            if (result -> right) q.push(result -> right);
            if (result -> left) q.push(result -> left);
        }
        return result -> val;
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
    public int findBottomLeftValue(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        TreeNode result = root;
        while (!queue.isEmpty()) {
            result = queue.poll();
            if (result.right != null) queue.offer(result.right);
            if (result.left != null) queue.offer(result.left);
        }
        return result.val;
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
    def findBottomLeftValue(self, root: TreeNode) -> int:
        queue = [root]
        while queue:
            cur = queue.pop(0)
            (cur.right and queue.append(cur.right)) or (cur.left and queue.append(cur.left))
        return cur.val
```
