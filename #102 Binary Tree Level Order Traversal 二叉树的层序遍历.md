# 102 Binary Tree Level Order Traversal 二叉树的层序遍历

__Description__:
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

__Example:__

For example:
Given binary tree [3,9,20,null,null,15,7],

```text
    3
   / \
  9  20
    /  \
   15   7
```

return its level order traversal as:

[
  [3],
  [9,20],
  [15,7]
]

__题目描述__:
给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。

__示例 :__

二叉树：[3,9,20,null,null,15,7],

```text
    3
   / \
  9  20
    /  \
   15   7
```

返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]

__思路__:

1. 递归法
递归时加上深度, 同一个深度的加入一个列表
2. 迭代法
采用队列
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
    vector<vector<int>> levelOrder(TreeNode* root) 
    {
        vector<vector<int>> result;
        helper(root, 0, result);
        return result;
    }
private:
    void helper(TreeNode* root, int depth, vector<vector<int>> &result)
    {
        if (!root) return;
        if (depth >= result.size()) result.push_back(vector<int>{});
        result[depth].push_back(root -> val);
        helper(root -> left, depth + 1, result);
        helper(root -> right, depth + 1, result);
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
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        LinkedList<TreeNode> queue = new LinkedList<>();     
        queue.add(root);
        while (!queue.isEmpty()) {
            List<Integer> temp = new ArrayList<>();
            int len = queue.size();
            for (int i = 0; i < len; i++) {
                TreeNode p = queue.remove();
                if (p != null) {
                    temp.add(p.val);
                    queue.add(p.left);
                    queue.add(p.right);
                }
            }
            if (!temp.isEmpty()) {
                result.add(temp);
            }
        }       
        return result;
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
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        def helper(root: TreeNode, depth: int, result: List[List[int]]) -> None:
            if not root:
                return
            if depth >= len(result):
                result.append([])
            result[depth].append(root.val)
            helper(root.left, depth + 1, result)
            helper(root.right, depth + 1, result)
        result = []
        helper(root, 0, result)
        return result
```
