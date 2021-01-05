# 104 Maximum Depth of Binary Tree 二叉树的最大深度

__Description__:
Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

__Note__: A leaf is a node with no children.

__Example__:

Given binary tree [3,9,20,null,null,15,7],

```text
    3
   / \
  9  20
    /  \
   15   7
```

return its depth = 3.

__题目描述__:
给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

__说明__： 叶子节点是指没有子节点的节点。

__示例__:

给定二叉树 [3,9,20,null,null,15,7]，

```text
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最大深度 3 。

__思路__:

主体思想是采用[DFS(深度优先搜索)](https://en.wikipedia.org/wiki/Depth-first_search)

1. 递归, 最大深度是左右子树中的较大值, 每次递归子节点的时候 +1
2. 迭代, 采用堆栈, 逐层扫描, 每到新的一层 +1
时间复杂度O(n), 空间复杂度O(n), 其中 n为树中的结点数, 因为每个结点都要访问一次

[DFS(深度优先搜索)](https://en.wikipedia.org/wiki/Depth-first_search)
> **深度优先搜索算法**（英语：Depth-First-Search，DFS）是一种用于遍历或搜索[树](https://zh.wikipedia.org/wiki/%E6%A0%91_(%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84) "树 (数据结构)")或[图](https://zh.wikipedia.org/wiki/%E5%9B%BE_(%E6%95%B0%E5%AD%A6) "图 (数学)")的[算法](https://zh.wikipedia.org/wiki/%E7%AE%97%E6%B3%95)
> 沿着树的深度遍历树的节点，尽可能深的搜索树的分支。
> 当节点v的所在边都己被探寻过，搜索将回溯到发现节点v的那条边的起始节点。
> 这一过程一直进行到已发现从源节点可达的所有节点为止。
> 如果还存在未被发现的节点，则选择其中一个作为源节点并重复以上过程，整个进程反复进行直到所有节点都被访问为止。属于盲目搜索。
> 步骤:
>
> 1. 首先将根节点放入堆栈中。
> 2. 从堆栈中取出第一个节点，并检验它是否为目标。如果找到目标，则结束搜寻并回传结果。否则将它某一个尚未检验过的直接子节点加入堆栈中。
> 3. 重复步骤2。
> 4. 如果不存在未检测过的直接子节点。将上一级节点加入堆栈中。重复步骤2。
> 5. 重复步骤4。
> 6. 若堆栈为空，表示整张图都检查过了——亦即图中没有欲搜寻的目标。结束搜寻并回传“找不到目标”。

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
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
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
     int maxDepth(TreeNode* root) 
     {
         if (!root) return 0;
         stack<TreeNode*> s;
         s.push(root);
         int result = 0;
         while (!s.empty()) 
         {
             int nums = s.size();
             while (nums > 0) 
             {
                 TreeNode* current = s.top();
                 s.pop();
                 if (current -> left) s.push(current -> left);
                 if (current -> right) s.push(current -> right);
                 nums--;
             }
             result++;
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
    public int maxDepth(TreeNode root) {
        return root == null ? 0 : Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
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
    def maxDepth(self, root: TreeNode) -> int:
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1 if root else 0
```
