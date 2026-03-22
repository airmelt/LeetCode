# 2265 Count Nodes Equal to Average of Subtree 统计值等于子树平均值的节点数

__Description:__

Given the `root` of a binary tree, return _the number of nodes where the value of the node is equal to the __average__ of the values in its __subtree___.

Note:

- The __average__ of `n` elements is the __sum__ of the `n` elements divided by `n` and __rounded down__ to the nearest integer.
- A __subtree__ of `root` is a tree consisting of `root` and all of its descendants.

__Example:__

Example 1:

![2265-1](https://assets.leetcode.com/uploads/2022/03/15/image-20220315203925-1.png)

```text
Input: root = [4,8,5,0,1,null,6]
Output: 5
Explanation: 
For the node with value 4: The average of its subtree is (4 + 8 + 5 + 0 + 1 + 6) / 6 = 24 / 6 = 4.
For the node with value 5: The average of its subtree is (5 + 6) / 2 = 11 / 2 = 5.
For the node with value 0: The average of its subtree is 0 / 1 = 0.
For the node with value 1: The average of its subtree is 1 / 1 = 1.
For the node with value 6: The average of its subtree is 6 / 1 = 6.
```

Example 2:

![2265-2](https://assets.leetcode.com/uploads/2022/03/26/image-20220326133920-1.png)

```text
Input: root = [1]
Output: 1
Explanation: For the node with value 1: The average of its subtree is 1 / 1 = 1.
```

__Constraints:__

- The number of nodes in the tree is in the range `[1, 1000]`.
- `0 <= Node.val <= 1000`

__题目描述:__

给你一棵二叉树的根节点 `root` ，找出并返回满足要求的节点数，要求节点的值等于其 __子树__ 中值的 __平均值__ 。

注意：

- `n` 个元素的平均值可以由 `n` 个元素 __求和__ 然后再除以 `n` ，并 __向下舍入__ 到最近的整数。
- `root` 的 __子树__ 由 `root` 和它的所有后代组成。

__示例:__

示例 1：

![2265-3](https://assets.leetcode.com/uploads/2022/03/15/image-20220315203925-1.png)

```text
输入：root = [4,8,5,0,1,null,6]
输出：5
解释：
对值为 4 的节点：子树的平均值 (4 + 8 + 5 + 0 + 1 + 6) / 6 = 24 / 6 = 4 。
对值为 5 的节点：子树的平均值 (5 + 6) / 2 = 11 / 2 = 5 。
对值为 0 的节点：子树的平均值 0 / 1 = 0 。
对值为 1 的节点：子树的平均值 1 / 1 = 1 。
对值为 6 的节点：子树的平均值 6 / 1 = 6 。
```

示例 2：

![2265-4](https://assets.leetcode.com/uploads/2022/03/26/image-20220326133920-1.png)

```text
输入：root = [1]
输出：1
解释：对值为 1 的节点：子树的平均值 1 / 1 = 1。
```

__提示：__

- 树中节点数目在范围 `[1, 1000]` 内
- `0 <= Node.val <= 1000`

__思路:__

```text
DFS
在 DFS 的同时统计子树的和以及节点数目，如果子树的和除以节点数目等于当前节点的值，则结果加一
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

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
    int averageOfSubtree(TreeNode* root)
    {
        dfs(root);
        return result;
    }
private:
    int result = 0;

    pair<int, int> dfs(TreeNode* node) 
    {
        if (!node) return make_pair(0, 0);
        auto l = dfs(node -> left), r = dfs(node -> right);
        int s = l.first + r.first + node -> val, n = l.second + r.second + 1;
        if (s / n == node -> val) ++result;
        return make_pair(s, n);
    }
};
```

__Java__:

```Java
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
    int averageOfSubtree(TreeNode* root)
    {
        dfs(root);
        return result;
    }
private:
    int result = 0;

    pair<int, int> dfs(TreeNode* node) 
    {
        if (!node) return make_pair(0, 0);
        auto l = dfs(node -> left), r = dfs(node -> right);
        int s = l.first + r.first + node -> val, n = l.second + r.second + 1;
        if (s / n == node -> val) ++result;
        return make_pair(s, n);
    }
};
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
    def averageOfSubtree(self, root: TreeNode) -> int:
        result = 0
        def dfs(node: TreeNode) -> Tuple[int, int]:
            nonlocal result
            if node is None:
                return (0, 0)
            l, r = dfs(node.left), dfs(node.right)
            result += (s := l[0] + r[0] + node.val) // (n := l[1] + r[1] + 1) == node.val
            return [s, n]
        dfs(root)
        return result              
```
