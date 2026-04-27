# 1339 Maximum Product of Splitted Binary Tree 分裂二叉树的最大乘积

__Description:__

Given the root of a binary tree, split the binary tree into two subtrees by removing one edge such that the product of the sums of the subtrees is maximized.

Return the maximum product of the sums of the two subtrees. Since the answer may be too large, return it modulo 109 + 7.

Note that you need to maximize the answer before taking the mod and not after taking it.

__Example:__

Example 1:

![Splitted Binary Tree 1](https://assets.leetcode.com/uploads/2020/01/21/sample_1_1699.png)

Input: root = [1,2,3,4,5,6]
Output: 110
Explanation: Remove the red edge and get 2 binary trees with sum 11 and 10. Their product is 110 (11*10)

Example 2:

![Splitted Binary Tree 2](https://assets.leetcode.com/uploads/2020/01/21/sample_2_1699.png)

Input: root = [1,null,2,3,4,null,null,5,6]
Output: 90
Explanation: Remove the red edge and get 2 binary trees with sum 15 and 6.Their product is 90 (15*6)

__Constraints:__

The number of nodes in the tree is in the range [2, 5 * 104].
1 <= Node.val <= 10^4

__题目描述：__

给你一棵二叉树，它的根为 root 。请你删除 1 条边，使二叉树分裂成两棵子树，且它们子树和的乘积尽可能大。

由于答案可能会很大，请你将结果对 10^9 + 7 取模后再返回。

__示例：__

示例 1：

![分裂二叉树 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/02/sample_1_1699.png)

输入：root = [1,2,3,4,5,6]
输出：110
解释：删除红色的边，得到 2 棵子树，和分别为 11 和 10 。它们的乘积是 110 （11*10）

示例 2：

![分裂二叉树 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/02/sample_2_1699.png)

输入：root = [1,null,2,3,4,null,null,5,6]
输出：90
解释：移除红色的边，得到 2 棵子树，和分别是 15 和 6 。它们的乘积为 90 （15*6）

示例 3：

输入：root = [2,3,9,10,7,8,6,5,4,11,1]
输出：1025

示例 4：

输入：root = [1,1]
输出：1

__提示：__

每棵树最多有 50000 个节点，且至少有 2 个节点。
每个节点的值在 [1, 10000] 之间。

__思路：__

DFS
先遍历二叉树, 记录以当前节点为根的子树的和
取得二叉树的所有节点的和
删除当前边的乘积等于当前节点的子树和和所有节点的和减去子树的和的乘积
时间复杂度为 O(n), 空间复杂度为 O(n)

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
private:
    vector<long> list;
    
    long dfs(TreeNode* root)
    {
        if (!root) return 0;
        long total = dfs(root -> left) + root -> val + dfs(root -> right);
        list.emplace_back(total);
        return total;
    }
public:
    int maxProduct(TreeNode* root)
    {
        long total = dfs(root), result = 0, mod = 1e9 + 7;
        for (long i : list) result = max(result, i * (total - i));
        return (int)(result % mod);
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
    private List<Long> list;
    public int maxProduct(TreeNode root) {
        list = new ArrayList<>();
        long total = dfs(root), result = 0, mod = 1_000_000_007;
        for (long i : list) result = Math.max(result, i * (total - i));
        return (int)(result % mod);
    }
    
    private long dfs(TreeNode root) {
        if (root == null) return 0;
        long total = dfs(root.left) + root.val + dfs(root.right);
        list.add(total);
        return total;
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
    def maxProduct(self, root: Optional[TreeNode]) -> int:
        s = []
        
        def dfs(cur: Optional[TreeNode]) -> int:
            if not cur:
                return 0
            s.append(total := dfs(cur.left) + cur.val + dfs(cur.right))
            return total
        total = dfs(root)
        return max(i * (total - i) for i in s) % (10 ** 9 + 7)
```
