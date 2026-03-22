# 637 Average of Levels in Binary Tree 二叉树的层平均值

__Description__:
Given a non-empty binary tree, return the average value of the nodes on each level in the form of an array.

__Example:__

Example 1:

Input:

```text
    3
   / \
  9  20
    /  \
   15   7
```

Output: [3, 14.5, 11]

Explanation:
The average value of nodes on level 0 is 3,  on level 1 is 14.5, and on level 2 is 11. Hence return [3, 14.5, 11].

__Note:__

The range of node's value is in the range of 32-bit signed integer.

__题目描述__:
给定一个非空二叉树, 返回一个由每层节点平均值组成的数组.

__示例 :__

示例 1:

输入:

```text
    3
   / \
  9  20
    /  \
   15   7
```

输出: [3, 14.5, 11]
解释:
第0层的平均值是 3,  第1层是 14.5, 第2层是 11. 因此返回 [3, 14.5, 11].

__注意：__

节点值的范围在32位有符号整数范围内。

__思路__:

参考[LeetCode #107 Binary Tree Level Order Traversal II 二叉树的层次遍历 II](https://www.jianshu.com/p/76abc4ff072f)
将每一层的结果记录求平均值即可
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
    vector<double> averageOfLevels(TreeNode* root) 
    {
        vector<double> result;
        queue<TreeNode*> q;
        q.push(root);
        while (q.size()) 
        {
            double total = 0;
            int nums = q.size();
            for (int i = 0; i < nums; i++) 
            {
                TreeNode* cur = q.front();
                q.pop();
                total += cur -> val;
                if (cur -> left) q.push(cur -> left);
                if (cur -> right) q.push(cur -> right);
            }
            result.push_back(total / nums);
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
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> result = new ArrayList<>();
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while (!q.isEmpty()) {
            double total = 0;
            int nums = q.size();
            for (int i = 0; i < nums; i++) {
                TreeNode cur = q.poll();
                total += cur.val;
                if (cur.left != null) q.offer(cur.left);
                if (cur.right != null) q.offer(cur.right);
            }
            result.add(total / nums);
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
    def averageOfLevels(self, root: TreeNode) -> List[float]:
        queue, result = [root], []
        while queue:
            size, total = len(queue), 0
            for _ in range(size):
                item = queue.pop(0)
                total += item.val
                if item.left:
                    queue.append(item.left)
                if item.right:
                    queue.append(item.right)
            result.append(total / size)
        return result
```
