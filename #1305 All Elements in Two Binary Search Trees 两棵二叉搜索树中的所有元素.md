# 1302 Deepest Leaves Sum 层数最深叶子节点的和

__Description:__

Given two binary search trees root1 and root2, return a list containing all the integers from both trees sorted in ascending order.

__Example:__

Example 1:

![Binary Search Tree 1](https://assets.leetcode.com/uploads/2019/12/18/q2-e1.png)

Input: root1 = [2,1,4], root2 = [1,0,3]
Output: [0,1,1,2,3,4]

Example 2:

![Binary Search Tree2](https://assets.leetcode.com/uploads/2019/12/18/q2-e5-.png)

Input: root1 = [1,null,8], root2 = [8,1]
Output: [1,1,8,8]

__Constraints:__

The number of nodes in each tree is in the range [0, 5000].
-10^5 <= Node.val <= 10^5

__题目描述：__

给你 root1 和 root2 这两棵二叉搜索树。请你返回一个列表，其中包含 两棵树 中的所有整数并按 升序 排序。.

__示例：__

示例 1：

![二叉搜索树 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/12/29/q2-e1.png)

输入：root1 = [2,1,4], root2 = [1,0,3]
输出：[0,1,1,2,3,4]

示例 2：

![二叉搜索树 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/12/29/q2-e5-.png)

输入：root1 = [1,null,8], root2 = [8,1]
输出：[1,1,8,8]

__提示：__

每棵树的节点数在 [0, 5000] 范围内
-10^5 <= Node.val <= 10^5

__思路：__

1. 迭代
中序遍历
用栈进行中序遍历, 同时合并较小值到结果中
时间复杂度为 O(n), 空间复杂度为 O(n)
2. 递归
先中序遍历然后排序
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

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
    vector<int> getAllElements(TreeNode* root1, TreeNode* root2) 
    {
        vector<int> result;
        stack<TreeNode*> s1, s2;
        while (!s1.empty() or !s2.empty() or root1 or root2)
        {
            while (root1)
            {
                s1.push(root1);
                root1 = root1 -> left;
            }
            while (root2)
            {
                s2.push(root2);
                root2 = root2 -> left;
            }
            int cur1 = s1.empty() ? INT_MAX : s1.top() -> val, cur2 = s2.empty() ? INT_MAX : s2.top() -> val;
            if (cur1 < cur2) 
            {
                root1 = s1.top() -> right;
                s1.pop();
            }
            else
            {
                root2 = s2.top() -> right;
                s2.pop();
            }
            result.emplace_back(min(cur1, cur2));
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
    public List<Integer> getAllElements(TreeNode root1, TreeNode root2) {
        List<Integer> result = new ArrayList<>();
        dfs(result, root1);
        dfs(result, root2);
        Collections.sort(result);
        return result;
    }
    
    private void dfs(List<Integer> result, TreeNode root) {
        if (root != null) {
            result.add(root.val);
            dfs(result, root.left);
            dfs(result, root.right);
        }
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
    def getAllElements(self, root1: TreeNode, root2: TreeNode) -> List[int]:
        return heapq.merge((f := lambda r: r and [*f(r.left), r.val, *f(r.right)] or [])(root1), f(root2))
```
