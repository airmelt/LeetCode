# 1161 Maximum Level Sum of a Binary Tree 最大层内元素和

__Description__:
Given the root of a binary tree, the level of its root is 1, the level of its children is 2, and so on.

Return the smallest level x such that the sum of all the values of nodes at level x is maximal.

__Example:__

Example 1:

![Binary Tree](https://assets.leetcode.com/uploads/2019/05/03/capture.JPG)

Input: root = [1,7,0,7,-8,null,null]
Output: 2
Explanation:
Level 1 sum = 1.
Level 2 sum = 7 + 0 = 7.
Level 3 sum = 7 + -8 = -1.
So we return the level with the maximum sum which is level 2.

Example 2:

Input: root = [989,null,10250,98693,-89388,null,null,null,-32127]
Output: 2

__Constraints:__

The number of nodes in the tree is in the range [1, 10^4].
-10^5 <= Node.val <= 10^5

__题目描述__:
给你一个二叉树的根节点 root。设根节点位于二叉树的第 1 层，而根节点的子节点位于第 2 层，依此类推。

请返回层内元素之和 最大 的那几层（可能只有一层）的层号，并返回其中 最小 的那个。

__示例 :__

示例 1：

![二叉树](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/08/17/capture.jpeg)

输入：root = [1,7,0,7,-8,null,null]
输出：2
解释：
第 1 层各元素之和为 1，
第 2 层各元素之和为 7 + 0 = 7，
第 3 层各元素之和为 7 + -8 = -1，
所以我们返回第 2 层的层号，它的层内元素之和最大。

示例 2：

输入：root = [989,null,10250,98693,-89388,null,null,null,-32127]
输出：2

__提示:__

树中的节点数在 [1, 10^4]范围内
-10^5 <= Node.val <= 10^5

__思路__:

层序遍历
可以用迭代或者递归
记录每一层的和及对应层数输出最大的和的层的编号
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
public:
    int maxLevelSum(TreeNode* root) 
    {
        queue<TreeNode*> q;
        if (root) q.push(root);
        int result = 0, max_value = INT_MIN, level = 0;
        while (!q.empty()) 
        {
            int size = q.size(), cur = 0;
            for (int i = 0; i < size; i++) 
            {
                TreeNode* child = q.front();
                q.pop();
                cur += child -> val;
                if (child -> left) q.push(child -> left);
                if (child -> right) q.push(child -> right);
            }
            ++level;
            if (cur > max_value) 
            {
                max_value = cur;
                result = level;
            }
        }
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
    public int maxLevelSum(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        if (root != null) queue.offer(root);
        int result = 0, maxValue = Integer.MIN_VALUE, level = 0;
        while (!queue.isEmpty()) {
            int size = queue.size(), cur = 0;
            for (int i = 0; i < size; i++) {
                TreeNode child = queue.poll();
                cur += child.val;
                if (child.left != null) queue.offer(child.left);
                if (child.right != null) queue.offer(child.right);
            }
            ++level;
            if (cur > maxValue) {
                maxValue = cur;
                result = level;
            }
        }
        return result;
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
    def maxLevelSum(self, root: Optional[TreeNode]) -> int:
        s = []
        def dfs(cur: Optional[TreeNode], level: int) -> None:
            if not cur:
                return
            if level + 1 > len(s):
                s.append(0)
            s[level] += cur.val
            dfs(cur.left, level + 1)
            dfs(cur.right, level + 1)
        
        dfs(root, 0)
        return s.index(max(s)) + 1
```
