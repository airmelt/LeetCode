# 1026 Maximum Difference Between Node and Ancestor 节点与其祖先之间的最大差值

__Description__:
Given the root of a binary tree, find the maximum value v for which there exist different nodes a and b where v = |a.val - b.val| and a is an ancestor of b.

A node a is an ancestor of b if either: any child of a is equal to b or any child of a is an ancestor of b.

__Example:__

Example 1:

![tree 1](https://assets.leetcode.com/uploads/2020/11/09/tmp-tree.jpg)

Input: root = [8,3,10,1,6,null,14,null,null,4,7,13]
Output: 7
Explanation: We have various ancestor-node differences, some of which are given below :
|8 - 3| = 5
|3 - 7| = 4
|8 - 1| = 7
|10 - 13| = 3
Among all possible differences, the maximum value of 7 is obtained by |8 - 1| = 7.

Example 2:

![tree 2](https://assets.leetcode.com/uploads/2020/11/09/tmp-tree-1.jpg)

Input: root = [1,null,2,null,0,3]
Output: 3

__Constraints:__

The number of nodes in the tree is in the range [2, 5000].
0 <= Node.val <= 10^5

__题目描述__:
给定二叉树的根节点 root，找出存在于 不同 节点 A 和 B 之间的最大值 V，其中 V = |A.val - B.val|，且 A 是 B 的祖先。

（如果 A 的任何子节点之一为 B，或者 A 的任何子节点是 B 的祖先，那么我们认为 A 是 B 的祖先）

__示例 :__

示例 1：

![树1](https://assets.leetcode.com/uploads/2020/11/09/tmp-tree.jpg)

输入：root = [8,3,10,1,6,null,14,null,null,4,7,13]
输出：7
解释：
我们有大量的节点与其祖先的差值，其中一些如下：
|8 - 3| = 5
|3 - 7| = 4
|8 - 1| = 7
|10 - 13| = 3
在所有可能的差值中，最大值 7 由 |8 - 1| = 7 得出。

示例 2：

![树 2](https://assets.leetcode.com/uploads/2020/11/09/tmp-tree-1.jpg)

输入：root = [1,null,2,null,0,3]
输出：3

__提示:__

树中的节点数在 2 到 5000 之间。
0 <= Node.val <= 10^5

__思路__:

DFS
由于绝对值中两个数可以交换顺序
所以并不需要找到祖先和对应节点
只要找到一条路径上所有结点的最大值和最小值
求两者之差
结果为所有路径的差的最大值
时间复杂度为 O(n), 空间复杂度为 O(1), 其中 n 表示结点数

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
    int maxAncestorDiff(TreeNode* root) 
    {
        return dfs(root, root -> val, root -> val);
    }
private:
    int dfs(TreeNode* root, int max_value, int min_value)
    {
        return root ? max(dfs(root -> left, max(max_value, root -> val), min(min_value, root -> val)), dfs(root -> right, max(max_value, root -> val), min(min_value, root -> val))) : max_value - min_value;
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
    public int maxAncestorDiff(TreeNode root) {
        return dfs(root, root.val, root.val);
    }
    
    private int dfs(TreeNode root, int maxValue, int minValue){
        return root != null ? Math.max(dfs(root.left, Math.max(maxValue, root.val), Math.min(minValue, root.val)), dfs(root.right, Math.max(maxValue, root.val), Math.min(minValue, root.val))) : maxValue - minValue;
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
    def maxAncestorDiff(self, root: Optional[TreeNode]) -> int:
        def dfs(cur: Optional[TreeNode], max_value: int, min_value: int) -> int:
            return max(dfs(cur.left, max(max_value, cur.val), min(min_value, cur.val)), dfs(cur.right, max(max_value, cur.val), min(min_value, cur.val))) if cur else max_value - min_value
        return dfs(root, root.val, root.val)
```
