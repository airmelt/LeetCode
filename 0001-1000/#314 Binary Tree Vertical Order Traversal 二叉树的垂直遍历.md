# 314 Binary Tree Vertical Order Traversal 二叉树的垂直遍历

__Description:__

Given the `root` of a binary tree, return ___the vertical order traversal__ of its nodes' values._ (i.e., from top to bottom, column by column).

If two nodes are in the same row and column, the order should be from __left to right__.

__Example:__

Example 1:

![314-1](https://assets.leetcode.com/uploads/2024/09/23/image1.png)

```text
Input: root = [3,9,20,null,null,15,7]
Output: [[9],[3,15],[20],[7]]
```

Example 2:

![314-2](https://assets.leetcode.com/uploads/2024/09/23/image3.png)

```text
Input: root = [3,9,8,4,0,1,7]
Output: [[4],[9],[3,0,1],[8],[7]]
```

Example 3:

![314-3](https://assets.leetcode.com/uploads/2024/09/23/image2.png)

```text
Input: root = [1,2,3,4,10,9,11,null,5,null,null,null,null,null,null,null,6]
Output: [[4],[2,5],[1,10,9,6],[3],[11]]
```

__Constraints:__

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

__题目描述:__

给你一个二叉树的根结点，返回其结点按 __垂直方向__（从上到下，逐列）遍历的结果。

如果两个结点在同一行和列，那么顺序则为 __从左到右__。

__示例:__

示例 1:

![314-4](https://assets.leetcode.com/uploads/2024/09/23/image1.png)

```text
输入：root = [3,9,20,null,null,15,7]
输出：[[9],[3,15],[20],[7]]
```

示例 2:

![314-5](https://assets.leetcode.com/uploads/2024/09/23/image3.png)

```text
输入：root = [3,9,8,4,0,1,7]
输出：[[4],[9],[3,0,1],[8],[7]]
```

示例 3:

![314-6](https://assets.leetcode.com/uploads/2024/09/23/image2.png)

```text
输入：root = [1,2,3,4,10,9,11,null,5,null,null,null,null,null,null,null,6]
输出：[[4],[2,5],[1,10,9,6],[3],[11]]
```

__提示：__

- 树中结点的数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`

__思路:__

```text
层序遍历 ➕ 有序哈希
层序遍历二叉树
在遍历的时候将节点按照纵坐标插入到有序哈希表中
最后按照有序哈希表的顺序输出结果
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
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
    vector<vector<int>> verticalOrder(TreeNode* root) 
    {
        vector<vector<int>> result;
        if (!root) return result;
        map<int, vector<int>> m;
        unordered_map<TreeNode*, int> pos;
        queue<TreeNode*> q;
        pos[root] = 0;
        q.push(root);
        while (!q.empty()) 
        {
            TreeNode* cur = q.front();
            q.pop();
            m[pos[cur]].emplace_back(cur -> val);
            if (cur -> left)
            {
                q.push(cur -> left);
                pos[cur -> left] = pos[cur] - 1;
            }
            if (cur -> right)
            {
                q.push(cur -> right);
                pos[cur -> right] = pos[cur] + 1;
            }
        }
        transform(m.begin(), m.end(), back_inserter(result), [](const auto &p) { return p.second; });
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
    public List<List<Integer>> verticalOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;
        Map<Integer, List<Integer>> map = new TreeMap<>();
        Map<TreeNode, Integer> pos = new HashMap<>();
        Queue<TreeNode> queue = new LinkedList<>();
        pos.put(root, 0);
        queue.offer(root);
        while (!queue.isEmpty()) {
            TreeNode cur = queue.poll();
            map.computeIfAbsent(pos.get(cur), k -> new ArrayList<>()).add(cur.val);
            if (cur.left != null) {
                queue.offer(cur.left);
                pos.put(cur.left, pos.get(cur) - 1);
            }
            if (cur.right != null) {
                queue.offer(cur.right);
                pos.put(cur.right, pos.get(cur) + 1);
            }
        }
        return new ArrayList<>(map.values());
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
    def verticalOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        d, queue = defaultdict(list), [(root, 0)] if root else []
        while queue:
            cur, offset = queue.pop(0)
            d[offset].append(cur.val)
            if cur.left:
                queue.append((cur.left, offset - 1))
            if cur.right:
                queue.append((cur.right, offset + 1))
        return [d[j] for j in sorted(d)]
```
