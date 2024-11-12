# 2331 Evaluate Boolean Binary Tree 计算布尔二叉树的值

__Description:__

You are given the `root` of a __full binary tree__ with the following properties:

- __Leaf nodes__ have either the value `0` or `1`, where `0` represents `False` and `1` represents `True`.
- __Non-leaf nodes__ have either the value `2` or `3`, where `2` represents the boolean `OR` and `3` represents the boolean `AND`.

The evaluation of a node is as follows:

- If the node is a leaf node, the evaluation is the __value__ of the node, i.e. `True` or `False`.
- Otherwise, __evaluate__ the node's two children and __apply__ the boolean operation of its value with the children's evaluations.

Return _the boolean result of __evaluating__ the_ `root` _node._

A __full binary tree__ is a binary tree where each node has either `0` or `2` children.

A leaf node is a node that has zero children.

__Example:__

Example 1:

![2331-1](https://assets.leetcode.com/uploads/2022/05/16/example1drawio1.png)

```text
Input: root = [2,1,3,null,null,0,1]
Output: true
Explanation: The above diagram illustrates the evaluation process.
The AND node evaluates to False AND True = False.
The OR node evaluates to True OR False = True.
The root node evaluates to True, so we return true.
```

Example 2:

```text
Input: root = [0]
Output: false
Explanation: The root node is a leaf node and it evaluates to false, so we return false.
```

__Constraints:__

- The number of nodes in the tree is in the range `[1, 1000]`.
- `0 <= Node.val <= 3`
- Every node has either `0` or `2` children.
- Leaf nodes have a value of `0` or `1`.
- Non-leaf nodes have a value of `2` or `3`.

__题目描述:__

给你一棵 完整二叉树 的根，这棵树有以下特征：

- __叶子节点__ 要么值为 `0` 要么值为 `1` ，其中 `0` 表示 `False` ， `1` 表示 `True` 。
- __非叶子节点__ 要么值为 `2` 要么值为 `3` ，其中 `2` 表示逻辑或 `OR` ， `3` 表示逻辑与 `AND` 。

计算 一个节点的值方式如下：

- 如果节点是个叶子节点，那么节点的 __值__ 为它本身，即 `True` 或者 `False` 。
- 否则，__计算__ 两个孩子的节点值，然后将该节点的运算符对两个孩子值进行 __运算__ 。

返回根节点 `root` 的布尔运算值。

__完整二叉树__ 是每个节点有 `0` 个或者 `2` 个孩子的二叉树。

叶子节点 是没有孩子的节点。

__示例:__

示例 1：

![2331-2](https://assets.leetcode.com/uploads/2022/05/16/example1drawio1.png)

```text
输入：root = [2,1,3,null,null,0,1]
输出：true
解释：上图展示了计算过程。
AND 与运算节点的值为 False AND True = False 。
OR 运算节点的值为 True OR False = True 。
根节点的值为 True ，所以我们返回 true 。
```

示例 2：

```text
输入：root = [0]
输出：false
解释：根节点是叶子节点，且值为 false，所以我们返回 false 。
```

__提示：__

- 树中节点数目在 `[1, 1000]` 之间。
- `0 <= Node.val <= 3`
- 每个节点的孩子数为 `0` 或 `2` 。
- 叶子节点的值为 `0` 或 `1` 。
- 非叶子节点的值为 `2` 或 `3` 。

__思路:__

```text
递归
根据题意, 递归计算左右子树的值
如果根节点的值为 2, 则左右子树进行或运算
如果根节点的值为 3, 则左右子树进行与运算
否则, 返回根节点的值是否为 0
时间复杂度为 O(N), 空间复杂度为 O(1)
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
    bool evaluateTree(TreeNode* root) 
    {
        return root -> val == 2 ? evaluateTree(root -> left) or evaluateTree(root -> right) : (root -> val == 3 ? evaluateTree(root -> left) and evaluateTree(root -> right) : !!root -> val);
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
    public boolean evaluateTree(TreeNode root) {
        return root.val == 2 ? evaluateTree(root.left) || evaluateTree(root.right) : (root.val == 3 ? evaluateTree(root.left) && evaluateTree(root.right) : root.val != 0);
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
    def evaluateTree(self, root: Optional[TreeNode]) -> bool:
        return self.evaluateTree(root.left) or self.evaluateTree(root.right) if root.val == 2 else self.evaluateTree(root.left) and self.evaluateTree(root.right) if root.val == 3 else not not root.val
```
