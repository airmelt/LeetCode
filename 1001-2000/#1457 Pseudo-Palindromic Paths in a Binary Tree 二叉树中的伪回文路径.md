# 1457 Pseudo-Palindromic Paths in a Binary Tree 二叉树中的伪回文路径

__Description:__

Given a binary tree where node values are digits from 1 to 9. A path in the binary tree is said to be pseudo-palindromic if at least one permutation of the node values in the path is a palindrome.

Return the number of pseudo-palindromic paths going from the root node to leaf nodes.

__Example:__

Example 1:

![1457-1](https://assets.leetcode.com/uploads/2020/05/06/palindromic_paths_1.png)

```text
Input: root = [2,3,1,3,1,null,1]
Output: 2 
Explanation: The figure above represents the given binary tree. There are three paths going from the root node to leaf nodes: the red path [2,3,3], the green path [2,1,1], and the path [2,3,1]. Among these paths only red path and green path are pseudo-palindromic paths since the red path [2,3,3] can be rearranged in [3,2,3] (palindrome) and the green path [2,1,1] can be rearranged in [1,2,1] (palindrome).
```

Example 2:

![1457-2](https://assets.leetcode.com/uploads/2020/05/07/palindromic_paths_2.png)

```text
Input: root = [2,1,1,1,3,null,null,null,null,null,1]
Output: 1 
Explanation: The figure above represents the given binary tree. There are three paths going from the root node to leaf nodes: the green path [2,1,1], the path [2,1,3,1], and the path [2,1]. Among these paths only the green path is pseudo-palindromic since [2,1,1] can be rearranged in [1,2,1] (palindrome).
```

Example 3:

```text
Input: root = [9]
Output: 1
```

__Constraints:__

- The number of nodes in the tree is in the range  `[1, 10 ^ 5]`.
- `1 <= Node.val <= 9`

__题目描述:__

给你一棵二叉树，每个节点的值为 1 到 9 。我们称二叉树中的一条路径是 「伪回文」的，当它满足：路径经过的所有节点值的排列中，存在一个回文序列。

请你返回从根到叶子节点的所有路径中 伪回文 路径的数目。

__示例:__

示例 1：

![1457-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/23/palindromic_paths_1.png)

```text
输入：root = [2,3,1,3,1,null,1]
输出：2 
解释：上图为给定的二叉树。总共有 3 条从根到叶子的路径：红色路径 [2,3,3] ，绿色路径 [2,1,1] 和路径 [2,3,1] 。
     在这些路径中，只有红色和绿色的路径是伪回文路径，因为红色路径 [2,3,3] 存在回文排列 [3,2,3] ，绿色路径 [2,1,1] 存在回文排列 [1,2,1] 。
```

示例 2：

![1457-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/23/palindromic_paths_2.png)

```text
输入：root = [2,1,1,1,3,null,null,null,null,null,1]
输出：1 
解释：上图为给定二叉树。总共有 3 条从根到叶子的路径：绿色路径 [2,1,1] ，路径 [2,1,3,1] 和路径 [2,1] 。
     这些路径中只有绿色路径是伪回文路径，因为 [2,1,1] 存在回文排列 [1,2,1] 。
```

示例 3：

```text
输入：root = [9]
输出：1
```

__提示：__

- 给定二叉树的节点数目在范围  `[1, 10 ^ 5]` 内
- `1 <= Node.val <= 9`

__思路:__

```text
递归
从根结点出发记录每一条路径上的值
因为回文中出现的次数要么为偶数要么只有 1 个奇数
考虑使用异或
数字只有 1-9 只需要一个整数就能存储
时间复杂度为 O(N), 空间复杂度为 O(logN), N 为树中结点数
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
private:
    int result;
    
    void helper(TreeNode* node, int cur) 
    {
        if (!node) return;
        cur ^= (1 << node -> val);
        if (!node -> left and !node -> right) if (!cur or !(cur & (cur - 1))) ++result;
        helper(node -> left, cur);
        helper(node -> right, cur);
    }
public:
    int pseudoPalindromicPaths (TreeNode* root)
    {
        result = 0;
        helper(root, 0);
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
    
    public int pseudoPalindromicPaths (TreeNode root) {
        helper(root, 0);
        return result;
    }
    
    private void helper(TreeNode node, int cur) {
        if (node == null) return;
        cur ^= (1 << node.val);
        if (node.left == null && node.right == null) if (cur == 0 || (cur & (cur - 1)) == 0) ++result;
        helper(node.left, cur);
        helper(node.right, cur);
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
    def pseudoPalindromicPaths (self, root: Optional[TreeNode]) -> int:
        result = 0
        
        def helper(node: Optional[TreeNode], cur: int) -> NoReturn:
            nonlocal result
            if not node:
                return
            cur ^= (1 << node.val)
            if not node.left and not node.right:
                result += (not cur) or (not (cur & (cur - 1)))
            helper(node.left, cur)
            helper(node.right, cur)
        helper(root, 0)
        return result
```
