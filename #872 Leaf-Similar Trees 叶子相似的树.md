# 872 Leaf-Similar Trees 叶子相似的树

__Description__:
Consider all the leaves of a binary tree.  From left to right order, the values of those leaves form a leaf value sequence.

![binary tree](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/16/tree.png)

For example, in the given tree above, the leaf value sequence is (6, 7, 4, 9, 8).

Two binary trees are considered leaf-similar if their leaf value sequence is the same.

Return true if and only if the two given trees with head nodes root1 and root2 are leaf-similar.

__Note:__

Both of the given trees will have between 1 and 100 nodes.

__题目描述__:
请考虑一颗二叉树上所有的叶子，这些叶子的值按从左到右的顺序排列形成一个 叶值序列 。

![二叉树](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/16/tree.png)

举个例子，如上图所示，给定一颗叶值序列为 (6, 7, 4, 9, 8) 的树。

如果有两颗二叉树的叶值序列是相同，那么我们就认为它们是 叶相似 的。

如果给定的两个头结点分别为 root1 和 root2 的树是叶相似的，则返回 true；否则返回 false 。

__提示：__

给定的两颗树可能会有 1 到 100 个结点。

__思路__:

1. 迭代, 用栈记录
2. 递归
找到所有叶子的值比较大小即可
时间复杂度O(n), 空间复杂度O(n), n是两棵树的最大结点数

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
    bool leafSimilar(TreeNode* root1, TreeNode* root2) 
    {
        return leaves(root1) == leaves(root2);
    }
private:
    vector<int> leaves(TreeNode* root) 
    {
        stack<TreeNode*> s;
        s.push(root);
        vector<int> result;
        while (s.size())
        {
            TreeNode* cur = s.top();
            s.pop();
            if (!cur -> left and !cur -> right) result.push_back(cur -> val);
            if (cur -> left) s.push(cur -> left);
            if (cur -> right) s.push(cur -> right);
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
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean leafSimilar(TreeNode root1, TreeNode root2) {
        return leaves(root1, "").equals(leaves(root2, ""));
    }
    
    private String leaves(TreeNode root, String result) {
        if (root == null) return result;
        if (root.left == null && root.right == null) return result + root.val;
        return leaves(root.left, result) + leaves(root.right, result);
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
    def leafSimilar(self, root1: TreeNode, root2: TreeNode) -> bool:
        def dfs(root: TreeNode) -> List[int]:
            s, result = [root], []
            while s:
                cur = s.pop()
                if not cur.left and not cur.right:
                    result.append(cur.val)
                if cur.left:
                    s.append(cur.left)
                if cur.right:
                    s.append(cur.right)
            return result
        return dfs(root1) == dfs(root2)
        # 官方题解
        # def dfs(node):
        #     if node:
        #         if not node.left and not node.right:
        #             yield node.val
        #         yield from dfs(node.left)
        #         yield from dfs(node.right)
        # return list(dfs(root1)) == list(dfs(root2))
```
