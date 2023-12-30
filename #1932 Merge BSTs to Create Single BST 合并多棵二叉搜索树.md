# 1932 Merge BSTs to Create Single BST 合并多棵二叉搜索树

__Description:__

You are given `n` __BST (binary search tree) root nodes__ for `n` separate BSTs stored in an array `trees` (__0-indexed__). Each BST in `trees` has __at most 3 nodes__, and no two roots have the same value. In one operation, you can:

- Select two __distinct__ indices `i` and `j` such that the value stored at one of the __leaves__ of `trees[i]` is equal to the __root value__ of `trees[j]`.
- Replace the leaf node in `trees[i]` with `trees[j]`.
- Remove `trees[j]` from `trees`.

Return _the __root__ of the resulting BST if it is possible to form a valid BST after performing_ `n - 1` _operations, or_ `null` _if it is impossible to create a valid BST_.

A BST (binary search tree) is a binary tree where each node satisfies the following property:

- Every node in the node's left subtree has a value __strictly less__ than the node's value.
- Every node in the node's right subtree has a value __strictly greater__ than the node's value.

A leaf is a node that has no children.

__Example:__

Example 1:

![1932-1](https://assets.leetcode.com/uploads/2021/06/08/d1.png)

```text
Input: trees = [[2,1],[3,2,5],[5,4]]
Output: [3,2,5,1,null,4]
Explanation:
In the first operation, pick i=1 and j=0, and merge trees[0] into trees[1].
Delete trees[0], so trees = [[3,2,5,1],[5,4]].
In the second operation, pick i=0 and j=1, and merge trees[1] into trees[0].
Delete trees[1], so trees = [[3,2,5,1,null,4]].
The resulting tree, shown above, is a valid BST, so return its root.
```

![1932-2](https://assets.leetcode.com/uploads/2021/06/24/diagram.png)

![1932-3](https://assets.leetcode.com/uploads/2021/06/24/diagram-2.png)

Example 2:

![1932-4](https://assets.leetcode.com/uploads/2021/06/08/d2.png)

```text
Input: trees = [[5,3,8],[3,2,6]]
Output: []
Explanation:
Pick i=0 and j=1 and merge trees[1] into trees[0].
Delete trees[1], so trees = [[5,3,8,2,6]].
The resulting tree is shown above. This is the only valid operation that can be performed, but the resulting tree is not a valid BST, so return null.
```

![1932-5](https://assets.leetcode.com/uploads/2021/06/24/diagram-3.png)

Example 3:

![1932-6](https://assets.leetcode.com/uploads/2021/06/08/d3.png)

```text
Input: trees = [[5,4],[3]]
Output: []
Explanation: It is impossible to perform any operations.
```

__Constraints:__

- `n == trees.length`
- `1 <= n <= 5 * 10 ^ 4`
- The number of nodes in each tree is in the range `[1, 3]`.
- Each node in the input may have children but no grandchildren.
- No two roots of `trees` have the same value.
- All the trees in the input are __valid BSTs__.
- `1 <= TreeNode.val <= 5 * 10 ^ 4`.

__题目描述:__

给你 `n` 个 __二叉搜索树的根节点__ ，存储在数组 `trees` 中（__下标从 0 开始__），对应 `n` 棵不同的二叉搜索树。 `trees` 中的每棵二叉搜索树 __最多有 3 个节点__ ，且不存在值相同的两个根节点。在一步操作中，将会完成下述步骤:

- 选择两个 __不同的__ 下标 `i` 和 `j` ，要求满足在 `trees[i]` 中的某个 __叶节点__ 的值等于 `trees[j]` 的 __根节点的值__ 。
- 用 `trees[j]` 替换 `trees[i]` 中的那个叶节点。
- 从 `trees` 中移除 `trees[j]` 。

如果在执行 `n - 1` 次操作后，能形成一棵有效的二叉搜索树，则返回结果二叉树的 __根节点__ ；如果无法构造一棵有效的二叉搜索树_，_返回 `null` 。

二叉搜索树是一种二叉树，且树中每个节点均满足下述属性：

- 任意节点的左子树中的值都 __严格小于__ 此节点的值。
- 任意节点的右子树中的值都 __严格大于__ 此节点的值。

叶节点是不含子节点的节点。

__示例:__

示例 1：

![1932-7](https://assets.leetcode.com/uploads/2021/06/08/d1.png)

```text
输入：trees = [[2,1],[3,2,5],[5,4]]
输出：[3,2,5,1,null,4]
解释：
第一步操作中，选出 i=1 和 j=0 ，并将 trees[0] 合并到 trees[1] 中。
删除 trees[0] ，trees = [[3,2,5,1],[5,4]] 。
在第二步操作中，选出 i=0 和 j=1 ，将 trees[1] 合并到 trees[0] 中。
删除 trees[1] ，trees = [[3,2,5,1,null,4]] 。
结果树如上图所示，为一棵有效的二叉搜索树，所以返回该树的根节点。
```

![1932-8](https://assets.leetcode.com/uploads/2021/06/24/diagram.png)

![1932-9](https://assets.leetcode.com/uploads/2021/06/24/diagram-2.png)

示例 2：

![1932-10](https://assets.leetcode.com/uploads/2021/06/08/d2.png)

```text
输入：trees = [[5,3,8],[3,2,6]]
输出：[]
解释：
选出 i=0 和 j=1 ，然后将 trees[1] 合并到 trees[0] 中。
删除 trees[1] ，trees = [[5,3,8,2,6]] 。
结果树如上图所示。仅能执行一次有效的操作，但结果树不是一棵有效的二叉搜索树，所以返回 null 。
```

![1932-11](https://assets.leetcode.com/uploads/2021/06/24/diagram-3.png)

示例 3：

![1932-12](https://assets.leetcode.com/uploads/2021/06/08/d3.png)

```text
输入：trees = [[5,4],[3]]
输出：[]
解释：无法执行任何操作。
```

__提示：__

- `n == trees.length`
- `1 <= n <= 5 * 10 ^ 4`
- 每棵树中节点数目在范围 `[1, 3]` 内。
- 输入数据的每个节点可能有子节点但不存在子节点的子节点
- `trees` 中不存在两棵树根节点值相同的情况。
- 输入中的所有树都是 __有效的二叉树搜索树__ 。
- `1 <= TreeNode.val <= 5 * 10 ^ 4`.

__思路:__

```text
中序遍历
最后的二叉搜索树的中序遍历应该是一个严格递增的序列
根结点不能是叶子结点
用一个哈希表记录所有出现过的叶子结点
用一个哈希映射记录所有出现过的根结点的值和对应的根结点, 题目中已经保证不会出现重复的根结点
遍历所有的树, 将所有的叶子结点加入哈希表, 将所有的根结点加入哈希映射
再遍历一次所有的树, 如果当前结点不在叶子结点的哈希表中, 将该结点从哈希映射中移除, 尝试进行中序遍历, 如果能够完成中序遍历, 则返回该树的根结点, 否则返回 null
中序遍历先访问最左边的结点, 如果当前结点是叶子结点, 则可以尝试进行合并
如果当前值不大于上一个值, 则返回 false
否则继续遍历右子树
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
    TreeNode* canMerge(vector<TreeNode*>& trees) 
    {
        for (const auto& tree : trees)
        {
            if (tree -> left) leaves.insert(tree -> left -> val);
            if (tree -> right) leaves.insert(tree -> right -> val);
            roots[tree -> val] = tree;
        }
        for (const auto& tree : trees)
        {
            if (!leaves.count(tree -> val))
            {
                roots.erase(tree -> val);
                return dfs(tree) and roots.empty() ? tree : nullptr;
            }
        }
        return nullptr;
    }
private:
    unordered_set<int> leaves;
    unordered_map<int, TreeNode*> roots;
    int pre = -0x3f3f3f3f;

    bool dfs(TreeNode* tree)
    {
        if (!tree) return true;
        if (!tree -> left and !tree -> right and roots.count(tree -> val))
        {
            tree -> left = roots[tree -> val] -> left;
            tree -> right = roots[tree -> val] -> right;
            roots.erase(tree -> val);
        }
        if (!dfs(tree -> left)) return false;
        if (pre >= tree -> val) return false;
        pre = tree -> val;
        return dfs(tree -> right);
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
    private int pre = -0x3f3f3f3f;
    private Set<Integer> leaves = new HashSet<>();
    private Map<Integer, TreeNode> roots = new HashMap<>();
    
    public TreeNode canMerge(List<TreeNode> trees) {
        for (TreeNode tree : trees) {
            if (tree.left != null) leaves.add(tree.left.val);
            if (tree.right != null) leaves.add(tree.right.val);
            roots.put(tree.val, tree);
        }
        for (TreeNode tree : trees) {
            if (!leaves.contains(tree.val)) {
                roots.remove(tree.val);
                return dfs(tree) && roots.isEmpty() ? tree : null;
            }
        }
        return null;
    }

    private boolean dfs(TreeNode tree) {
        if (tree == null) return true;
        if (tree.left == null && tree.right == null && roots.containsKey(tree.val)) {
            tree.left = roots.get(tree.val).left;
            tree.right = roots.get(tree.val).right;
            roots.remove(tree.val);
        }
        if (!dfs(tree.left)) return false;
        if (pre >= tree.val) return false;
        pre = tree.val;
        return dfs(tree.right);
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
    def canMerge(self, trees: List[TreeNode]) -> Optional[TreeNode]:
        leaves, roots, pre = set(), defaultdict(lambda x: Optional[TreeNode]), -inf
        for tree in trees:
            if tree.left:
                leaves.add(tree.left.val)
            if tree.right:
                leaves.add(tree.right.val)
            roots[tree.val] = tree
        
        def dfs(tree: Optional[TreeNode]) -> bool:
            if not tree:
                return True
            if not tree.left and not tree.right and tree.val in roots:
                tree.left, tree.right = roots[tree.val].left, roots[tree.val].right
                roots.pop(tree.val)
            if not dfs(tree.left):
                return False
            nonlocal pre
            if tree.val <= pre:
                return False
            pre = tree.val
            return dfs(tree.right)
        for tree in trees:
            if tree.val not in leaves:
                roots.pop(tree.val)
                return tree if dfs(tree) and not roots else None
        return None
```
