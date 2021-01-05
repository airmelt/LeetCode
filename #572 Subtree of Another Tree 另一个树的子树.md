# 572 Subtree of Another Tree 另一个树的子树

__Description__:
Given two non-empty binary trees s and t, check whether tree t has exactly the same structure and node values with a subtree of s. A subtree of s is a tree consists of a node in s and all of this node's descendants. The tree s could also be considered as a subtree of itself.

__Example:__

Example 1:
Given tree s:

```text
     3
    / \
   4   5
  / \
 1   2
```

Given tree t:

```text
   4
  / \
 1   2
```

Return true, because t has the same structure and node values with a subtree of s.

Example 2:
Given tree s:

```text
     3
    / \
   4   5
  / \
 1   2
    /
   0
```

Given tree t:

```text
   4
  / \
 1   2
```

Return false.

__题目描述__:
给定两个非空二叉树 s 和 t，检验 s 中是否包含和 t 具有相同结构和节点值的子树。s 的一个子树包括 s 的一个节点和这个节点的所有子孙。s 也可以看做它自身的一棵子树。

__示例 :__

示例 1:
给定的树 s:

```text
     3
    / \
   4   5
  / \
 1   2
```

给定的树 t：

```text
   4
  / \
 1   2
```

返回 true，因为 t 与 s 的一个子树拥有相同的结构和节点值。

示例 2:
给定的树 s：

```text
     3
    / \
   4   5
  / \
 1   2
    /
   0
```

给定的树 t：

```text
   4
  / \
 1   2
```

返回 false。

__思路__:

参考[LeetCode #100 Same Tree 相同的树](https://www.jianshu.com/p/bd2076be1764)
对 s的每一个结点调用一次是否与 t相同
时间复杂度O(n ^ 2), 空间复杂度O(n)

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
    bool isSubtree(TreeNode* s, TreeNode* t) 
    {
        stack<TreeNode*> st;
        st.push(s);
        while (st.size()) 
        {
            TreeNode* cur = st.top();
            st.pop();
            if (isSame(cur, t)) return true;
            if (cur -> left) st.push(cur -> left);
            if (cur -> right) st.push(cur -> right);
        }
        return false;
    }
private:
    bool isSame(TreeNode* s, TreeNode* t) 
    {
        stack<TreeNode*> p, q;
        p.push(s);
        q.push(t);
        while (p.size() or q.size()) 
        {
            TreeNode* i = p.top();
            p.pop();
            TreeNode* j = q.top();
            q.pop();
            if (!i and !j) continue;
            if (!i or !j) return false;
            if (i -> val != j -> val) return false;
            else 
            {
                p.push(i -> left);
                p.push(i -> right);
                q.push(j -> left);
                q.push(j -> right);
            }
        }
        return true;
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
    public boolean isSubtree(TreeNode s, TreeNode t) {
        if (s == null && t == null) return true;
        if (s == null || t == null) return false;
        return isSame(s, t) || isSubtree(s.left, t) || isSubtree(s.right, t);
    }

    private boolean isSame(TreeNode s, TreeNode t) {
        if (s == null && t == null) return true;
        if (s == null || t == null) return false;
        return s.val == t.val && isSame(s.left, t.left) && isSame(s.right, t.right);
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
    def isSubtree(self, s: TreeNode, t: TreeNode) -> bool:
        if not s and not t:
            return True
        if not s or not t:
            return False
        def isSame(s: TreeNode, t: TreeNode) -> bool:
            if not s and not t:
                return True
            if not s or not t:
                return False
            return s.val == t.val and isSame(s.left, t.left) and isSame(s.right, t.right)
        return isSame(s, t) or self.isSubtree(s.left, t) or self.isSubtree(s.right, t)
```
