__Description__:
Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

__Example__:
Example 1:
Input:   
```
           1         1
          / \       / \
         2   3     2   3
        [1,2,3],   [1,2,3]
```
Output: true

Example 2:
Input:     
```
          1          1
          /           \
         2             2
        [1,2],     [1,null,2]
```
Output: false

Example 3:
Input:     
```
           1         1
          / \       / \
         2   1     1   2
        [1,2,1],   [1,1,2]
```
Output: false

__题目描述__:
给定两个二叉树，编写一个函数来检验它们是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

 __示例__:
示例 1:
输入:
```
           1         1
          / \       / \
         2   3     2   3
        [1,2,3],   [1,2,3]
```
输出: true

示例 2:
输入:
```
          1          1
          /           \
         2             2
        [1,2],     [1,null,2]
```
输出: false

示例 3:
输入:
```
           1         1
          / \       / \
         2   1     1   2
        [1,2,1],   [1,1,2]
```
输出: false

__思路__:
先判断两个树是否空, 两棵树都为空才返回true
否则比较两棵树的左右两边, 可以用递归(C++/Java)/迭代(Python)方式
时间复杂度O(n), 空间复杂度O(n), n为树中结点数量

[树 Tree-wiki](https://en.wikipedia.org/wiki/Tree_(data_structure)):
> 树是由n（n>0）个有限节点组成一个具有层次关系的[集合](https://zh.wikipedia.org/wiki/%E9%9B%86%E5%90%88 "集合")。把它叫做“树”是因为它看起来像一棵倒挂的树，也就是说它是根朝上，而叶朝下的。
> 特点:
> 1. 每个节点都只有有限个子节点或无子节点；
> 2. 没有父节点的节点称为根节点；
> 3. 每一个非根节点有且只有一个父节点；
> 4. 除了根节点外，每个子节点可以分为多个不相交的子树；
> 5. 树里面没有环路(cycle)
> 树的术语:
> - 节点的度：一个节点含有的子树的个数称为该节点的度；
> - 树的度：一棵树中，最大的节点的度称为树的度；
> - 叶节点或终端节点：度为零的节点；
> - 非终端节点或分支节点：度不为零的节点；
> - 父亲节点或父节点：若一个节点含有子节点，则这个节点称为其子节点的父节点；
> - 孩子节点或子节点：一个节点含有的子树的根节点称为该节点的子节点；
> - 兄弟节点：具有相同父节点的节点互称为兄弟节点；
> - 节点的层次：从根开始定义起，根为第1层，根的子节点为第2层，以此类推；
> - 深度：对于任意节点n,n的深度为从根到n的唯一路径长，根的深度为0；
> - 高度：对于任意节点n,n的高度为从n到一片树叶的最长路径长，所有树叶的高度为0；
> - 堂兄弟节点：父节点在同一层的节点互为堂兄弟；
> - 节点的祖先：从根到该节点所经分支上的所有节点；
> - 子孙：以某节点为根的子树中任一节点都称为该节点的子孙。
> - 森林：由m（m>=0）棵互不相交的树的集合称为森林；
> C++使用__左孩子右兄弟__定义树:
> ```
> struct TreeNode{
> //该节点的元素
> int element;
> //指向该节点的第一个孩子
> TreeNode *firstChild;
> //指向该节点的兄弟节点
> TreeNode *nextSibling;
> };
> ```
__代码__:
__C++__:
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (!p && !q) return true;
        if (p && q && p -> val == q -> val) return isSameTree(p -> left, q -> left) && isSameTree(p -> right, q -> right);
        else return false;
    }
};
```

__Java__:
```
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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) return true;
        if (p != null && q != null && p.val == q.val) return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
        else return false;
    }
}
```

__Python__:
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        stack = [(p, q)]
        while stack:
            m, n = stack.pop(0)
            if m and n and m.val == n.val:
                stack.append((m.left, n.left))
                stack.append((m.right, n.right))
            else:
                if m != n:
                    return False
        return True
```
