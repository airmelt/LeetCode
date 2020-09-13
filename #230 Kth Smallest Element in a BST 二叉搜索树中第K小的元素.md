__Description__:
Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

__Example:__
Example 1:

Input: root = [3,1,4,null,2], k = 1
```
   3
  / \
 1   4
  \
   2
```
Output: 1

Example 2:

Input: root = [5,3,6,2,4,null,null,1], k = 3
```
       5
      / \
     3   6
    / \
   2   4
  /
 1
```
Output: 3

__Follow up:__
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

__Constraints:__

The number of elements of the BST is between 1 to 10^4.
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

__题目描述__:
给定一个二叉搜索树，编写一个函数 kthSmallest 来查找其中第 k 个最小的元素。

__说明：__
你可以假设 k 总是有效的，1 ≤ k ≤ 二叉搜索树元素个数。

__示例 :__
示例 1:

输入: root = [3,1,4,null,2], k = 1
```
   3
  / \
 1   4
  \
   2
```
输出: 1

示例 2:

输入: root = [5,3,6,2,4,null,null,1], k = 3
```
       5
      / \
     3   6
    / \
   2   4
  /
 1
```
输出: 3

__进阶：__
如果二叉搜索树经常被修改（插入/删除操作）并且你需要频繁地查找第 k 小的值，你将如何优化 kthSmallest 函数？

__思路__:
1. 中序排列一定是升序数组, 返回第 k - 1个元素即可
时间复杂度O(n), 空间复杂度O(n)
2. 利用二叉搜索树的性质, 搜索结点的子结点数
要么在左子树查找, 要么在右子树查找
时间复杂度O(nlgn), 空间复杂度O(1)
3. 中序排列并剪枝, 只要遍历到第 k个元素就返回
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
    int kthSmallest(TreeNode* root, int k) 
    {
        stack<TreeNode*> s;
        int count = 0;
        while (!s.empty() || root)
        {
            if (root)
            {
                s.push(root);
                root = root -> left;
            }
            else
            {
                root = s.top();
                s.pop();
                ++count;
                if (count == k) return root -> val;
                root = root -> right;
            }
        }
        return -1;
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
    public int kthSmallest(TreeNode root, int k) {
        int left = helper(root.left);
        if (left >= k) return kthSmallest(root.left, k);
        else if (left + 1 == k) return root.val;
        return kthSmallest(root.right, k - left - 1);
    }

    private int helper(TreeNode root){
        if (root == null) return 0;
        return helper(root.left) + helper(root.right) + 1;
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
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        def helper(root: TreeNode) -> List[int]:
            return [] if not root else helper(root.left) + [root.val] + helper(root.right)
        return helper(root)[k - 1]
```