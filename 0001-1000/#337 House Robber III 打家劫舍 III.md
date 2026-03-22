# 337 House Robber III 打家劫舍 III

__Description__:
The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

__Example:__

Example 1:

Input: [3,2,3,null,3,null,1]

```text
     3
    / \
   2   3
    \   \ 
     3   1
```

Output: 7
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.

Example 2:

Input: [3,4,5,1,3,null,1]

```text
     3
    / \
   4   5
  / \   \ 
 1   3   1
```

Output: 9
Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.

__题目描述__:
在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。

__示例 :__

示例 1:

输入: [3,2,3,null,3,null,1]

```text
     3
    / \
   2   3
    \   \ 
     3   1
```

输出: 7
解释: 小偷一晚能够盗取的最高金额 = 3 + 3 + 1 = 7.

示例 2:

输入: [3,4,5,1,3,null,1]

```text
     3
    / \
   4   5
  / \   \ 
 1   3   1
```

输出: 9
解释: 小偷一晚能够盗取的最高金额 = 4 + 5 = 9.

__思路__:

记忆化搜索
从根节点(root)开始
选择根节点, 或者选择根节点的子节点的左右节点, 按层搜索, 并把结果加入一个 map中
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
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution 
{
public:
    int rob(TreeNode* root) 
    {
        return helper(root);
    }
private:
    unordered_map<TreeNode*, int> m;
    
    int helper(TreeNode* root)
    {
        return !root ? 0 : (m.count(root) ? m[root] : m[root] = max(helper(root -> left) + helper(root -> right), (root -> left ? helper(root -> left -> left) + helper(root -> left -> right) : 0) + (root -> right ? helper(root -> right -> left) + helper(root -> right -> right) : 0) + root -> val));
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
    public int rob(TreeNode root) {
        return helper(root);
    }
    
    private Map<TreeNode, Integer> map = new HashMap<>();
    
    private int helper(TreeNode root) {
        if (root == null) return 0;
        if (map.get(root) != null) return map.get(root);
        map.put(root, Math.max(helper(root.left) + helper(root.right), (root.left != null ? helper(root.left.left) + helper(root.left.right) : 0) + (root.right != null ? helper(root.right.left) + helper(root.right.right) : 0) + root.val));
        return map.get(root);
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
    def rob(self, root: TreeNode) -> int:
        d = {}
        def helper(root: TreeNode) -> int:
            if not root:
                return 0
            if root in d:
                return d[root]
            d[root] = max(helper(root.left) + helper(root.right), (helper(root.left.left) + helper(root.left.right) if root.left else 0) + (helper(root.right.left) + helper(root.right.right) if root.right else 0) + root.val)
            return d[root]
        return helper(root)
```
