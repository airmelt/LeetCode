# 617 Merge Two Binary Trees 合并二叉树

__Description__:
Given two binary trees and imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not.

You need to merge them into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of new tree.

__Example:__

Example 1:

Input:

```text
       Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7         
```

Output:
Merged tree:

```text
         3
        / \
       4   5
      / \   \ 
     5   4   7
```

__Note:__
The merging process must start from the root nodes of both trees.

__题目描述__:
你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。

__示例 :__

示例 1:

输入:

```text
       Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7         
```

输出:
合并后的树:

```text
         3
        / \
       4   5
      / \   \ 
     5   4   7
```

__注意:__
合并必须从两个树的根节点开始。

__思路__:

1. 迭代, 访问每一个结点, 如果均非空就加上并插入到栈中, 否则保证输出到结点非空进行交换
2. 递归, 先合并根结点然后合并左子树右子树, 可以直接在原地进行
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
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) 
    {
        stack<TreeNode*> s;
        if (!t1 and !t2) return NULL;
        if (!t1) return t2;
        if (!t2) return t1;
        TreeNode* result = !t1 ? t1 : t2;
        TreeNode* temp = !t2 ? t2 : t1;
        if (temp) result -> val += temp -> val;
        s.push(result);
        s.push(temp);
        while (s.size()) 
        {
            TreeNode* q = s.top();
            s.pop();
            TreeNode* p = s.top();
            s.pop();
            if (!q) continue;
            if (p -> left and q -> left) 
            {
                p -> left -> val += q -> left -> val;
                s.push(p -> left);
                s.push(q -> left);
            } 
            else if (!p -> left and q -> left) p -> left = q -> left;
            if (p -> right and q -> right) 
            {
                p -> right -> val += q -> right -> val;
                s.push(p -> right);
                s.push(q -> right);
            } 
            else if (!p -> right and q -> right) p -> right = q -> right;
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
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null) return t2;
        if (t2 == null) return t1;
        t1.val += t2.val;
        t1.left = mergeTrees(t1.left, t2.left);
        t1.right = mergeTrees(t1.right, t2.right);
        return t1;
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
    def mergeTrees(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        if t1 and t2:
            t1.val += t2.val
            t1.left = self.mergeTrees(t1.left, t2.left)
            t1.right = self.mergeTrees(t1.right, t2.right)
        return t1 or t2
```
