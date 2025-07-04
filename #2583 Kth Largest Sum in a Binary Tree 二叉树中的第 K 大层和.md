# 2583 Kth Largest Sum in a Binary Tree 二叉树中的第 K 大层和

__Description:__

You are given the `root` of a binary tree and a positive integer `k`.

The level sum in the tree is the sum of the values of the nodes that are on the same level.

Return _the_ `k ^ th` ___largest__ level sum in the tree (not necessarily distinct)_. If there are fewer than `k` levels in the tree, return `-1`.

Note that two nodes are on the same level if they have the same distance from the root.

__Example:__

Example 1:

![2583-1](https://assets.leetcode.com/uploads/2022/12/14/binaryytreeedrawio-2.png)

```text
Input: root = [5,8,9,2,1,3,7,4,6], k = 2
Output: 13
Explanation: The level sums are the following:
```

- Level 1: 5.
- Level 2: 8 + 9 = 17.
- Level 3: 2 + 1 + 3 + 7 = 13.
- Level 4: 4 + 6 = 10.

The 2nd largest level sum is 13.

Example 2:

![2583-2](https://assets.leetcode.com/uploads/2022/12/14/treedrawio-3.png)

```text
Input: root = [1,2,null,3], k = 1
Output: 3
Explanation: The largest level sum is 3.
```

__Constraints:__

- The number of nodes in the tree is `n`.
- `2 <= n <= 10 ^ 5`
- `1 <= Node.val <= 10 ^ 6`
- `1 <= k <= n`

__题目描述:__

给你一棵二叉树的根节点 `root` 和一个正整数 `k` 。

树中的 层和 是指 同一层 上节点值的总和。

返回树中第 `k` 大的层和（不一定不同）。如果树少于 `k` 层，则返回 `-1` 。

注意，如果两个节点与根节点的距离相同，则认为它们在同一层。

__示例:__

示例 1：

![2583-3](https://assets.leetcode.com/uploads/2022/12/14/binaryytreeedrawio-2.png)

```text
输入：root = [5,8,9,2,1,3,7,4,6], k = 2
输出：13
解释：树中每一层的层和分别是：
```

- Level 1: 5
- Level 2: 8 + 9 = 17
- Level 3: 2 + 1 + 3 + 7 = 13
- Level 4: 4 + 6 = 10

第 2 大的层和等于 13 。

示例 2：

![2583-4](https://assets.leetcode.com/uploads/2022/12/14/treedrawio-3.png)

```text
输入：root = [1,2,null,3], k = 1
输出：3
解释：最大的层和是 3 。
```

__提示：__

- 树中的节点数为 `n`
- `2 <= n <= 10 ^ 5`
- `1 <= Node.val <= 10 ^ 6`
- `1 <= k <= n`

__思路:__

```text
层序遍历
一边遍历一边计算当前层的总和
并把总和加入一个列表或者堆中
最后对列表进行排序或者使用快速选择找到第 k 大的值
也可以把堆大小控制在 k, 返回堆顶元素
时间复杂度为 O(N), 空间复杂度为 O(N), 快速选择最快为 O(N)
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
    long long kthLargestLevelSum(TreeNode* root, int k) 
    {
        vector<long long> heap;
        vector<TreeNode*> q = {root};
        while (!q.empty()) 
        {
            long long cur = 0LL;
            vector<TreeNode*> level;
            for (const auto& node : q) 
            {
                cur += node -> val;
                if (node -> left) level.emplace_back(node -> left);
                if (node -> right) level.emplace_back(node -> right);
            }
            heap.push_back(cur);
            q = move(level);
        }
        ranges::nth_element(heap, heap.begin() + (heap.size() - k));
        return heap.size() < k ? -1 : heap[heap.size() - k];
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
    public long kthLargestLevelSum(TreeNode root, int k) {
        var heap = new ArrayList<Long>();
        var queue = List.of(root);
        while (!queue.isEmpty()) {
            long cur = 0L;
            var level = queue;
            queue = new ArrayList<>();
            for (TreeNode node : level) {
                cur += node.val;
                if (node.left != null) queue.add(node.left);
                if (node.right != null) queue.add(node.right);
            }
            heap.add(cur);
        }
        Collections.sort(heap);
        return heap.size() < k ? -1 : heap.get(heap.size() - k);
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
    def kthLargestLevelSum(self, root: Optional[TreeNode], k: int) -> int:
        heap, queue = [], [root]
        while queue:
            cur, level, queue = 0, queue, []
            for node in level:
                cur += node.val
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            heappush(heap, cur)
            if len(heap) > k:
                heappop(heap)
        return -1 if k > len(heap) else heap[0]
```
