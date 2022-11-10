# 1373 Maximum Sum BST in Binary Tree 二叉搜索子树的最大键值和

__Description:__

Given a binary tree root, return the maximum sum of all keys of any sub-tree which is also a Binary Search Tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.

__Example:__

Example 1:

![二叉树 1](https://assets.leetcode.com/uploads/2020/01/30/sample_1_1709.png)

Input: root = [1,4,3,2,4,2,5,null,null,null,null,null,null,4,6]
Output: 20
Explanation: Maximum sum in a valid Binary search tree is obtained in root node with key equal to 3.

Example 2:

![二叉树 2](https://assets.leetcode.com/uploads/2020/01/30/sample_2_1709.png)

Input: root = [4,3,null,1,2]
Output: 2
Explanation: Maximum sum in a valid Binary search tree is obtained in a single root node with key equal to 2.

Example 3:

Input: root = [-4,-2,-5]
Output: 0
Explanation: All values are negatives. Return an empty BST.

__Constraints:__

The number of nodes in the tree is in the range [1, 4 \* 104].
-4 \* 10^4 <= Node.val <= 4 \* 10^4

__题目描述：__

给你一棵以 root 为根的 二叉树 ，请你返回 任意 二叉搜索子树的最大键值和。

二叉搜索树的定义如下：

任意节点的左子树中的键值都 小于 此节点的键值。
任意节点的右子树中的键值都 大于 此节点的键值。
任意节点的左子树和右子树都是二叉搜索树。

__示例：__

示例 1：

![Binary Tree 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/07/sample_1_1709.png)

输入：root = [1,4,3,2,4,2,5,null,null,null,null,null,null,4,6]
输出：20
解释：键值为 3 的子树是和最大的二叉搜索树。

示例 2：

![Binary Tree 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/07/sample_2_1709.png)

输入：root = [4,3,null,1,2]
输出：2
解释：键值为 2 的单节点子树是和最大的二叉搜索树。

示例 3：

输入：root = [-4,-2,-5]
输出：0
解释：所有节点键值都为负数，和最大的二叉搜索树为空。

示例 4：

输入：root = [2,1,3]
输出：6

示例 5：

输入：root = [5,4,8,3,null,6,3]
输出：7

__提示：__

每棵树有 1 到 40000 个节点。
每个节点的键值在 [-4 \* 10^4 , 4 \* 10^4] 之间。

__思路：__

```text
DFS
注意到 BST 的左子树和右子树都为 BST
且根结点大于左子树的最大值和小于右子树的最小值
用一个全局变量记录 BST 的最大和
后序遍历搜索所有结点并记录下当前和, 最大值和最小值
左子树初始化为 {0, root -> val, -INF}
右子树初始化为 {0, INF, root -> val}
时间复杂度为 O(n), 空间复杂度为 O(1)
```

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
    int result = 0, INF = 0x3f3f3f3f;

    vector<int> dfs(TreeNode* root) 
    {
        vector<int> left{0, root -> val, -INF}, right{0, INF, root -> val};
        if (root -> left) left = dfs(root -> left);
        if (root -> right) right = dfs(root -> right);
        if (left[2] < root -> val and right[1] > root -> val) 
        {
            int s = root -> val + left.front() + right.front();
            result = max(result, s);
            return {s, left[1], right[2]};
        }
        return {-INF, -INF, INF};
    }
public:
    int maxSumBST(TreeNode* root) 
    {
        dfs(root);
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
    private int result = 0, INF = 0x3f3f3f3f;
    public int maxSumBST(TreeNode root) {
        dfs(root);
        return result;
    }
    
    private int[] dfs(TreeNode root) {
        int[] left = new int[]{0, root.val, -INF}, right = new int[]{0, INF, root.val};
        if (root.left != null) left = dfs(root.left);
        if (root.right != null) right = dfs(root.right);
        if (left[2] < root.val && right[1] > root.val) {
            int s = root.val + left[0] + right[0];
            result = Math.max(result, s);
            return new int[]{s, left[1], right[2]};
        }
        return new int[]{-INF, -INF, INF};
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
    def maxSumBST(self, root: Optional[TreeNode]) -> int:
        self.result = 0
        def dfs(cur: Optional[TreeNode]) -> List[int]:
            left, right = [0, cur.val, -float('inf')], [0, float('inf'), cur.val]
            if cur.left:
                left = dfs(cur.left)
            if cur.right:
                right = dfs(cur.right)
            if left[2] < cur.val < right[1]:
                self.result = max(self.result, (s := cur.val + left[0] + right[0]))
                return [s, left[1], right[2]]
            return [-float('inf'), -float('inf'), float('inf')]
        dfs(root)
        return self.result
```
