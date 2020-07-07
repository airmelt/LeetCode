__Description__:
Given a reference of a node in a connected undirected graph.

Return a deep copy (clone) of the graph.

Each node in the graph contains a val (int) and a list (List[Node]) of its neighbors.
```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

Test case format:

For simplicity sake, each node's value is the same as the node's index (1-indexed). For example, the first node with val = 1, the second node with val = 2, and so on. The graph is represented in the test case using an adjacency list.

Adjacency list is a collection of unordered lists used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with val = 1. You must return the copy of the given node as a reference to the cloned graph.

__Example:__
Example 1:
![graph 1](https://upload-images.jianshu.io/upload_images/16639143-2ca9ab1c2ebf627e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).

Example 2:
![graph 2](https://upload-images.jianshu.io/upload_images/16639143-b7bfbeffec7240bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: adjList = [[]]
Output: [[]]
Explanation: Note that the input contains one empty list. The graph consists of only one node with val = 1 and it does not have any neighbors.

Example 3:
Input: adjList = []
Output: []
Explanation: This an empty graph, it does not have any nodes.

Example 4:
![graph 4](https://upload-images.jianshu.io/upload_images/16639143-0c43d095d7f355a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: adjList = [[2],[1]]
Output: [[2],[1]]
 

__Constraints:__

1 <= Node.val <= 100
Node.val is unique for each node.
Number of Nodes will not exceed 100.
There is no repeated edges and no self-loops in the graph.
The Graph is connected and all nodes can be visited starting from the given node.

__题目描述__:
给你无向 连通 图中一个节点的引用，请你返回该图的 深拷贝（克隆）。

图中的每个节点都包含它的值 val（int） 和其邻居的列表（list[Node]）。
```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

测试用例格式：

简单起见，每个节点的值都和它的索引相同。例如，第一个节点值为 1（val = 1），第二个节点值为 2（val = 2），以此类推。该图在测试用例中使用邻接列表表示。

邻接列表 是用于表示有限图的无序列表的集合。每个列表都描述了图中节点的邻居集。

给定节点将始终是图中的第一个节点（值为 1）。你必须将 给定节点的拷贝 作为对克隆图的引用返回。

__示例 :__
示例 1：
![图 1](https://upload-images.jianshu.io/upload_images/16639143-d71dbe7d0aee8db3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入：adjList = [[2,4],[1,3],[2,4],[1,3]]
输出：[[2,4],[1,3],[2,4],[1,3]]
解释：
图中有 4 个节点。
节点 1 的值是 1，它有两个邻居：节点 2 和 4 。
节点 2 的值是 2，它有两个邻居：节点 1 和 3 。
节点 3 的值是 3，它有两个邻居：节点 2 和 4 。
节点 4 的值是 4，它有两个邻居：节点 1 和 3 。

示例 2：
![图 2](https://upload-images.jianshu.io/upload_images/16639143-1903cdd899b402d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入：adjList = [[]]
输出：[[]]
解释：输入包含一个空列表。该图仅仅只有一个值为 1 的节点，它没有任何邻居。

示例 3：
输入：adjList = []
输出：[]
解释：这个图是空的，它不含任何节点。

示例 4：
![图 4](https://upload-images.jianshu.io/upload_images/16639143-e1df3fc59fdc5d14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入：adjList = [[2],[1]]
输出：[[2],[1]]

__提示：__

节点数不超过 100 。
每个节点值 Node.val 都是唯一的，1 <= Node.val <= 100。
无向图是一个简单图，这意味着图中没有重复的边，也没有自环。
由于图是无向的，如果节点 p 是节点 q 的邻居，那么节点 q 也必须是节点 p 的邻居。
图是连通图，你可以从给定节点访问到所有节点。

__思路__:
1. 递归法
2. 迭代法(队列)
用一个 visited数组记录已经访问过的节点
重新建立每一个节点
遍历节点的 neighbors数组, 建立节点之间的关系
时间复杂度O(n ^ 2), 空间复杂度O(n)

__代码__:
__C++__:
```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;
    
    Node() {
        val = 0;
        neighbors = vector<Node*>();
    }
    
    Node(int _val) {
        val = _val;
        neighbors = vector<Node*>();
    }
    
    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/

class Solution 
{
public:
    Node* cloneGraph(Node* node) 
    {
        if (!node) return node;
        Node* visited[101]{nullptr};
        queue<Node*> q;
        q.push(node);
        visited[node -> val] = new Node(node -> val, vector<Node*>{});
        while (!q.empty())
        {
            Node* cur = q.front();
            q.pop();
            for (auto neighbor : cur -> neighbors)
            {
                if (!visited[neighbor -> val])
                {
                    visited[neighbor -> val] = new Node(neighbor -> val, vector<Node*>{});
                    q.push(neighbor);
                }
                visited[cur -> val] -> neighbors.push_back(visited[neighbor -> val]);
            }
        }
        return visited[node -> val];
    }
};
```

__Java__:
```Java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    private Node visited[] = new Node[101];
    
    public Node cloneGraph(Node node) {
        if (node == null) return node;
        Node root = new Node(node.val, new ArrayList<Node>());
        visited[node.val] = root;
        for (int i = 0; i < node.neighbors.size(); i++) {
            if (visited[node.neighbors.get(i).val] != null) root.neighbors.add(visited[node.neighbors.get(i).val]);
            else root.neighbors.add(cloneGraph(node.neighbors.get(i)));
        }
        return root;
    }
}
```

__Python__:
```Python
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = []):
        self.val = val
        self.neighbors = neighbors
"""

class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':
        return copy.deepcopy(node)
```