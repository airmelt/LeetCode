# 113 Path Sum II 路径总和 II

__Description__:
Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

__Note:__
 A leaf is a node with no children.

__Example:__

Given the below binary tree and sum = 22,

```text
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \    / \
7    2  5   1
```

Return:

[
   [5,4,11,2],
   [5,8,4,5]
]

__题目描述__:
给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。

__说明:__
 叶子节点是指没有子节点的节点。

__示例 :__

给定如下二叉树，以及目标和 sum = 22，

```text
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```

返回:

[
   [5,4,11,2],
   [5,8,4,5]
]

__思路__:

回溯法
参考[LeetCode #112 Path Sum 路径总和](https://www.jianshu.com/p/28393f816dab)
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
    vector<vector<int>> pathSum(TreeNode* root, int sum) 
    {
        vector<vector<int>> result;
        vector<int> temp;
        helper(result, root, sum, temp);
        return result;
    }
private:
    void helper(vector<vector<int>> &result, TreeNode *root, int target, vector<int> &temp) 
    {
        if (!root) return;
        temp.push_back(root -> val);
        if (root -> val == target and !root -> left and !root -> right) result.push_back(temp);
        helper(result, root -> left, target - root -> val, temp);
        helper(result, root -> right, target - root -> val, temp);
        temp.pop_back();
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
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        helper(result, root, sum, temp);
        return result;
    }
    
    private void helper(List<List<Integer>> result, TreeNode root, int target, List<Integer> temp) {
        if (root == null) return;
        temp.add(root.val);
        if (root.val == target && root.left == null && root.right == null) result.add(new ArrayList<>(temp));
        helper(result, root.left, target - root.val, temp);
        helper(result, root.right, target - root.val, temp);
        temp.remove(temp.size() - 1);
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
    def pathSum(self, root: TreeNode, sum: int) -> List[List[int]]:
        def helper(root: TreeNode, target: int, path: List[int]) -> List[List[int]]:
            if not root:
                return []
            path.append(root.val)
            if not root.left and not root.right:
                result = path[:]
                path.pop()
                if root.val == target:
                    return [result]
                return []
            target -= root.val
            result = helper(root.left, target, path) + helper(root.right, target, path)
            path.pop()
            return result
        return helper(root, sum, [])
```
