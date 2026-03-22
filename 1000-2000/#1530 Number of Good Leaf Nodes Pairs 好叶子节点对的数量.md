# 1530 Number of Good Leaf Nodes Pairs 好叶子节点对的数量

__Description:__

You are given the `root` of a binary tree and an integer `distance`. A pair of two different __leaf__ nodes of a binary tree is said to be good if the length of __the shortest path__ between them is less than or equal to `distance`.

Return the number of good leaf node pairs in the tree.

__Example:__

Example 1:

![1530-1](https://assets.leetcode.com/uploads/2020/07/09/e1.jpg)

```text
Input: root = [1,2,3,null,4], distance = 3
Output: 1
Explanation: The leaf nodes of the tree are 3 and 4 and the length of the shortest path between them is 3. This is the only good pair.
```

Example 2:

![1530-2](https://assets.leetcode.com/uploads/2020/07/09/e2.jpg)

```text
Input: root = [1,2,3,4,5,6,7], distance = 3
Output: 2
Explanation: The good pairs are [4,5] and [6,7] with shortest path = 2. The pair [4,6] is not good because the length of ther shortest path between them is 4.
```

Example 3:

```text
Input: root = [7,1,4,6,null,5,3,null,null,null,null,null,2], distance = 3
Output: 1
Explanation: The only good pair is [2,5].
```

__Constraints:__

- The number of nodes in the `tree` is in the range `[1, 2 ^ 10].`
- `1 <= Node.val <= 100`
- `1 <= distance <= 10`

__题目描述:__

给你二叉树的根节点 `root` 和一个整数 `distance` 。

如果二叉树中两个 __叶__ 节点之间的 __最短路径长度__ 小于或者等于 `distance` ，那它们就可以构成一组 __好叶子节点对__ 。

返回树中 好叶子节点对的数量 。

__示例:__

示例 1：

![1530-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/26/e1.jpg)

```text
输入：root = [1,2,3,null,4], distance = 3
输出：1
解释：树的叶节点是 3 和 4 ，它们之间的最短路径的长度是 3 。这是唯一的好叶子节点对。
```

示例 2：

![1530-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/26/e2.jpg)

```text
输入：root = [1,2,3,4,5,6,7], distance = 3
输出：2
解释：好叶子节点对为 [4,5] 和 [6,7] ，最短路径长度都是 2 。但是叶子节点对 [4,6] 不满足要求，因为它们之间的最短路径长度为 4 。
```

示例 3：

```text
输入：root = [7,1,4,6,null,5,3,null,null,null,null,null,2], distance = 3
输出：1
解释：唯一的好叶子节点对是 [2,5] 。
```

示例 4：

```text
输入：root = [100], distance = 1
输出：0
```

示例 5：

```text
输入：root = [1,1,1], distance = 2
输出：1
```

__提示：__

- `tree` 的节点数在 `[1, 2 ^ 10]` 范围内。
- 每个节点的值都在 `[1, 100]` 之间。
- `1 <= distance <= 10`

__思路:__

```text
递归
不需要记录节点的值, 只需要记录子节点到父节点的距离的个数
只需要记录 distance + 1 的节点的个数, 最多只用记录 distance 距离的节点
每次记录完当前节点需要把记录值记录到上一级节点, 也就是后序遍历的方式记录
时间复杂度为 O(ND ^ 2), 空间复杂度为 O(HD), 其中 D 为 distance, H 为树的高度, 最大为 N
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    vector<int> dfs(const TreeNode* root, const int distance, int& result) 
    {
        if (!root) return {};
        if (!root -> left and !root -> right) return { 0 };
        vector<int> cur, left = dfs(root -> left, distance, result), right = dfs(root -> right, distance, result);
        for (auto& l: left) if (++l <= distance) cur.emplace_back(l);
        for (auto& r: right) if (++r <= distance) cur.emplace_back(r);
        for (const auto& l: left) for (const auto& r: right) result += (l + r <= distance);
        return cur;
    }
public:
    int countPairs(TreeNode* root, int distance) 
    {
        int result = 0;
        dfs(root, distance, result);
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
    
    public int countPairs(TreeNode root, int distance) {
        dfs(root, distance);
        return result;
    }
    
    private int[] dfs(TreeNode root, int distance) {
        int[] count = new int[distance + 1];
        if (root == null) return count;
        if (root.left == null && root.right == null) {
            count[1] = 1;
            return count;
        }
        int[] left = dfs(root.left, distance), right = dfs(root.right, distance);
        for (int i = 1; i <= distance; i++) for (int j = 1; j <= distance - i; j++) result += left[i] * right[j];
        for (int i = 2; i <= distance; i++) count[i] = left[i - 1] + right[i - 1];
        return count;
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
    def countPairs(self, root: TreeNode, distance: int) -> int:
        result = 0

        def dfs(node: TreeNode, depth: int) -> dict:
            nonlocal result
            if not node.left and not node.right:
                return Counter([depth])

            left_leaf_depth, right_leaf_depth = Counter(), Counter()
            if node.left:
                left_leaf_depth = dfs(node.left, depth + 1)
            if node.right:
                right_leaf_depth = dfs(node.right, depth + 1)

            for left_depth, left_count in left_leaf_depth.items():
                for right_depth, right_count in right_leaf_depth.items():
                    if (left_depth - depth) + (right_depth - depth) <= distance:
                        result += left_count * right_count

            return left_leaf_depth + right_leaf_depth

        dfs(root, 0)
        return result
```
