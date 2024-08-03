# 2196 Create Binary Tree From Descriptions 根据描述创建二叉树

__Description:__

You are given a 2D integer array `descriptions` where `descriptions[i] = [parenti, childi, isLefti]` indicates that `parenti` is the __parent__ of `childi` in a __binary__ tree of __unique__ values. Furthermore,

- If `isLefti == 1`, then `childi` is the left child of `parenti`.
- If `isLefti == 0`, then `childi` is the right child of `parenti`.

Construct the binary tree described by `descriptions` and return _its __root___.

The test cases will be generated such that the binary tree is valid.

__Example:__

Example 1:

![2196-1](https://assets.leetcode.com/uploads/2022/02/09/example1drawio.png)

```text
Input: descriptions = [[20,15,1],[20,17,0],[50,20,1],[50,80,0],[80,19,1]]
Output: [50,20,80,15,17,19]
Explanation: The root node is the node with value 50 since it has no parent.
The resulting binary tree is shown in the diagram.
```

Example 2:

![2196-2](https://assets.leetcode.com/uploads/2022/02/09/example2drawio.png)

```text
Input: descriptions = [[1,2,1],[2,3,0],[3,4,1]]
Output: [1,2,null,null,3,4]
Explanation: The root node is the node with value 1 since it has no parent.
The resulting binary tree is shown in the diagram.
```

__Constraints:__

- `1 <= descriptions.length <= 10 ^ 4`
- `descriptions[i].length == 3`
- `1 <= parenti, childi <= 10 ^ 5`
- `0 <= isLefti <= 1`
- The binary tree described by `descriptions` is valid.

__题目描述:__

给你一个二维整数数组 `descriptions` ，其中 `descriptions[i] = [parenti, childi, isLefti]` 表示 `parenti` 是 `childi` 在 __二叉树__ 中的 __父节点__，二叉树中各节点的值 __互不相同__ 。此外:

- 如果 `isLefti == 1` ，那么 `childi` 就是 `parenti` 的左子节点。
- 如果 `isLefti == 0` ，那么 `childi` 就是 `parenti` 的右子节点。

请你根据 `descriptions` 的描述来构造二叉树并返回其 __根节点__ 。

测试用例会保证可以构造出 有效 的二叉树。

__示例:__

示例 1：

![2196-3](https://assets.leetcode.com/uploads/2022/02/09/example1drawio.png)

```text
输入：descriptions = [[20,15,1],[20,17,0],[50,20,1],[50,80,0],[80,19,1]]
输出：[50,20,80,15,17,19]
解释：根节点是值为 50 的节点，因为它没有父节点。
结果二叉树如上图所示。
```

示例 2：

![2196-4](https://assets.leetcode.com/uploads/2022/02/09/example2drawio.png)

```text
输入：descriptions = [[1,2,1],[2,3,0],[3,4,1]]
输出：[1,2,null,null,3,4]
解释：根节点是值为 1 的节点，因为它没有父节点。 
结果二叉树如上图所示。
```

__提示：__

- `1 <= descriptions.length <= 10 ^ 4`
- `descriptions[i].length == 3`
- `1 <= parenti, childi <= 10 ^ 5`
- `0 <= isLefti <= 1`
- `descriptions` 所描述的二叉树是一棵有效二叉树

__思路:__

```text
模拟
按照题意将节点放入哈希表
并组装子树
同时记录每个节点的入度
最后入度为 0 的节点就是根节点
返回根节点
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

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
    TreeNode* createBinaryTree(vector<vector<int>>& descriptions) 
    {
        map<int, TreeNode*> nodes;
        map<int, int> indegree;
        for (const auto& d : descriptions) 
        {
            int u = d[0], v = d[1], p = d[2];
            if (!nodes.count(u)) nodes[u] = new TreeNode(u);
            if (!nodes.count(v)) nodes[v] = new TreeNode(v);
            if (p) nodes[u] -> left = nodes[v];
            else nodes[u] -> right = nodes[v];
            ++indegree[v];
        }
        for (const auto& [node, root] : nodes) if (!indegree[node]) return root;
        return nullptr;
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
    public TreeNode createBinaryTree(int[][] descriptions) {
        Map<Integer, TreeNode> nodes = new HashMap<>();
        Map<Integer, Integer> indegree = new HashMap<>();
        for (var d : descriptions) {
            int u = d[0], v = d[1], p = d[2];
            if (!nodes.containsKey(u)) nodes.put(u, new TreeNode(u));
            if (!nodes.containsKey(v)) nodes.put(v, new TreeNode(v));
            if (p == 1) nodes.get(u).left = nodes.get(v);
            else nodes.get(u).right = nodes.get(v);
            indegree.merge(v, 1, Integer::sum);
        }
        for (var node : nodes.keySet()) if (indegree.getOrDefault(node, 0) == 0) return nodes.get(node);
        return null;
    }
}
```

__Python__:

```Python
class Solution:
    def createBinaryTree(self, descriptions: List[List[int]]) -> Optional[TreeNode]:
        nodes, vals = defaultdict(TreeNode), set()
        for x, y, left in descriptions:
            if left:
                nodes[x].left = nodes[y]
            else:
                nodes[x].right = nodes[y]
            vals.add(y)
        for v, node in nodes.items():
            node.val = v
        return next(node for v, node in nodes.items() if v not in vals)
```
