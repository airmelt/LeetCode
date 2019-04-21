__Description__:
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

__Example__:
For example, this binary tree [1,2,2,3,4,4,3] is symmetric:
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
But the following [1,2,2,null,3,null,3] is not:
```
    1
   / \
  2   2
   \   \
   3    3
```
__Note__:
Bonus points if you could solve it both recursively and iteratively.


__题目描述__:
给定一个二叉树，检查它是否是镜像对称的。

 __示例__:
例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
```
    1
   / \
  2   2
   \   \
   3    3
```
__说明__:
如果你可以运用递归和迭代两种方法解决这个问题，会很加分。

__思路__:
本题与[LeetCode #100 Same Tree 相同的树](https://www.jianshu.com/p/bd2076be1764)思路类似
不过是将根结点的左子树和根结点的右子树对称比较, 可以用递归(C++/Python)/迭代(Java)方式
时间复杂度O(n), 空间复杂度O(n), n为树中结点数量

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
    bool isSymmetric(TreeNode* root) {
        if (!root) return true;
        return isMirror(root, root);
    }
private:
    bool isMirror(TreeNode* p, TreeNode* q) {
        if (!p && !q) return true;
        if (!p || !q) return false;
        if (p -> val != q -> val) return false;
        return isMirror(p -> left, q -> right) && isMirror(p -> right, q -> left);
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
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        Stack<TreeNode> s1 = new Stack<>();
        Stack<TreeNode> s2 = new Stack<>();
        s1.push(root);
        s2.push(root);
        while (!s1.isEmpty() && !s2.isEmpty()) {
            TreeNode left = s1.pop();
            TreeNode right = s2.pop();
            if (left == null && right == null) continue;
            if (left == null || right == null) return false;
            if (left.val != right.val) return false;
            s1.push(left.left);
            s1.push(left.right);
            s2.push(right.right);
            s2.push(right.left);
        }
        return s1.isEmpty() && s2.isEmpty();
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
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root:
            return True
        return self.isMirror(root, root)

    def isMirror(self, p: TreeNode, q: TreeNode) -> bool:
        if not p and not q:
            return True
        elif not p or not q:
            return False
        elif p.val != q.val:
            return False
        else:
            return self.isMirror(p.left, q.right) and self.isMirror(p.right, q.left)
```
