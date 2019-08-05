__Description__:
Given a n-ary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

__Example:__
For example, given a 3-ary tree:
![3-ary tree](https://upload-images.jianshu.io/upload_images/16639143-caf24b6dedfe7889.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

We should return its max depth, which is 3.

__Note:__

The depth of the tree is at most 1000.
The total number of nodes is at most 5000.

__题目描述__:
给定一个 N 叉树，找到其最大深度。

最大深度是指从根节点到最远叶子节点的最长路径上的节点总数。

__示例 :__
例如，给定一个 3叉树 :
![3叉树](https://upload-images.jianshu.io/upload_images/16639143-caf24b6dedfe7889.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们应返回其最大深度，3。

__说明:__

树的深度不会超过 1000。
树的节点总不会超过 5000。

__思路__:
使用层序遍历
1. 迭代
2. 递归
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```
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
class Solution {
public:
    int maxDepth(Node* root) {
        if (!root) return 0;
        queue<Node*> q;
        q.push(root);
        int result = 0;
        while (q.size()) {
            result++;
            int total = q.size();
            while (total--) {
                Node* cur = q.front();
                q.pop();
                for (int i = 0; i < cur -> children.size(); i++) q.push(cur -> children[i]);
            }
        }
        return result;
    }
};
```

__Java__:
```
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
    public int maxDepth(Node root) {
        if (root == null) return 0;
        int result = 0;
        for (Node node : root.children) {
            int depth = maxDepth(node);
            result = result > depth ? result : depth;
        }
        return result + 1;
    }
}
```

__Python__:
```
"""
# Definition for a Node.
class Node:
    def __init__(self, val, children):
        self.val = val
        self.children = children
"""
class Solution:
    def maxDepth(self, root: 'Node') -> int:
        if not root:
            return 0
        result = 0
        for item in root.children:
            depth = self.maxDepth(item)
            result = max(depth, result)
        return result + 1
```
