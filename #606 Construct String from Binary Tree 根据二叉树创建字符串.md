# 606 Construct String from Binary Tree 根据二叉树创建字符串

__Description__:
You need to construct a string consists of parenthesis and integers from a binary tree with the preorder traversing way.

The null node needs to be represented by empty parenthesis pair "()". And you need to omit all the empty parenthesis pairs that don't affect the one-to-one mapping relationship between the string and the original binary tree.

__Example:__

Example 1:
Input: Binary tree: [1,2,3,4]

```text
       1
     /   \
    2     3
   /
  4
```

Output: "1(2(4))(3)"

Explanation: Originallay it needs to be "1(2(4)())(3()())",
but you need to omit all the unnecessary empty parenthesis pairs.
And it will be "1(2(4))(3)".

Example 2:
Input: Binary tree: [1,2,3,null,4]

```text
       1
     /   \
    2     3
     \
      4
```

Output: "1(2()(4))(3)"

Explanation: Almost the same as the first example,
except we can't omit the first parenthesis pair to break the one-to-one mapping relationship between the input and the output.

__题目描述__:
你需要采用前序遍历的方式，将一个二叉树转换成一个由括号和整数组成的字符串。

空节点则用一对空括号 "()" 表示。而且你需要省略所有不影响字符串与原始二叉树之间的一对一映射关系的空括号对。

__示例 :__

示例 1:

输入: 二叉树: [1,2,3,4]

```text
       1
     /   \
    2     3
   /
  4
```

输出: "1(2(4))(3)"

解释: 原本将是“1(2(4)())(3())”，
在你省略所有不必要的空括号对之后，
它将是“1(2(4))(3)”。

示例 2:

输入: 二叉树: [1,2,3,null,4]

```text
       1
     /   \
    2     3
     \
      4
```

输出: "1(2()(4))(3)"

解释: 和第一个示例相似，
除了我们不能省略第一个对括号来中断输入和输出之间的一对一映射关系。

__思路__:

1. 递归, 用先序遍历的思想访问每一个结点并加上括号
2. 迭代, 存下每个结点和每个结点的深度, 深度用来给最后的结点加上右括号
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
    string tree2str(TreeNode* t) 
    {
        string result = "";
        int level = 0;
        if (!t) return result;
        stack<TreeNode*> s;
        stack<int> l;
        s.push(t);
        l.push(level);
        while (s.size()) 
        {
            TreeNode* cur = s.top();
            s.pop();
            level = l.top();
            l.pop();
            if (level > 0) result += "(";
            if (cur) result += to_string(cur -> val);
            else 
            {
                result += ")";
                continue;
            }
            if (cur -> left or cur -> right) 
            {
                if (cur -> right) 
                {
                    s.push(cur -> right);
                    l.push(level + 1);
                }
                s.push(cur -> left);
                l.push(level + 1);
            } 
            else 
            {
                if (s.size()) level -= l.top() - 1;
                while (level--) result += ")";
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
    private StringBuilder sb = new StringBuilder();

    public String tree2str(TreeNode t) {
        preOrder(t);
        return sb.toString();
    }

    private void preOrder(TreeNode t) {
        if (t == null) return;
        sb.append(t.val);
        if (t.left != null || t.right != null) {
            sb.append("(");
            preOrder(t.left);
            sb.append(")");
            if (t.right != null) {
                sb.append("(");
                preOrder(t.right);
                sb.append(")");
            }
        }
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
    def tree2str(self, t: TreeNode) -> str:
        result = ""
        if not t:
            return result
        result += str(t.val)
        if t.left or t.right:
            result += "(" + self.tree2str(t.left) + ")"
            if t.right:
                result += "(" + self.tree2str(t.right) + ")"
        return result
```
