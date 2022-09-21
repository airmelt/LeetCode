# 1315 Sum of Nodes with Even-Valued Grandparent 祖父节点值为偶数的节点和

__Description:__

Given the root of a binary tree, return the sum of values of nodes with an even-valued grandparent. If there are no nodes with an even-valued grandparent, return 0.

A grandparent of a node is the parent of its parent if it exists.

__Example:__

Example 1:

![Binary Tree 1](https://assets.leetcode.com/uploads/2021/08/10/even1-tree.jpg)

Input: root = [6,7,8,2,7,1,3,9,null,1,4,null,null,null,5]
Output: 18
Explanation: The red nodes are the nodes with even-value grandparent while the blue nodes are the even-value grandparents.

Example 2:

![Binary Tree 2](https://assets.leetcode.com/uploads/2021/08/10/even2-tree.jpg)

Input: root = [1]
Output: 0

__Constraints:__

The number of nodes in the tree is in the range [1, 10^4].
1 <= Node.val <= 100

__题目描述：__

给你一棵二叉树，请你返回满足以下条件的所有节点的值之和：

该节点的祖父节点的值为偶数。（一个节点的祖父节点是指该节点的父节点的父节点。）
如果不存在祖父节点值为偶数的节点，那么返回 0 。

__示例：__

![二叉树](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/10/1473_ex1.png)

输入：root = [6,7,8,2,7,1,3,9,null,1,4,null,null,null,5]
输出：18
解释：图中红色节点的祖父节点的值为偶数，蓝色节点为这些红色节点的祖父节点。

__提示：__

树中节点的数目在 1 到 10^4 之间。
每个节点的值在 1 到 100 之间。

__思路：__

1. DFS
每次遍历到孙子节点, 如果其祖父节点为偶数, 将结果累计
时间复杂度为 O(n), 空间复杂度为 O(h), 其中 h 为二叉树的高度
2. BFS
每次遍历到偶数值的节点, 加入其所有孙子节点
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
    int sumEvenGrandparent(TreeNode* root) 
    {
        return dfs(root, -1, -1);
    }
private:
    int dfs(TreeNode* cur, int parent, int grand)
    {
        int result = 0;
        if (!cur) return result;
        if (!(grand & 1)) result += cur -> val;
        result += dfs(cur -> left, cur -> val, parent) + dfs(cur -> right, cur -> val, parent);
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
    public int sumEvenGrandparent(TreeNode root) {
        int result = 0;
        List<TreeNode> list = new ArrayList<>();
        list.add(root);
        for (int i = 0; i < list.size(); i++) {
            TreeNode cur = list.get(i);
            if ((cur.val & 1) == 0) result += (cur.left != null ? (cur.left.left != null ? cur.left.left.val : 0) : 0) + (cur.left != null ? (cur.left.right != null ? cur.left.right.val : 0) : 0) + (cur.right != null ? (cur.right.left != null ? cur.right.left.val : 0) : 0) + (cur.right != null ? (cur.right.right != null ? cur.right.right.val : 0) : 0);
            if (cur.left != null) list.add(cur.left);
            if (cur.right != null) list.add(cur.right);
        }
        return result;
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
    def sumEvenGrandparent(self, root: TreeNode) -> int:
        def dfs(cur: TreeNode, parent: int, grand: int) -> int:
            result = 0
            if not cur:
                return result
            if not (grand & 1):
                result += cur.val
            result += dfs(cur.left, cur.val, parent) + dfs(cur.right, cur.val, parent)
            return result
        return dfs(root, -1, -1)
```
