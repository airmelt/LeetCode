__Description__:
Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

__Example:__
Given binary tree [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
return its zigzag level order traversal as:

[
  [3],
  [20,9],
  [15,7]
]

__题目描述__:
给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

__示例 :__
给定二叉树 [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
返回锯齿形层次遍历如下：

[
  [3],
  [20,9],
  [15,7]
]

__思路__:
直接层序遍历, 每两层加上一次翻转, 或者用层数判断之后再考虑加在表头还是表尾
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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) 
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
        if (depth & 1) result[depth].insert(result[depth].begin(), root -> val);
        else result[depth].push_back(root -> val);
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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        helper(root, result, 0);
        return result;
    }
    
    private void helper(TreeNode root, List<List<Integer>> result, int depth) {
        if (root == null) return;
        if (result.size() == depth) result.add(new ArrayList<Integer>());
        if ((depth & 1) == 1) result.get(depth).add(0, root.val);
        else result.get(depth).add(root.val);
        helper(root.left, result, depth + 1);
        helper(root.right, result, depth + 1);
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
    def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
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
        for i, item in enumerate(result):
            if (i & 1):
                item.reverse()
        return result
```