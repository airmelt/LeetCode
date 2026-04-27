# 2641 Cousins in Binary Tree II 二叉树的堂兄弟节点 II

__Description:__

Given the `root` of a binary tree, replace the value of each node in the tree with the __sum of all its cousins' values__.

Two nodes of a binary tree are cousins if they have the same depth with different parents.

Return _the_ `root` _of the modified tree_.

Note that the depth of a node is the number of edges in the path from the root node to it.

__Example:__

Example 1:

![2641-1](https://assets.leetcode.com/uploads/2023/01/11/example11.png)

```text
Input: root = [5,4,9,1,10,null,7]
Output: [0,0,0,7,7,null,11]
Explanation: The diagram above shows the initial binary tree and the binary tree after changing the value of each node.
```

- Node with value 5 does not have any cousins so its sum is 0.
- Node with value 4 does not have any cousins so its sum is 0.
- Node with value 9 does not have any cousins so its sum is 0.
- Node with value 1 has a cousin with value 7 so its sum is 7.
- Node with value 10 has a cousin with value 7 so its sum is 7.
- Node with value 7 has cousins with values 1 and 10 so its sum is 11.

Example 2:

![2641-2](https://assets.leetcode.com/uploads/2023/01/11/diagram33.png)

```text
Input: root = [3,1,2]
Output: [0,0,0]
Explanation: The diagram above shows the initial binary tree and the binary tree after changing the value of each node.
```

- Node with value 3 does not have any cousins so its sum is 0.
- Node with value 1 does not have any cousins so its sum is 0.
- Node with value 2 does not have any cousins so its sum is 0.

__Constraints:__

- The number of nodes in the tree is in the range `[1, 10 ^ 5]`.
- `1 <= Node.val <= 10 ^ 4`

__题目描述:__

给你一棵二叉树的根 `root` ，请你将每个节点的值替换成该节点的所有 __堂兄弟节点值的和__ 。

如果两个节点在树中有相同的深度且它们的父节点不同，那么它们互为 堂兄弟 。

请你返回修改值之后，树的根 `root` 。

注意，一个节点的深度指的是从树根节点到这个节点经过的边数。

__示例:__

示例 1：

![2641-3](https://assets.leetcode.com/uploads/2023/01/11/example11.png)

```text
输入：root = [5,4,9,1,10,null,7]
输出：[0,0,0,7,7,null,11]
解释：上图展示了初始的二叉树和修改每个节点的值之后的二叉树。
```

- 值为 5 的节点没有堂兄弟，所以值修改为 0 。
- 值为 4 的节点没有堂兄弟，所以值修改为 0 。
- 值为 9 的节点没有堂兄弟，所以值修改为 0 。
- 值为 1 的节点有一个堂兄弟，值为 7 ，所以值修改为 7 。
- 值为 10 的节点有一个堂兄弟，值为 7 ，所以值修改为 7 。
- 值为 7 的节点有两个堂兄弟，值分别为 1 和 10 ，所以值修改为 11 。

示例 2：

![2641-4](https://assets.leetcode.com/uploads/2023/01/11/diagram33.png)

```text
输入：root = [3,1,2]
输出：[0,0,0]
解释：上图展示了初始的二叉树和修改每个节点的值之后的二叉树。
```

- 值为 3 的节点没有堂兄弟，所以值修改为 0 。
- 值为 1 的节点没有堂兄弟，所以值修改为 0 。
- 值为 2 的节点没有堂兄弟，所以值修改为 0 。

__提示：__

- 树中节点数目的范围是 `[1, 10 ^ 5]` 。
- `1 <= Node.val <= 10 ^ 4`

__思路:__

```text
BFS
root 的值为 0, 因为它没有堂兄弟节点
两次 BFS
先用一次 BFS 计算每一层的节点值之和 next_level_sum
然后再用一次 BFS 更新每个节点的值为 next_level_sum - children_sum
其中 children_sum 是当前节点的所有子节点的值之和
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
    TreeNode* replaceValueInTree(TreeNode* root) 
    {
        root -> val = 0;
        vector<TreeNode*> q = {root};
        while (!q.empty()) 
        {
            vector<TreeNode*> cur(q);
            q.clear();
            int next_level_sum = 0;
            for (const auto& node : cur) 
            {
                if (node -> left) 
                {
                    q.emplace_back(node -> left);
                    next_level_sum += node -> left -> val;
                }
                if (node -> right) 
                {
                    q.emplace_back(node -> right);
                    next_level_sum += node -> right -> val;
                }
            }
            for (const auto& node : cur) 
            {
                int children_sum = (node -> left ? node -> left -> val : 0) + (node -> right ? node -> right -> val : 0);
                if (node -> left)  node -> left -> val = next_level_sum - children_sum;
                if (node -> right)  node -> right -> val = next_level_sum - children_sum;
            }
        }
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
    public TreeNode replaceValueInTree(TreeNode root) {
        root.val = 0;
        List<TreeNode> q = List.of(root);
        while (!q.isEmpty()) {
            var cur = q;
            q = new ArrayList<>();
            int nextLevelSum = 0;
            for (TreeNode node : cur) {
                if (node.left != null) {
                    q.add(node.left);
                    nextLevelSum += node.left.val;
                }
                if (node.right != null) {
                    q.add(node.right);
                    nextLevelSum += node.right.val;
                }
            }
            for (TreeNode node : cur) {
                int childrenSum = (node.left == null ? 0 : node.left.val) + (node.right == null ? 0 : node.right.val);
                if (node.left != null) node.left.val = nextLevelSum - childrenSum;
                if (node.right != null) node.right.val = nextLevelSum - childrenSum;
            }
        }
        return root;
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
    def replaceValueInTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        root.val = 0
        q = [root]
        while q:
            cur, q, next_level_sum = q, [], 0
            for node in cur:
                if node.left:
                    q.append(node.left)
                    next_level_sum += node.left.val
                if node.right:
                    q.append(node.right)
                    next_level_sum += node.right.val
            for node in cur:
                children_sum = (node.left.val if node.left else 0) + (node.right.val if node.right else 0)
                if node.left:
                    node.left.val = next_level_sum - children_sum
                if node.right:
                    node.right.val = next_level_sum - children_sum
        return root
```
