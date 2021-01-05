# 590 N-ary Tree Postorder Traversal N叉树的后序遍历

__Description__:
Given an n-ary tree, return the postorder traversal of its nodes' values.

__Example:__

For example, given a 3-ary tree:
![3-ary tree](https://upload-images.jianshu.io/upload_images/16639143-caf24b6dedfe7889.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Return its postorder traversal as: [5,6,3,2,4,1].

__Note:__

Recursive solution is trivial, could you do it iteratively?

__题目描述__:
给定一个 N 叉树，返回其节点值的后序遍历。

__示例 :__

例如，给定一个 3叉树 :
![3叉树](https://upload-images.jianshu.io/upload_images/16639143-caf24b6dedfe7889.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
返回其后序遍历: [5,6,3,2,4,1]。

__说明:__
递归法很简单，你可以使用迭代法完成此题吗?

__思路__:

1. 递归
2. 迭代, 利用栈, 插入结点的孩子结点

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
    vector<int> postorder(Node* root) 
    {
        vector<int> result;
        if (!root) return result;
        stack<Node*> s;
        s.push(root);
        while (s.size()) 
        {
            auto cur = s.top();
            s.pop();
            result.insert(result.begin(), cur -> val);
            for (auto temp : cur -> children) s.push(temp);
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
    public List<Integer> postorder(Node root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;
        for (Node node : root.children) result.addAll(postorder(node));
        result.add(root.val);
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
    def postorder(self, root: 'Node') -> List[int]:
        result = []
        def helper(root: 'Node') -> None:
            if not root:
                return
            for item in root.children:
                helper(item)
            result.append(root.val)
        helper(root)
        return result
```
