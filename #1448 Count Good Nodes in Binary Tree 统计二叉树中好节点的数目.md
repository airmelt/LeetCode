# 1448 Count Good Nodes in Binary Tree 统计二叉树中好节点的数目

__Description:__

Given a binary tree `root`, a node _X_ in the tree is named __good__ if in the path from root to _X_ there are no nodes with a value _greater than_ X.

Return the number of good nodes in the binary tree.

__Example:__

Example 1:

![1448-1](https://assets.leetcode.com/uploads/2020/04/02/test_sample_1.png)

```text
Input: root = [3,1,4,3,null,1,5]
Output: 4
Explanation: Nodes in blue are good.
Root Node (3) is always a good node.
Node 4 -> (3,4) is the maximum value in the path starting from the root.
Node 5 -> (3,4,5) is the maximum value in the path
Node 3 -> (3,1,3) is the maximum value in the path.
```

Example 2:

![1448-2](https://assets.leetcode.com/uploads/2020/04/02/test_sample_2.png)

```text
Input: root = [3,3,null,4,2]
Output: 3
Explanation: Node 2 -> (3, 3, 2) is not good, because "3" is higher than it.
```

Example 3:

```text
Input: root = [1]
Output: 1
Explanation: Root is considered as good.
```

__Constraints:__

- The number of nodes in the binary tree is in the range `[1, 10 ^ 5]`.
- Each node's value is between `[-10 ^ 4, 10 ^ 4]`.

__题目描述:__

给你一棵根为 `root` 的二叉树，请你返回二叉树中好节点的数目。

「好节点」X 定义为：从根到该节点 X 所经过的节点中，没有任何节点的值大于 X 的值。

__示例:__

示例 1：

![1448-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/16/test_sample_1.png)

```text
输入：root = [3,1,4,3,null,1,5]
输出：4
解释：图中蓝色节点为好节点。
根节点 (3) 永远是个好节点。
节点 4 -> (3,4) 是路径中的最大值。
节点 5 -> (3,4,5) 是路径中的最大值。
节点 3 -> (3,1,3) 是路径中的最大值。
```

示例 2：

![1448-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/16/test_sample_2.png)

```text
输入：root = [3,3,null,4,2]
输出：3
解释：节点 2 -> (3, 3, 2) 不是好节点，因为 "3" 比它大。
```

示例 3：

```text
输入：root = [1]
输出：1
解释：根节点是好节点。
```

__提示：__

- 二叉树中节点数目范围是 `[1, 10 ^ 5]` 。
- 每个节点权值的范围是 `[-10 ^ 4, 10 ^ 4]` 。

__思路:__

```text
递归
从根结点出发, 根结点的值设置为最大值
按照 根 - 左 - 右 的方式依次访问所有结点并更新当前的最大值
将不小于当前最大值的结点统计入结果
递归终点为空结点
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
private:
    int helper(TreeNode* cur, int max_value)
    {
        return !cur ? 0 : (cur -> val >= max_value) + helper(cur -> left, max_value = max(cur -> val, max_value)) + helper(cur -> right, max_value);
    }
public:
    int goodNodes(TreeNode* root) 
    {
        return helper(root, root -> val);
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
    public int goodNodes(TreeNode root) {
        return helper(root, root.val);
    }
    
    private int helper(TreeNode cur, int maxValue) {
        return cur == null ? 0 : (cur.val >= maxValue ? 1 : 0) + helper(cur.left, maxValue = Math.max(cur.val, maxValue)) + helper(cur.right, maxValue);
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
    def goodNodes(self, root: TreeNode) -> int:
        return (f := lambda cur, max_value: (cur.val >= max_value) + f(cur.left, (max_value := max(max_value, cur.val))) + f(cur.right, max_value) if cur else 0)(root, root.val)
```
