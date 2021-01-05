# 993 Cousins in Binary Tree 二叉树的堂兄弟节点

__Description__:
In a binary tree, the root node is at depth 0, and children of each depth k node are at depth k+1.

Two nodes of a binary tree are cousins if they have the same depth, but have different parents.

We are given the root of a binary tree with unique values, and the values x and y of two different nodes in the tree.

Return true if and only if the nodes corresponding to the values x and y are cousins.

__Example:__

Example 1:
![Binary Tree 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/16/q1248-01.png)
Input: root = [1,2,3,4], x = 4, y = 3
Output: false

Example 2:
![Binary Tree 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/16/q1248-02.png)
Input: root = [1,2,3,null,4,null,5], x = 5, y = 4
Output: true

Example 3:
![Binary Tree 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/16/q1248-03.png)
Input: root = [1,2,3,null,4], x = 2, y = 3
Output: false

__Note:__

The number of nodes in the tree will be between 2 and 100.
Each node has a unique integer value from 1 to 100.

__题目描述__:
在二叉树中，根节点位于深度 0 处，每个深度为 k 的节点的子节点位于深度 k+1 处。

如果二叉树的两个节点深度相同，但父节点不同，则它们是一对堂兄弟节点。

我们给出了具有唯一值的二叉树的根节点 root，以及树中两个不同节点的值 x 和 y。

只有与值 x 和 y 对应的节点是堂兄弟节点时，才返回 true。否则，返回 false。

__示例 :__

示例 1：
![二叉树1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/16/q1248-01.png)
输入：root = [1,2,3,4], x = 4, y = 3
输出：false

示例 2：
![二叉树2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/16/q1248-02.png)
输入：root = [1,2,3,null,4,null,5], x = 5, y = 4
输出：true

示例 3：
![二叉树3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/16/q1248-03.png)
输入：root = [1,2,3,null,4], x = 2, y = 3
输出：false

__提示：__

二叉树的节点数介于 2 到 100 之间。
每个节点的值都是唯一的、范围为 1 到 100 的整数。

__思路__:

1. 层序遍历, 如果x, y在同一层, 且不是相邻的两个结点(较小的结点的下标不能为偶数), 则为堂兄弟结点, 如果为空, 可以在该层加入 0或者 null
2. 遍历同时记录下结点的高度和父结点, 比较 x和 y的高度和父结点即可
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
    bool isCousins(TreeNode* root, int x, int y) 
    {
        dfs(root, x, y, 0);
        return (height_x == height_y) && (parent_x != parent_y);
    }
private:
    int height_x = -1, height_y = -1, parent_x = -1, parent_y = -1;
    void dfs(const TreeNode* root, const int x, const int y, const int h)
    {
        if (!root) return;
        if (root -> left and root -> left -> val == x or root -> right and root -> right -> val == x)
        {
            parent_x = root -> val;
            height_x = h;
        }
        if (root -> left and root -> left -> val == y or root -> right and root -> right -> val == y)
        {
            parent_y = root -> val;
            height_y = h;
        }
        dfs(root -> left, x, y, h + 1);
        dfs(root -> right, x, y, h + 1);
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
    private Map<Integer, Integer> depth;
    private Map<Integer, TreeNode> parent;

    public boolean isCousins(TreeNode root, int x, int y) {
        depth = new HashMap();
        parent = new HashMap();
        dfs(root, null);
        return (depth.get(x) == depth.get(y) && parent.get(x) != parent.get(y));
    }

    public void dfs(TreeNode node, TreeNode par) {
        if (node != null) {
            depth.put(node.val, par != null ? 1 + depth.get(par.val) : 0);
            parent.put(node.val, par);
            dfs(node.left, node);
            dfs(node.right, node);
        }
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
    def isCousins(self, root: TreeNode, x: int, y: int) -> bool:
        queue = [root]
        while queue:
            size, level = len(queue), []
            for _ in range(size):
                cur = queue.pop(0)
                if cur:
                    level.append(cur.val)
                    queue.append(cur.left)
                    queue.append(cur.right)
                else:
                    level.append(0)
            if x in level and y in level:
                index_x, index_y = min(level.index(x), level.index(y)), max(level.index(x), level.index(y))
                if index_x + 1 == index_y and not (index_x & 1):
                    return False
                return True
            if x in level or y in level:
                return False
        return False
```
