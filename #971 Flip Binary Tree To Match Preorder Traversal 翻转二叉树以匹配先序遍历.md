# 971 Flip Binary Tree To Match Preorder Traversal 翻转二叉树以匹配先序遍历

__Description__:
You are given the root of a binary tree with n nodes, where each node is uniquely assigned a value from 1 to n. You are also given a sequence of n values voyage, which is the desired pre-order traversal of the binary tree.

Any node in the binary tree can be flipped by swapping its left and right subtrees. For example, flipping node 1 will have the following effect:

![fliptree](https://assets.leetcode.com/uploads/2021/02/15/fliptree.jpg)

Flip the smallest number of nodes so that the pre-order traversal of the tree matches voyage.

Return a list of the values of all flipped nodes. You may return the answer in any order. If it is impossible to flip the nodes in the tree to make the pre-order traversal match voyage, return the list [-1].

__Example:__

Example 1:

![fliptree 1](https://assets.leetcode.com/uploads/2019/01/02/1219-01.png)

Input: root = [1,2], voyage = [2,1]
Output: [-1]
Explanation: It is impossible to flip the nodes such that the pre-order traversal matches voyage.

Example 2:

![fliptree 2](https://assets.leetcode.com/uploads/2019/01/02/1219-02.png)

Input: root = [1,2,3], voyage = [1,3,2]
Output: [1]
Explanation: Flipping node 1 swaps nodes 2 and 3, so the pre-order traversal matches voyage.

Example 3:

![fliptree 2](https://assets.leetcode.com/uploads/2019/01/02/1219-02.png)

Input: root = [1,2,3], voyage = [1,2,3]
Output: []
Explanation: The tree's pre-order traversal already matches voyage, so no nodes need to be flipped.

__Constraints:__

The number of nodes in the tree is n.
n == voyage.length
1 <= n <= 100
1 <= Node.val, voyage[i] <= n
All the values in the tree are unique.
All the values in voyage are unique.

__题目描述__:
给你一棵二叉树的根节点 root ，树中有 n 个节点，每个节点都有一个不同于其他节点且处于 1 到 n 之间的值。

另给你一个由 n 个值组成的行程序列 voyage ，表示 预期 的二叉树 先序遍历 结果。

通过交换节点的左右子树，可以 翻转 该二叉树中的任意节点。例，翻转节点 1 的效果如下：

![翻转二叉树](https://assets.leetcode.com/uploads/2021/02/15/fliptree.jpg)

请翻转 最少 的树中节点，使二叉树的 先序遍历 与预期的遍历行程 voyage 相匹配 。

如果可以，则返回 翻转的 所有节点的值的列表。你可以按任何顺序返回答案。如果不能，则返回列表 [-1]。

__示例 :__

示例 1：

![翻转二叉树 1](https://assets.leetcode.com/uploads/2019/01/02/1219-01.png)

输入：root = [1,2], voyage = [2,1]
输出：[-1]
解释：翻转节点无法令先序遍历匹配预期行程。

示例 2：

![翻转二叉树 2](https://assets.leetcode.com/uploads/2019/01/02/1219-02.png)

输入：root = [1,2,3], voyage = [1,3,2]
输出：[1]
解释：交换节点 2 和 3 来翻转节点 1 ，先序遍历可以匹配预期行程。

示例 3：

![翻转二叉树 2](https://assets.leetcode.com/uploads/2019/01/02/1219-02.png)

输入：root = [1,2,3], voyage = [1,2,3]
输出：[]
解释：先序遍历已经匹配预期行程，所以不需要翻转节点。

__提示:__

树中的节点数目为 n
n == voyage.length
1 <= n <= 100
1 <= Node.val, voyage[i] <= n
树中的所有值 互不相同
voyage 中的所有值 互不相同

__思路__:

递归
先比较根结点, 如果根结点为空直接返回
如果根结点不等于当前遍历值, 将标记值修改为 true 表示不可能翻转成功, 并返回
如果根有左右结点尝试交换之后再递归比较
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
    int i, flag;
    vector<int> result;
    
    void f(TreeNode* root, vector<int>& voyage)
    {
        if (!root) return;
        if (root -> val != voyage[i]) 
        {
            flag = 1;
            return;
        }
        ++i;
        if (root -> left and root -> right) 
        {
            if (root -> left -> val != voyage[i]) 
            {
                TreeNode* temp = root -> left;
                root -> left = root -> right;
                root -> right = temp;
                result.emplace_back(root -> val);
            }
        }
        f(root -> left, voyage);
        f(root -> right, voyage);
    }
public:
    vector<int> flipMatchVoyage(TreeNode* root, vector<int>& voyage) 
    {
        i = flag = 0;
        f(root, voyage);
        return flag ? vector<int>{ -1 } : result;
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
    private int i, flag;
    private List<Integer> result = new ArrayList<>();
    
    public List<Integer> flipMatchVoyage(TreeNode root, int[] voyage) {
        f(root, voyage);
        return flag == 1 ? Arrays.asList(new Integer[]{ -1 }) : result;
    }
    
    private void f(TreeNode root, int[] voyage) {
        if (root == null) return;
        if (root.val != voyage[i]) {
            flag = 1;
            return;
        };
        ++i;
        if (root.left != null && root.right != null) {
            if (root.left.val != voyage[i]) {
                TreeNode temp = root.left;
                root.left = root.right;
                root.right = temp;
                result.add(root.val);
            }
        }
        f(root.left, voyage);
        f(root.right, voyage);
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
    def flipMatchVoyage(self, root: TreeNode, voyage: List[int]) -> List[int]:
        i, result, flag = 0, [], False
        def f(node):
            nonlocal i, flag
            if not node:
                return
            if node.val != voyage[i]:
                flag = True
                return
            i += 1
            if node.left and node.right:
                if node.left.val != voyage[i]:
                    node.left, node.right = node.right, node.left
                    result.append(node.val)
            f(node.left)
            f(node.right)
        f(root)
        return [-1] if flag else result
```
