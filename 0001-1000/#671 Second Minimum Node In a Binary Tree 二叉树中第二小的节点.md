# 671 Second Minimum Node In a Binary Tree 二叉树中第二小的节点

__Description__:
Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly two or zero sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes. More formally, the property root.val = min(root.left.val, root.right.val) always holds.

Given such a binary tree, you need to output the second minimum value in the set made of all the nodes' value in the whole tree.

If no such second minimum value exists, output -1 instead.

__Example:__

Example 1:

Input:

```text
    2
   / \
  2   5
     / \
    5   7
```

Output: 5
Explanation: The smallest value is 2, the second smallest value is 5.

Example 2:

Input:

```text
    2
   / \
  2   2
```

Output: -1
Explanation: The smallest value is 2, but there isn't any second smallest value.

__题目描述__:
给定一个非空特殊的二叉树，每个节点都是正数，并且每个节点的子节点数量只能为 2 或 0。如果一个节点有两个子节点的话，那么这个节点的值不大于它的子节点的值。

给出这样的一个二叉树，你需要输出所有节点中的第二小的值。如果第二小的值不存在的话，输出 -1 。

__示例 :__

示例 1:

输入:

```text
    2
   / \
  2   5
     / \
    5   7
```

输出: 5
说明: 最小的值是 2 ，第二小的值是 5 。

示例 2:

输入:

```text
    2
   / \
  2   2
```

输出: -1
说明: 最小的值是 2, 但是不存在第二小的值。

__思路__:

注意本题翻译的有点问题, 英文题目中有说明, root的值是等于左右节点的最小值
找到一个大于根结点的值即可

1. 迭代, 层序遍历
2. 递归
需要注意边界条件(2147483647)
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
    int findSecondMinimumValue(TreeNode* root) 
    {
        if (!root) return -1;
        queue<TreeNode*> q;
        int min1 = root -> val;
        unsigned int min2 = 0xffffffff;
        q.push(root);
        while (q.size()) 
        {
            TreeNode* cur = q.front();
            q.pop();
            if (cur -> val < min2 && cur -> val > min1) min2 = cur -> val;
            if (cur -> left) q.push(cur -> left);
            if (cur -> right) q.push(cur -> right);
        }
        return min2 == min1 ? -1 : min2;
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
    public int findSecondMinimumValue(TreeNode root) {
        return find(root, root.val);
    }
    
    private int find(TreeNode root, int temp) {
        if (root == null) return -1;
        if (root.val > temp) return root.val;
        int l = find(root.left, temp), r = find(root.right, temp);
        if (l > temp && r > temp) return Math.min(l, r);
        return Math.max(l, r);
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
    def findSecondMinimumValue(self, root: TreeNode) -> int:
        def find(root: TreeNode, temp: int) -> int:
            if not root:
                return -1
            if root.val > temp:
                return root.val
            l, r = find(root.left, temp), find(root.right, temp)
            if l > temp and r > temp:
                return min(l, r)
            return max(l, r)
        return find(root, root.val)
```
