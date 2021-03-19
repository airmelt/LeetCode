# 515 Find Largest Value in Each Tree Row 在每个树行中找最大值

__Description__:
Given the root of a binary tree, return an array of the largest value in each row of the tree (0-indexed).

__Example:__

Example 1:

![largest_e1](https://assets.leetcode.com/uploads/2020/08/21/largest_e1.jpg)

Input: root = [1,3,2,5,3,null,9]
Output: [1,3,9]

Example 2:

Input: root = [1,2,3]
Output: [1,3]

Example 3:

Input: root = [1]
Output: [1]

Example 4:

Input: root = [1,null,2]
Output: [1,2]

Example 5:

Input: root = []
Output: []

__Constraints:__

The number of nodes in the tree will be in the range [0, 104].
-2^31 <= Node.val <= 2^31 - 1

__题目描述__:
您需要在二叉树的每一行中找到最大的值。

__示例 :__

输入:

```text
          1
         / \
        3   2
       / \   \  
      5   3   9 
```

输出: [1, 3, 9]

__思路__:

层序遍历
寻找每一层的最大值, 可以用递归或者队列迭代
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
    vector<int> largestValues(TreeNode* root) 
    {
        vector<int> result;
        if (!root) return result;
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty())
        {
            int s = q.size(), m = q.front() -> val;
            while (s--)
            {
                TreeNode *cur = q.front();
                q.pop();
                m = max(m, cur -> val);
                if (cur -> left) q.push(cur -> left);
                if (cur -> right) q.push(cur -> right);
            }
            result.emplace_back(m);
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
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        helper(root, result, 0);
        return result;
    }
    
    private void helper(TreeNode root, List<Integer> result, int level) {
        if (root == null) return;
        if (level >= result.size()) result.add(level, root.val);
        else {
            int cur = result.get(level);
            if (root.val > cur) result.set(level, root.val);
        }
        helper(root.left, result, level + 1);
        helper(root.right, result, level + 1);
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
    def largestValues(self, root: TreeNode) -> List[int]:
        result, queue = [], deque()
        if root:
            queue.append(root)
        while queue:
            n, m = len(queue), -sys.maxsize - 1
            for _ in range(n):
                m = max((node := queue.popleft()).val, m)
                (node.left and queue.append(node.left)) or (node.right and queue.append(node.right))
            result.append(m)
        return result
```
