# 2471 Minimum Number of Operations to Sort a Binary Tree by Level 逐层排序二叉树所需的最少操作数目

__Description:__

You are given the `root` of a binary tree with __unique values__.

In one operation, you can choose any two nodes at the same level and swap their values.

Return the minimum number of operations needed to make the values at each level sorted in a strictly increasing order.

The level of a node is the number of edges along the path between it and the root node.

__Example:__

Example 1:

![2471-1](https://assets.leetcode.com/uploads/2022/09/18/image-20220918174006-2.png)

```text
Input: root = [1,4,3,7,6,8,5,null,null,null,null,9,null,10]
Output: 3
Explanation:
```

- Swap 4 and 3. The 2nd level becomes [3,4].
- Swap 7 and 5. The 3rd level becomes [5,6,8,7].
- Swap 8 and 7. The 3rd level becomes [5,6,7,8].

We used 3 operations so return 3.

It can be proven that 3 is the minimum number of operations needed.

Example 2:

![2471-2](https://assets.leetcode.com/uploads/2022/09/18/image-20220918174026-3.png)

```text
Input: root = [1,3,2,7,6,5,4]
Output: 3
Explanation:
```

- Swap 3 and 2. The 2nd level becomes [2,3].
- Swap 7 and 4. The 3rd level becomes [4,6,5,7].
- Swap 6 and 5. The 3rd level becomes [4,5,6,7].

We used 3 operations so return 3.

It can be proven that 3 is the minimum number of operations needed.

Example 3:

![2471-3](https://assets.leetcode.com/uploads/2022/09/18/image-20220918174052-4.png)

```text
Input: root = [1,2,3,4,5,6]
Output: 0
Explanation: Each level is already sorted in increasing order so return 0.
```

__Constraints:__

- The number of nodes in the tree is in the range `[1, 10 ^ 5]`.
- `1 <= Node.val <= 10 ^ 5`
- All the values of the tree are __unique__.

__题目描述:__

给你一个 __值互不相同__ 的二叉树的根节点 `root` 。

在一步操作中，你可以选择 同一层 上任意两个节点，交换这两个节点的值。

返回每一层按 严格递增顺序 排序所需的最少操作数目。

节点的 层数 是该节点和根节点之间的路径的边数。

__示例:__

示例 1 ：

![2471-4](https://assets.leetcode.com/uploads/2022/09/18/image-20220918174006-2.png)

```text
输入：root = [1,4,3,7,6,8,5,null,null,null,null,9,null,10]
输出：3
解释：
```

- 交换 4 和 3 。第 2 层变为 [3,4] 。
- 交换 7 和 5 。第 3 层变为 [5,6,8,7] 。
- 交换 8 和 7 。第 3 层变为 [5,6,7,8] 。

共计用了 3 步操作，所以返回 3 。

可以证明 3 是需要的最少操作数目。

示例 2 ：

![2471-5](https://assets.leetcode.com/uploads/2022/09/18/image-20220918174026-3.png)

```text
输入：root = [1,3,2,7,6,5,4]
输出：3
解释：
```

- 交换 3 和 2 。第 2 层变为 [2,3] 。
- 交换 7 和 4 。第 3 层变为 [4,6,5,7] 。
- 交换 6 和 5 。第 3 层变为 [4,5,6,7] 。

共计用了 3 步操作，所以返回 3 。

可以证明 3 是需要的最少操作数目。

示例 3 ：

![2471-6](https://assets.leetcode.com/uploads/2022/09/18/image-20220918174052-4.png)

```text
输入：root = [1,2,3,4,5,6]
输出：0
解释：每一层已经按递增顺序排序，所以返回 0 。
```

__提示：__

- 树中节点的数目在范围 `[1, 10 ^ 5]` 。
- `1 <= Node.val <= 10 ^ 5`
- 树中的所有值 __互不相同__ 。

__思路:__

```text
层序遍历
层序遍历二叉树
记录每一层的节点值
排序的时候找到每一个成环的位置
比如 [2, 0, 1, 4, 5] 中, [2, 0, 1] 是一个环
排序只需要在环内交换即可, 交换次数为环内元素个数 - 1
为了使用下标可以将元素离散化
即将元素映射到 range(n)
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
    int minimumOperations(TreeNode* root) 
    {
        queue<TreeNode*> q;
        q.emplace(root);
        int result = 0;
        while (!q.empty())
        {
            int s = q.size();
            vector<int> level(s), a(s);
            for (int i = 0; i < s; i++) 
            {
                auto cur = q.front();
                q.pop();
                a[i] = level[i] = cur -> val;
                if (cur -> left) q.emplace(cur -> left);
                if (cur -> right) q.emplace(cur -> right);
            }
            unordered_map<int, int> m;
            sort(a.begin(), a.end());
            for (int i = 0, n = level.size(); i < n; i++) m[a[i]] = i;
            for (int i = 0, n = level.size(), t = 0, j = 0; i < n; i++) 
            {
                while (level[i] != a[i]) 
                {
                    level[i] = level[j = m[t = level[i]]];
                    level[j] = t;
                    ++result;
                }
            }
        }
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
    public int minimumOperations(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>(){{ add(root); }};
        int result = 0;
        while (!queue.isEmpty()) {
            int s = queue.size(), level[] = new int[s], a[] = new int[s];
            for (int i = 0; i < s; i++) {
                TreeNode cur = queue.poll();
                level[i] = a[i] = cur.val;
                if (cur.left != null) queue.offer(cur.left);
                if (cur.right != null) queue.offer(cur.right);
            }
            var map = new HashMap<Integer, Integer>();
            Arrays.sort(a);
            for (int i = 0, n = level.length; i < n; i++) map.put(a[i], i);
            for (int i = 0, n = level.length, t = 0, j = 0; i < n; i++) {
                while (level[i] != a[i]) {
                    level[i] = level[j = map.get(t = level[i])];
                    level[j] = t;
                    ++result;
                }
            }
        }
        return result;
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
    def minimumOperations(self, root: Optional[TreeNode]) -> int:
        result, queue = 0, [root]
        while queue:
            s = len(queue)
            level = []
            for _ in range(s):
                cur = queue.pop(0)
                level.append(cur.val)
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
            visited = [False] * (n := len(level))
            a = sorted(range(n), key=lambda i: level[i])
            result += n
            for v in a:
                if visited[v]:
                    continue
                while not visited[v]:
                    visited[v], v = True, a[v]
                result -= 1
        return result
```
