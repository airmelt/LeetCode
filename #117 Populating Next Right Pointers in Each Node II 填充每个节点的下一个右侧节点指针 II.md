__Description__:
Given a binary tree
```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

__Follow up:__

You may only use constant extra space.
Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.
 

Example 1:
![binary tree](https://upload-images.jianshu.io/upload_images/16639143-7cdfba59149ec722.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: root = [1,2,3,4,5,null,7]
Output: [1,#,2,3,#,4,5,7,#]
Explanation: Given the above binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
 

__Constraints:__

The number of nodes in the given tree is less than 6000.
-100 <= node.val <= 100

__题目描述__:
给定一个二叉树
```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。

初始状态下，所有 next 指针都被设置为 NULL。

__进阶：__

你只能使用常量级额外空间。
使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

__示例 :__
![二叉树](https://upload-images.jianshu.io/upload_images/16639143-21f401ef0af9a2c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入：root = [1,2,3,4,5,null,7]
输出：[1,#,2,3,#,4,5,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。

__提示：__

树中的节点数小于 6000
-100 <= node.val <= 100

__思路__:
1. 递归法
左子树的下一个是右子树或者由根节点的 next指针给定
右子树的下一个由根节点的 next指针给定
根节点的 next不是叶节点, 就返回左右子树不为空的那部分
递归必须先递归右子树, 否则会断链
2. 迭代法
类似层序遍历, 每次将下一层的 next指针连接好, 设置一个指针指向每一层的第一个节点
每一层遍历时, 按 next指针移动
next指针更新方式与递归法相同
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution 
{
public:
    Node* connect(Node* root) 
    {
        if (!root) return root;
        if (root -> left)
        {
            if (root -> right) root -> left -> next = root -> right;
            else root -> left -> next = helper(root -> next);
        }
        if (root -> right) root -> right -> next = helper(root -> next);
        connect(root -> right);
        connect(root -> left);
        return root;
    }
private:
    Node* helper(Node* root)
    {
        while (root)
        {
            if (root -> left) return root -> left;
            if (root -> right) return root -> right;
            root = root -> next;
        }
        return nullptr;
    }
};
```

__Java__:
```Java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/

class Solution {
    public Node connect(Node root) {
        if (root == null) return root;
        if (root.left != null) {
            if (root.right != null) root.left.next = root.right;
            else root.left.next = helper(root.next);
        }
        if (root.right != null) root.right.next = helper(root.next);
        connect(root.right);
        connect(root.left);
        return root;
    }
    
    private Node helper(Node root) {
        while (root != null) {
            if (root.left != null) return root.left;
            if (root.right != null) return root.right;
            root = root.next;
        }
        return null;
    }
}
```

__Python__:
```Python
"""
# Definition for a Node.
class Node:
    def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""

class Solution:
    def connect(self, root: 'Node') -> 'Node':
        node = root
        while node:
            head, last = None, None
            while node:
                for child in (node.left, node.right):
                    if not child: 
                        continue
                    if not head: 
                        head = child
                    if last: 
                        last.next = child
                    last = child
                node = node.next
            node = head
        return root
```