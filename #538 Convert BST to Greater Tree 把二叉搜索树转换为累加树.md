# 538 Convert BST to Greater Tree 把二叉搜索树转换为累加树

__Description__:
Given a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

__Example:__

Input: The root of a Binary Search Tree like this:

```text
              5
            /   \
           2     13
```

Output: The root of a Greater Tree like this:

```text
             18
            /   \
          20     13
```

__题目描述__:
给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。

__示例 :__

例如：

输入: 二叉搜索树:

```text
              5
            /   \
           2     13
```

输出: 转换为累加树:

```text
             18
            /   \
          20     13
```

__思路__:

1. 先计算所有数字之和, 然后按照先序遍历给各个结点赋值
2. 按照右 -> 根 -> 左的顺序记录结点的值并累加
可以用递归和迭代解决
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
    TreeNode* convertBST(TreeNode* root) 
    {
        int pre = 0;
        order(root, pre);
        return root;
    }
private:
    void order(TreeNode* root, int &pre) 
    {
        if (!root) return;
        stack<TreeNode*> s;
        while (root or s.size()) 
        {
            if (root) 
            {
                s.push(root);
                root = root -> right;
            } 
            else 
            {
                root = s.top();
                s.pop();
                root -> val += pre;
                pre = root -> val;
                root = root -> left;
            }
        }
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
    private int pre = 0;
    public TreeNode convertBST(TreeNode root) {
        order(root);
        return root;
    }

    private void order(TreeNode root) {
        if (root == null) return;
        order(root.right);
        root.val += pre;
        pre = root.val;
        order(root.left);
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
    def convertBST(self, root: TreeNode) -> TreeNode:
        s, l, p, pre = 0, [], root, 0
        def summary(root: TreeNode) -> int:
            if not root:
                return 0
            return root.val + dfs(root.left) + dfs(root.right)
        s = summary(root)
        while p or l:
            if p:
                l.append(p)
                p = p.left
            else:
                p = l.pop()
                pre = p.val
                p.val = s
                s -= pre
                p = p.right
        return root
```
