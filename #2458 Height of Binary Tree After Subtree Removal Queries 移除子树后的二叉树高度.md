# 2458 Height of Binary Tree After Subtree Removal Queries 移除子树后的二叉树高度

__Description:__

You are given the `root` of a __binary tree__ with `n` nodes. Each node is assigned a unique value from `1` to `n`. You are also given an array `queries` of size `m`.

You have to perform `m` __independent__ queries on the tree where in the `i ^ th` query you do the following:

- __Remove__ the subtree rooted at the node with the value `queries[i]` from the tree. It is __guaranteed__ that `queries[i]` will __not__ be equal to the value of the root.

Return _an array_ `answer` _of size_ `m` _where_ `answer[i]` _is the height of the tree after performing the_ `i ^ th` _query_.

Note:

- The queries are independent, so the tree returns to its __initial__ state after each query.
- The height of a tree is the __number of edges in the longest simple path__ from the root to some node in the tree.

__Example:__

Example 1:

![2458-1](https://assets.leetcode.com/uploads/2022/09/07/binaryytreeedrawio-1.png)

```text
Input: root = [1,3,4,2,null,6,5,null,null,null,null,null,7], queries = [4]
Output: [2]
Explanation: The diagram above shows the tree after removing the subtree rooted at node with value 4.
The height of the tree is 2 (The path 1 -> 3 -> 2).
```

Example 2:

![2458-2](https://assets.leetcode.com/uploads/2022/09/07/binaryytreeedrawio-2.png)

```text
Input: root = [5,8,9,2,1,3,7,4,6], queries = [3,2,4,8]
Output: [3,2,3,2]
Explanation: We have the following queries:
```

- Removing the subtree rooted at node with value 3. The height of the tree becomes 3 (The path 5 -> 8 -> 2 -> 4).
- Removing the subtree rooted at node with value 2. The height of the tree becomes 2 (The path 5 -> 8 -> 1).
- Removing the subtree rooted at node with value 4. The height of the tree becomes 3 (The path 5 -> 8 -> 2 -> 6).
- Removing the subtree rooted at node with value 8. The height of the tree becomes 2 (The path 5 -> 9 -> 3).

__Constraints:__

- The number of nodes in the tree is `n`.
- `2 <= n <= 10 ^ 5`
- `1 <= Node.val <= n`
- All the values in the tree are __unique__.
- `m == queries.length`
- `1 <= m <= min(n, 10 ^ 4)`
- `1 <= queries[i] <= n`
- `queries[i] != root.val`

__题目描述:__

给你一棵 __二叉树__ 的根节点 `root` ，树中有 `n` 个节点。每个节点都可以被分配一个从 `1` 到 `n` 且互不相同的值。另给你一个长度为 `m` 的数组 `queries` 。

你必须在树上执行 `m` 个 __独立__ 的查询，其中第 `i` 个查询你需要执行以下操作:

- 从树中 __移除__ 以 `queries[i]` 的值作为根节点的子树。题目所用测试用例保证 `queries[i]` __不__ 等于根节点的值。

返回一个长度为 `m` 的数组 `answer` ，其中 `answer[i]` 是执行第 `i` 个查询后树的高度。

注意：

- 查询之间是独立的，所以在每个查询执行后，树会回到其 __初始__ 状态。
- 树的高度是从根到树中某个节点的 __最长简单路径中的边数__ 。

__示例:__

示例 1：

![2458-3](https://assets.leetcode.com/uploads/2022/09/07/binaryytreeedrawio-1.png)

```text
输入：root = [1,3,4,2,null,6,5,null,null,null,null,null,7], queries = [4]
输出：[2]
解释：上图展示了从树中移除以 4 为根节点的子树。
树的高度是 2（路径为 1 -> 3 -> 2）。
```

示例 2：

![2458-4](https://assets.leetcode.com/uploads/2022/09/07/binaryytreeedrawio-2.png)

```text
输入：root = [5,8,9,2,1,3,7,4,6], queries = [3,2,4,8]
输出：[3,2,3,2]
解释：执行下述查询：
```

- 移除以 3 为根节点的子树。树的高度变为 3（路径为 5 -> 8 -> 2 -> 4）。
- 移除以 2 为根节点的子树。树的高度变为 2（路径为 5 -> 8 -> 1）。
- 移除以 4 为根节点的子树。树的高度变为 3（路径为 5 -> 8 -> 2 -> 6）。
- 移除以 8 为根节点的子树。树的高度变为 2（路径为 5 -> 9 -> 3）。

__提示：__

- 树中节点的数目是 `n`
- `2 <= n <= 10 ^ 5`
- `1 <= Node.val <= n`
- 树中的所有值 __互不相同__
- `m == queries.length`
- `1 <= m <= min(n, 10 ^ 4)`
- `1 <= queries[i] <= n`
- `queries[i] != root.val`

__思路:__

```text
DFS ➕ 离线查询
使用哈希表 height 记录每个节点的高度
遍历树, 计算删除每个节点的时候的剩余高度 rest
当删除节点的左子树时, rest = max(rest, depth + height[node -> right])
当删除节点的右子树时, rest = max(rest, depth + height[node -> left])
其中 depth 为当前节点的深度
最后遍历 queries, 原地修改为剩余高度并返回结果
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
    vector<int> treeQueries(TreeNode* root, vector<int>& queries) 
    {
        unordered_map<TreeNode*, int> height;
        function<int(TreeNode*)> get_height = [&](TreeNode* node) -> int
        {
            if (!node) return 0;
            return height[node] = 1 + max(get_height(node -> left), get_height(node -> right));
        };
        get_height(root);
        vector<int> remain(height.size() + 1);
        function<void(TreeNode*, int, int)> dfs = [&](TreeNode* node, int depth, int rest) 
        {
            if (!node) return;
            ++depth;
            remain[node -> val] = rest;
            dfs(node -> left, depth, max(rest, depth + height[node -> right]));
            dfs(node -> right, depth, max(rest, depth + height[node -> left]));
        };
        dfs(root, -1, 0);
        for (int i = 0, n = queries.size(); i < n; i++) queries[i] = remain[queries[i]];
        return queries;
    }
};

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
    private int[] remain;
    Map<TreeNode, Integer> height = new HashMap<TreeNode, Integer>();

    public int[] treeQueries(TreeNode root, int[] queries) {
        getHeight(root);
        remain = new int[height.size() + 1];
        dfs(root, -1, 0);
        for (int i = 0, n = queries.length; i < n; i++) queries[i] = remain[queries[i]];
        return queries;
    }

    private int getHeight(TreeNode node) {
        if (node == null) return 0;
        height.merge(node, 1 + Math.max(getHeight(node.left), getHeight(node.right)), Integer::sum);
        return height.get(node);
    }

    private void dfs(TreeNode node, int depth, int rest) {
        if (node == null) return;
        ++depth;
        remain[node.val] = rest;
        dfs(node.left, depth, Math.max(rest, depth + height.getOrDefault(node.right, 0)));
        dfs(node.right, depth, Math.max(rest, depth + height.getOrDefault(node.left, 0)));
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
    def treeQueries(self, root: Optional[TreeNode], queries: List[int]) -> List[int]:
        height = defaultdict(int)
        def get_height(node: Optional[TreeNode]) -> int:
            if node is None: 
                return 0
            height[node] = 1 + max(get_height(node.left), get_height(node.right))
            return height[node]
        get_height(root)

        remain = [0] * (len(height) + 1)
        def dfs(node: Optional[TreeNode], depth: int, rest: int) -> None:
            if node is None: 
                return
            depth += 1
            remain[node.val] = rest
            dfs(node.left, depth, max(rest, depth + height[node.right]))
            dfs(node.right, depth, max(rest, depth + height[node.left]))
        dfs(root, -1, 0)
        return [remain[q] for q in queries]
```
