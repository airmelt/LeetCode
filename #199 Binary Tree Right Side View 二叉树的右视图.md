__Description__:
Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

__Example:__

Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]
Explanation:
```
   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

__题目描述__:
给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

__示例 :__

输入: [1,2,3,null,5,null,4]
输出: [1, 3, 4]
解释:
```
   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

__思路__:
参考[LeetCode #102 Binary Tree Level Order Traversal 二叉树的层序遍历](https://www.jianshu.com/p/61e7034d309e)
层次遍历按层输出最右边的结点
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
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution 
{
public:
    vector<int> rightSideView(TreeNode* root) 
    {
        vector<int> result;
        if (!root) return result;
        queue<TreeNode*> q;
        q.push(root);
        while (q.size())
        {
            int s = q.size();
            result.push_back(q.front() -> val);
            while (s--)
            {
                TreeNode* cur = q.front();
                q.pop();
                if (cur -> right) q.push(cur -> right);
                if (cur -> left) q.push(cur -> left);
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
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int s = queue.size();
            result.add(queue.peek().val);
            while (s-- > 0) {
                TreeNode cur = queue.poll();
                if (cur.right != null) queue.offer(cur.right);
                if (cur.left != null) queue.offer(cur.left);
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
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def rightSideView(self, root: TreeNode) -> List[int]:
        d, f = {}, lambda r, i: r and (d.__setitem__(i, r.val) or f(r.left, i + 1) or f(r.right, i + 1))
        return f(root, 0) or [*d.values()]
```