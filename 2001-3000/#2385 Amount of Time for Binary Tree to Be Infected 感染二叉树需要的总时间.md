# 2385 Amount of Time for Binary Tree to Be Infected 感染二叉树需要的总时间

__Description:__

You are given the `root` of a binary tree with __unique__ values, and an integer `start`. At minute `0`, an __infection__ starts from the node with value `start`.

Each minute, a node becomes infected if:

- The node is currently uninfected.
- The node is adjacent to an infected node.

Return the number of minutes needed for the entire tree to be infected.

__Example:__

Example 1:

![2385-1](https://assets.leetcode.com/uploads/2022/06/25/image-20220625231744-1.png)

```text
Input: root = [1,5,3,null,4,10,6,9,2], start = 3
Output: 4
Explanation: The following nodes are infected during:
```

- Minute 0: Node 3
- Minute 1: Nodes 1, 10 and 6
- Minute 2: Node 5
- Minute 3: Node 4
- Minute 4: Nodes 9 and 2

It takes 4 minutes for the whole tree to be infected so we return 4.

Example 2:

![2385-2](https://assets.leetcode.com/uploads/2022/06/25/image-20220625231812-2.png)

```text
Input: root = [1], start = 1
Output: 0
Explanation: At minute 0, the only node in the tree is infected so we return 0.
```

__Constraints:__

- The number of nodes in the tree is in the range `[1, 10 ^ 5]`.
- `1 <= Node.val <= 10 ^ 5`
- Each node has a __unique__ value.
- A node with a value of `start` exists in the tree.

__题目描述:__

给你一棵二叉树的根节点 `root` ，二叉树中节点的值 __互不相同__ 。另给你一个整数 `start` 。在第 `0` 分钟，__感染__ 将会从值为 `start` 的节点开始爆发。

每分钟，如果节点满足以下全部条件，就会被感染：

- 节点此前还没有感染。
- 节点与一个已感染节点相邻。

返回感染整棵树需要的分钟数。

__示例:__

示例 1：

![2385-3](https://assets.leetcode.com/uploads/2022/06/25/image-20220625231744-1.png)

```text
输入：root = [1,5,3,null,4,10,6,9,2], start = 3
输出：4
解释：节点按以下过程被感染：
```

- 第 0 分钟：节点 3
- 第 1 分钟：节点 1、10、6
- 第 2 分钟：节点5
- 第 3 分钟：节点 4
- 第 4 分钟：节点 9 和 2

感染整棵树需要 4 分钟，所以返回 4 。

示例 2：

![2385-4](https://assets.leetcode.com/uploads/2022/06/25/image-20220625231812-2.png)

```text
输入：root = [1], start = 1
输出：0
解释：第 0 分钟，树中唯一一个节点处于感染状态，返回 0 。
```

__提示：__

- 树中节点的数目在范围 `[1, 10 ^ 5]` 内
- `1 <= Node.val <= 10 ^ 5`
- 每个节点的值 __互不相同__
- 树中必定存在值为 `start` 的节点

__思路:__

```text
1. 两次遍历
先利用 DFS 将树转换为邻接表(图)
再利用 BFS 计算感染整棵树需要的分钟数
时间复杂度为 O(N), 空间复杂度为 O(N)
2. 一次遍历
如果根节点就是 start, 实际上转换为求树的直径
当 start 不是根节点时, 从根节点开始 DFS
如果当前节点为 start, 则返回 1, 并计算左右子树的深度
如果左右子树的深度都大于 0, 则更新 result
否则返回负的深度表示未找到 start
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
    int amountOfTime(TreeNode* root, int start) 
    {
        int result = 0;
        function<int(TreeNode*)> dfs = [&](auto node) -> int
        {
            if (!node) return 0;
            int left = dfs(node -> left), right = dfs(node -> right);
            if (node -> val == start) 
            {
                result = -min(left, right);
                return 1;
            }
            if (left > 0 || right > 0) 
            {
                result = max(result, abs(left) + abs(right));
                return max(left, right) + 1;
            }
            return min(left, right) - 1;
        };
        dfs(root);
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
    private int result = 0;

    public int amountOfTime(TreeNode root, int start) {
        dfs(root, start);
        return result;
    }

    private int dfs(TreeNode node, int start) {
        if (node == null) return 0;
        int left = dfs(node.left, start), right = dfs(node.right, start);
        if (node.val == start) {
            result = -Math.min(left, right);
            return 1;
        }
        if (left > 0 || right > 0) {
            result = Math.max(result, Math.abs(left) + Math.abs(right));
            return Math.max(left, right) + 1;
        }
        return Math.min(left, right) - 1;
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
    def amountOfTime(self, root: Optional[TreeNode], start: int) -> int:
        graph, result, visited = defaultdict(set), 0, {start}

        def dfs(root):
            if root:
                if root.left:
                    graph[root.val].add(root.left.val)
                    graph[root.left.val].add(root.val)
                    dfs(root.left)
                if root.right:
                    graph[root.val].add(root.right.val)
                    graph[root.right.val].add(root.val)
                    dfs(root.right)
        dfs(root)
        def bfs(start, cur):
            nonlocal result
            result = max(result, cur)
            for c in graph[start]:
                if c not in visited:
                    visited.add(c)
                    bfs(c, cur + 1)
        bfs(start, 0)
        return result
```
