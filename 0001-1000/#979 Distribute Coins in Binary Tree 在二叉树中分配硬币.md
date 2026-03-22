# 979 Distribute Coins in Binary Tree 在二叉树中分配硬币

__Description__:
You are given the root of a binary tree with n nodes where each node in the tree has node.val coins. There are n coins in total throughout the whole tree.

In one move, we may choose two adjacent nodes and move one coin from one node to another. A move may be from parent to child, or from child to parent.

Return the minimum number of moves required to make every node have exactly one coin.

__Example:__

Example 1:

Input: root = [3,0,0]
Output: 2
Explanation: From the root of the tree, we move one coin to its left child, and one coin to its right child.

Example 2:

Input: root = [0,3,0]
Output: 3
Explanation: From the left child of the root, we move two coins to the root [taking two moves]. Then, we move one coin from the root of the tree to the right child.

__Constraints:__

The number of nodes in the tree is n.
1 <= n <= 100
0 <= Node.val <= n
The sum of all Node.val is n.

__题目描述__:
给定一个有 N 个结点的二叉树的根结点 root，树中的每个结点上都对应有 node.val 枚硬币，并且总共有 N 枚硬币。

在一次移动中，我们可以选择两个相邻的结点，然后将一枚硬币从其中一个结点移动到另一个结点。(移动可以是从父结点到子结点，或者从子结点移动到父结点。)。

返回使每个结点上只有一枚硬币所需的移动次数。

__示例 :__

示例 1：

![tree1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/01/19/tree1.png)

输入：[3,0,0]
输出：2
解释：从树的根结点开始，我们将一枚硬币移到它的左子结点上，一枚硬币移到它的右子结点上。

示例 2：

![tree2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/01/19/tree2.png)

输入：[0,3,0]
输出：3
解释：从根结点的左子结点开始，我们将两枚硬币移到根结点上 [移动两次]。然后，我们把一枚硬币从根结点移到右子结点上。

示例 3：

![tree3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/01/19/tree3.png)

输入：[1,0,2]
输出：2

示例 4：

![tree4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/01/19/tree4.png)

输入：[1,0,0,null,3]
输出：4

__提示:__

1<= N <= 100
0 <= node.val <= N

__思路__:

后序遍历
叶子结点需要返回 x - 1 个金币给自己的父节点
这个 x - 1 有可能是正数负数或者 0
如果 x - 1 = 0, 说明其自身刚好有 1 个金币不用分配
否则结果需要加上 abs(x - 1) 表示需要给父节点或者从父节点分配 x - 1 个金币
时间复杂度为 O(n), 空间复杂度为 O(h), 其中 h 为树的高度

__代码__:
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
    int distributeCoins(TreeNode* root) 
    {
        int result = 0;
        dfs(root, result);
        return result;
    }
private:
    int dfs(TreeNode* root, int& result)
    {
        int coins = root -> val + (root -> right ? dfs(root -> right, result) : 0) + (root -> left ? dfs(root -> left, result) : 0) - 1;
        result += abs(coins);
        return coins;
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
    private int result = 0;
    
    public int distributeCoins(TreeNode root) {
        dfs(root);
        return result;
    }
    
    private int dfs(TreeNode root) {
        if (root == null) return 0;
        if (root.left != null) root.val += dfs(root.left);
        if (root.right != null) root.val += dfs(root.right);
        result += Math.abs(root.val - 1);
        return root.val - 1;
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
    def distributeCoins(self, root: Optional[TreeNode]) -> int:
        result = 0
        
        def dfs(root: TreeNode) -> int:
            nonlocal result
            coins = root.val + (dfs(root.right) if root.right else 0) + (dfs(root.left) if root.left else 0) - 1
            result += abs(coins)
            return coins
        dfs(root)
        return result
```
