# 863 All Nodes Distance K in Binary Tree 二叉树中所有距离为 K 的结点

__Description__:
Given the root of a binary tree, the value of a target node target, and an integer k, return an array of the values of all nodes that have a distance k from the target node.

You can return the answer in any order.

__Example:__

Example 1:

![Binary Tree](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/28/sketch0.png)

Input: root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, k = 2
Output: [7,4,1]
Explanation: The nodes that are a distance 2 from the target node (with value 5) have values 7, 4, and 1.

Example 2:

Input: root = [1], target = 1, k = 3
Output: []

__Constraints:__

The number of nodes in the tree is in the range [1, 500].
0 <= Node.val <= 500
All the values Node.val are unique.
target is the value of one of the nodes in the tree.
0 <= k <= 1000

__题目描述__:
给定一个二叉树（具有根结点 root）， 一个目标结点 target ，和一个整数值 K 。

返回到目标结点 target 距离为 K 的所有结点的值的列表。 答案可以以任何顺序返回。

__示例 :__

示例 1：

输入：root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, K = 2
输出：[7,4,1]
解释：
所求结点为与目标结点（值为 5）距离为 2 的结点，
值分别为 7，4，以及 1

![二叉树](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/28/sketch0.png)

注意，输入的 "root" 和 "target" 实际上是树上的结点。
上面的输入仅仅是对这些对象进行了序列化描述。

__提示:__

给定的树是非空的。
树上的每个结点都具有唯一的值 0 <= node.val <= 500 。
目标结点 target 是树上的结点。
0 <= K <= 1000.

__思路__:

注意到每个结点的值是独一无二的

1. 父节点
将每个结点的父节点保存
然后用 dfs 分别查找结点的左右子结点及父节点, 注意去重
2. 图
建立图, 然后在图中进行 DFS
时间复杂度为 O(n), 空间复杂度为 O(n)

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
private:
    unordered_map<int, TreeNode*> parent;
    vector<int> result;
    
    void find_parent(TreeNode* node)
    {
        if (node -> left)
        {
            parent[node -> left -> val] = node;
            find_parent(node -> left);
        }
        if (node -> right)
        {
            parent[node -> right -> val] = node;
            find_parent(node -> right);
        }
    }
    
    void dfs(TreeNode* node, TreeNode* start, int depth, int k)
    {
        if (!node) return;
        if (depth == k)
        {
            result.emplace_back(node -> val);
            return;
        }
        if (node -> left != start) dfs(node -> left, node, depth + 1, k);
        if (node -> right != start) dfs(node -> right, node, depth + 1, k);
        if (parent[node -> val] != start) dfs(parent[node -> val], node, depth + 1, k);
    }
public:
    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) 
    {
        find_parent(root);
        dfs(target, nullptr, 0, k);
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
    private Map<Integer, TreeNode> parent;
    private List<Integer> result;
    
    public List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
        parent = new HashMap<Integer, TreeNode>();
        result = new ArrayList<Integer>();
        findParent(root);
        dfs(target, null, 0, k);
        return result;
    }
    
    private void findParent(TreeNode node) {
        if (node.left != null) {
            parent.put(node.left.val, node);
            findParent(node.left);
        }
        if (node.right != null) {
            parent.put(node.right.val, node);
            findParent(node.right);
        }
    }
    
    private void dfs(TreeNode node, TreeNode start, int depth, int k) {
        if (node == null) return;
        if (depth == k) {
            result.add(node.val);
            return;
        }
        if (node.left != start) dfs(node.left, node, depth + 1, k);
        if (node.right != start) dfs(node.right, node, depth + 1, k);
        if (parent.get(node.val) != start) dfs(parent.get(node.val), node, depth + 1, k);
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
    def distanceK(self, root: TreeNode, target: TreeNode, k: int) -> List[int]:
        graph, cur, visited = defaultdict(set), [target.val], { target.val }
        
        def dfs(root: TreeNode) -> None:
            (root.left and (graph[root.val].add(root.left.val) or graph[root.left.val].add(root.val) or dfs(root.left))) or (root.right and (graph[root.val].add(root.right.val) or graph[root.right.val].add(root.val) or dfs(root.right)))

        dfs(root)
        while (k := k - 1) > -1:
            next_time = []
            for next_node in cur:
                for node in graph[next_node]:
                    node not in visited and (visited.add(node) or next_time.append(node))
            cur = next_time
        return cur
```
