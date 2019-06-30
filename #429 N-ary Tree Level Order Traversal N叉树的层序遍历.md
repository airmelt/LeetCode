__Description__:
Given an n-ary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example, given a 3-ary tree:
![3-ary tree](https://upload-images.jianshu.io/upload_images/16639143-e48a64a29deee174.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

We should return its level order traversal:
```
[
     [1],
     [3,2,4],
     [5,6]
]
 ```

__Note:__

The depth of the tree is at most 1000.
The total number of nodes is at most 5000.

__题目描述__:
给定一个 N 叉树，返回其节点值的层序遍历。 (即从左到右，逐层遍历)。

例如，给定一个 3叉树 :
![3叉树](https://upload-images.jianshu.io/upload_images/16639143-e48a64a29deee174.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

返回其层序遍历:
```
[
     [1],
     [3,2,4],
     [5,6]
]
```

__思路__:
参考[LeetCode #107 Binary Tree Level Order Traversal II 二叉树的层次遍历 II](https://www.jianshu.com/p/76abc4ff072f)
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
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> result;
        if (!root) return result;
        queue<Node*> q;
        q.push(root);
        while (!q.empty()) {
            vector<int> temp;
            int s = q.size();
            for (int i = 0; i < s; i++) {
                Node* cur = q.front();
                temp.push_back(cur -> val);
                for (int j = 0; j < cur -> children.size(); j++) q.push(cur -> children[j]);
                q.pop();
            }
            result.push_back(temp);
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
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            List<Integer> temp = new ArrayList<>();
            int len = queue.size();
            for (int i = 0; i < len; i++) {
                Node t = queue.poll();
                temp.add(t.val);
                for (int j = 0; j < t.children.size(); j++) queue.add(t.children.get(j));
            }
            result.add(temp);
        }
        return result;
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
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        result = []
        def bfs(root, result, n=0):
            if root:
                try:
                    result[n].append(root.val)
                except IndexError:
                    result.append([root.val])
                for i in root.children:
                    bfs(i, result, n + 1)
        bfs(root, result)
        return result
```
