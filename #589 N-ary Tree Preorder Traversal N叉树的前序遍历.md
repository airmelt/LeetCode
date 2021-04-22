# 589 N-ary Tree Preorder Traversal N叉树的前序遍历

__Description__:
Given an n-ary tree, return the preorder traversal of its nodes' values.

__Example:__

For example, given a 3-ary tree:

![3-ary tree](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

Return its preorder traversal as: [1,3,5,6,2,4].

__Note:__

Recursive solution is trivial, could you do it iteratively?

__题目描述__:
给定一个 N 叉树，返回其节点值的前序遍历。

__示例 :__

例如，给定一个 3叉树 :

![3叉树](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

返回其前序遍历: [1,3,5,6,2,4]。

__说明:__
递归法很简单，你可以使用迭代法完成此题吗?

__思路__:

1. 递归
2. 迭代, 利用栈, 反向插入结点的孩子结点

时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/
class Solution 
{
public:
    vector<int> preorder(Node* root) 
    {
        vector<int> result ;
        if (!root) return result;
        stack<Node*> s;
        s.push(root);
        while (s.size()) 
        {       
            auto cur = s.top();
            s.pop();
            result.push_back(cur -> val);
            vector<int>::reverse_iterator temp;
            // std::reverse_iterator 是一个反转给定迭代器方向的迭代器适配器
            for (auto temp = (cur -> children).rbegin(); temp != (cur -> children).rend(); temp++) s.push(*temp);
        }
        return result;
    }
};
```

__Java__:

```Java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val,List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/
class Solution {
    public List<Integer> preorder(Node root) {
        List<Integer> result = new LinkedList<>();
        if (root == null)
            return result;
        Stack<Node> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            root = stack.pop();
            result.add(root.val);
            for (int i = root.children.size() - 1; i >= 0; i--) stack.push(root.children.get(i));
        }
        return result;
    }
}
```

__Python__:

```Python
"""
# Definition for a Node.
class Node:
    def __init__(self, val, children):
        self.val = val
        self.children = children
"""
class Solution:
    def preorder(self, root: 'Node') -> List[int]:
        result = []
        def dfs(root: 'Node') -> None:
            if not root:
                return
            result.append(root.val)
            for item in root.children:
                dfs(item)
        dfs(root)
        return result
```
