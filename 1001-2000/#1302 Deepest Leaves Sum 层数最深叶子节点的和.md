# 1302 Deepest Leaves Sum 层数最深叶子节点的和

__Description:__

Given the root of a binary tree, return the sum of values of its deepest leaves.

__Example:__

Example 1:

![binary tree](https://assets.leetcode.com/uploads/2019/07/31/1483_ex1.png)

Input: root = [1,2,3,4,5,null,6,7,null,null,null,null,8]
Output: 15

Example 2:

Input: root = [6,7,8,2,7,1,3,9,null,1,4,null,null,null,5]
Output: 19

__Constraints:__

The number of nodes in the tree is in the range [1, 10^4].
1 <= Node.val <= 100

__题目描述：__

给你一棵二叉树的根节点 root ，请你返回 层数最深的叶子节点的和 。

__示例：__

示例 1：

![二叉树](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/12/28/1483_ex1.png)

输入：root = [1,2,3,4,5,null,6,7,null,null,null,null,8]
输出：15

示例 2：

输入：root = [6,7,8,2,7,1,3,9,null,1,4,null,null,null,5]
输出：19

__提示：__

树中节点数目在范围 [1, 10^4] 之间。
1 <= Node.val <= 100

__思路：__

1. 迭代
使用队列层序遍历
每到新的一层, 重置累加值为 0
加上该层的所有节点的值, 直到最后一层
时间复杂度为 O(n), 空间复杂度为 O(n)
2. 递归
记录最大层数和结果
每次更新最大层时, 将该节点的值赋给结果
累计同一层的所有节点的值
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
    int deepestLeavesSum(TreeNode* root) 
    {
        int result = 0;
        queue<TreeNode*> q;
        if (root) q.push(root);
        while (!q.empty())
        {
            result = 0;
            for (int i = 0, s = q.size(); i < s; i++)
            {
                TreeNode* cur = q.front();
                q.pop();
                result += cur -> val;
                if (cur -> left) q.push(cur -> left);
                if (cur -> right) q.push(cur -> right);
            }
        }
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
    private int result = 0, maxLevel = 0;
    
    public int deepestLeavesSum(TreeNode root) {
        helper(root, 0);
        return result;
    }
    
    private void helper(TreeNode root, int level) {
        if (root == null) return;
        if (level > maxLevel) {
            maxLevel = level;
            result = root.val;
        } else if (level == maxLevel) result += root.val;
        helper(root.left, level + 1);
        helper(root.right, level + 1);
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
    def deepestLeavesSum(self, root: Optional[TreeNode]) -> int:
        result, queue = 0, [root] if root else []
        while queue:
            result, s = 0, len(queue)
            for _ in range(s):
                cur = queue.pop(0)
                result += cur.val
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
        return result
```
