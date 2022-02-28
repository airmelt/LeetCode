# 1028 Recover a Tree From Preorder Traversal 从先序遍历还原二叉树

__Description__:
We run a preorder depth-first search (DFS) on the root of a binary tree.

At each node in this traversal, we output D dashes (where D is the depth of this node), then we output the value of this node.  If the depth of a node is D, the depth of its immediate child is D + 1.  The depth of the root node is 0.

If a node has only one child, that child is guaranteed to be the left child.

Given the output traversal of this traversal, recover the tree and return its root.

__Example:__

Example 1:

![Tree 1](https://assets.leetcode.com/uploads/2019/04/08/recover-a-tree-from-preorder-traversal.png)

Input: traversal = "1-2--3--4-5--6--7"
Output: [1,2,5,3,4,6,7]

Example 2:

![Tree 2](https://assets.leetcode.com/uploads/2019/04/11/screen-shot-2019-04-10-at-114101-pm.png)

Input: traversal = "1-2--3---4-5--6---7"
Output: [1,2,5,3,null,6,null,4,null,7]

Example 3:

![Tree 3](https://assets.leetcode.com/uploads/2019/04/11/screen-shot-2019-04-10-at-114955-pm.png)

Input: traversal = "1-401--349---90--88"
Output: [1,401,null,349,88,90]

__Constraints:__

The number of nodes in the original tree is in the range [1, 1000].
1 <= Node.val <= 10^9

__题目描述__:
我们从二叉树的根节点 root 开始进行深度优先搜索。

在遍历中的每个节点处，我们输出 D 条短划线（其中 D 是该节点的深度），然后输出该节点的值。（如果节点的深度为 D，则其直接子节点的深度为 D + 1。根节点的深度为 0）。

如果节点只有一个子节点，那么保证该子节点为左子节点。

给出遍历输出 S，还原树并返回其根节点 root。

__示例 :__

示例 1：

![二叉树 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/04/12/recover-a-tree-from-preorder-traversal.png)

输入："1-2--3--4-5--6--7"
输出：[1,2,5,3,4,6,7]

示例 2：

![二叉树 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/04/12/screen-shot-2019-04-10-at-114101-pm.png)

输入："1-2--3---4-5--6---7"
输出：[1,2,5,3,null,6,null,4,null,7]

示例 3：

![二叉树 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/04/12/screen-shot-2019-04-10-at-114955-pm.png)

输入："1-401--349---90--88"
输出：[1,401,null,349,88,90]

__提示:__

原始树中的节点数介于 1 和 1000 之间。
每个节点的值介于 1 和 10 ^ 9 之间。

__思路__:

栈
先序遍历的顺序为根-左-右
由于父结点只有一个结点时, 保证该结点为左结点
所以当前结点要么是上一个结点的左结点要么是根到上一个结点路径上的一个结点(不包括上一个结点)的右结点
通过记录下 '-' 可以得到当前结点的深度
用栈记录下深度遍历从根到当前结点的路径上的所有结点
当当前结点为右结点时, 为了找到当前结点的父结点, 需要找到深度刚好比当前结点少 1 的结点
因为栈中记录了根结点到当前结点的所有结点, 所以可以弹出结点直到 stack.size == level
最后栈顶的元素即为根结点
时间复杂度为 O(n), 空间复杂度为 O(h), 其中 n 表示 traversal 的长度, h 为树的深度

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
    TreeNode* recoverFromPreorder(string traversal) 
    {
        stack<TreeNode*> path;
        int pos = 0, n = traversal.size();
        while (pos < n) 
        {
            int level = 0, value = 0;
            while (traversal[pos] == '-')
            {
                ++level;
                ++pos;
            }
            while (pos < n and isdigit(traversal[pos]))
            {
                value = 10 * value + (traversal[pos] - '0');
                ++pos;
            }
            TreeNode* node = new TreeNode(value);
            if (level == path.size()) 
            {
                if (!path.empty()) path.top() -> left = node;
            }
            else
            {
                while (level != path.size()) path.pop();
                path.top() -> right = node;
            }
            path.push(node);
        }
        while (path.size() != 1) path.pop();
        return path.top();
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
    public TreeNode recoverFromPreorder(String traversal) {
        Deque<TreeNode> path = new LinkedList<TreeNode>();
        int pos = 0, n = traversal.length();
        while (pos < n) {
            int level = 0, value = 0;
            while (traversal.charAt(pos) == '-') {
                ++level;
                ++pos;
            }
            while (pos < n && Character.isDigit(traversal.charAt(pos))) {
                value = value * 10 + (traversal.charAt(pos) - '0');
                ++pos;
            }
            TreeNode node = new TreeNode(value);
            if (level == path.size()) {
                if (!path.isEmpty()) path.peek().left = node;
            } else {
                while (level != path.size()) path.pop();
                path.peek().right = node;
            }
            path.push(node);
        }
        while (path.size() > 1) path.pop();
        return path.peek();
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
    def recoverFromPreorder(self, traversal: str) -> Optional[TreeNode]:
        path, pos, n = list(), 0, len(traversal)
        while pos < n:
            level = 0
            while traversal[pos] == '-':
                level += 1
                pos += 1
            value = 0
            while pos < n and traversal[pos].isdigit():
                value = value * 10 + (ord(traversal[pos]) - ord('0'))
                pos += 1
            node = TreeNode(value)
            if level == len(path):
                if path:
                    path[-1].left = node
            else:
                path = path[:level]
                path[-1].right = node
            path.append(node)
        return path[0]
```
