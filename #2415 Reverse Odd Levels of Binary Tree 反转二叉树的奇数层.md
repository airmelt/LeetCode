# 2415 Reverse Odd Levels of Binary Tree 反转二叉树的奇数层

__Description:__

Given the `root` of a __perfect__ binary tree, reverse the node values at each __odd__ level of the tree.

- For example, suppose the node values at level 3 are `[2,1,3,4,7,11,29,18]`, then it should become `[18,29,11,7,4,3,1,2]`.

Return the root of the reversed tree.

A binary tree is perfect if all parent nodes have two children and all leaves are on the same level.

The level of a node is the number of edges along the path between it and the root node.

__Example:__

Example 1:

![2415-1](https://assets.leetcode.com/uploads/2022/07/28/first_case1.png)

```text
Input: root = [2,3,5,8,13,21,34]
Output: [2,5,3,8,13,21,34]
Explanation: 
The tree has only one odd level.
The nodes at level 1 are 3, 5 respectively, which are reversed and become 5, 3.
```

Example 2:

![2415-2](https://assets.leetcode.com/uploads/2022/07/28/second_case3.png)

```text
Input: root = [7,13,11]
Output: [7,11,13]
Explanation: 
The nodes at level 1 are 13, 11, which are reversed and become 11, 13.
```

Example 3:

```text
Input: root = [0,1,2,0,0,0,0,1,1,1,1,2,2,2,2]
Output: [0,2,1,0,0,0,0,2,2,2,2,1,1,1,1]
Explanation: 
The odd levels have non-zero values.
The nodes at level 1 were 1, 2, and are 2, 1 after the reversal.
The nodes at level 3 were 1, 1, 1, 1, 2, 2, 2, 2, and are 2, 2, 2, 2, 1, 1, 1, 1 after the reversal.
```

__Constraints:__

- The number of nodes in the tree is in the range `[1, 2 ^ 14]`.
- `0 <= Node.val <= 10 ^ 5`
- `root` is a __perfect__ binary tree.

__题目描述:__

给你一棵 __完美__ 二叉树的根节点 `root` ，请你反转这棵树中每个 __奇数__ 层的节点值。

- 例如，假设第 3 层的节点值是 `[2,1,3,4,7,11,29,18]` ，那么反转后它应该变成 `[18,29,11,7,4,3,1,2]` 。

反转后，返回树的根节点。

完美 二叉树需满足：二叉树的所有父节点都有两个子节点，且所有叶子节点都在同一层。

节点的 层数 等于该节点到根节点之间的边数。

__示例:__

示例 1：

![2415-3](https://assets.leetcode.com/uploads/2022/07/28/first_case1.png)

```text
输入：root = [2,3,5,8,13,21,34]
输出：[2,5,3,8,13,21,34]
解释：
这棵树只有一个奇数层。
在第 1 层的节点分别是 3、5 ，反转后为 5、3 。
```

示例 2：

![2415-4](https://assets.leetcode.com/uploads/2022/07/28/second_case3.png)

```text
输入：root = [7,13,11]
输出：[7,11,13]
解释： 
在第 1 层的节点分别是 13、11 ，反转后为 11、13 。
```

示例 3：

```text
输入：root = [0,1,2,0,0,0,0,1,1,1,1,2,2,2,2]
输出：[0,2,1,0,0,0,0,2,2,2,2,1,1,1,1]
解释：奇数层由非零值组成。
在第 1 层的节点分别是 1、2 ，反转后为 2、1 。
在第 3 层的节点分别是 1、1、1、1、2、2、2、2 ，反转后为 2、2、2、2、1、1、1、1 。
```

__提示：__

- 树中的节点数目在范围 `[1, 2 ^ 14]` 内
- `0 <= Node.val <= 10 ^ 5`
- `root` 是一棵 __完美__ 二叉树

__思路:__

```text
模拟
记录当前层数
如果当前层为奇数层
则将当前遍历的节点值进行交换
递归遍历左右子树
由于是完美二叉树, 所以左右子树的节点数相同, 可以递归遍历
时间复杂度为 O(N), 空间复杂度为 O(logN)
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
    TreeNode* reverseOddLevels(TreeNode* root) 
    {
        function<void(TreeNode*, TreeNode*, int)> dfs = [&](TreeNode* root1, TreeNode* root2, int level) -> void
        {
            if (!root1 or !root2) return;
            if (!(level & 1)) 
            {
                root1 -> val ^= root2 -> val;
                root2 -> val ^= root1 -> val;
                root1 -> val ^= root2 -> val;
            }
            dfs(root1 -> left, root2 -> right, level + 1);
            dfs(root1 -> right, root2 -> left, level + 1);
        };
        dfs(root -> left, root -> right, 0);
        return root;
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
    public TreeNode reverseOddLevels(TreeNode root) {
        dfs(root.left, root.right, 0);
        return root;
    }

    private void dfs(TreeNode root1, TreeNode root2, int level) {
        if (root1 == null || root2 == null) return;
        if ((level & 1) == 0) {
            root1.val ^= root2.val;
            root2.val ^= root1.val;
            root1.val ^= root2.val;
        }
        dfs(root1.left, root2.right, level + 1);
        dfs(root1.right, root2.left, level + 1);
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
    def reverseOddLevels(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def dfs(root1: Optional[TreeNode], root2: Optional[TreeNode], level: int) -> NoReturn:
            if root1 is None or root2 is None:
                return
            if not (level & 1):
                root1.val, root2.val = root2.val, root1.val
            dfs(root1.left, root2.right, level + 1)
            dfs(root1.right, root2.left, level + 1)
        dfs(root.left, root.right, 0)
        return root
```
