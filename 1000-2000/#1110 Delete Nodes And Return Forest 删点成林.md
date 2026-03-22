# 1110 Delete Nodes And Return Forest 删点成林

__Description__:
Given the root of a binary tree, each node in the tree has a distinct value.

After deleting all nodes with a value in to_delete, we are left with a forest (a disjoint union of trees).

Return the roots of the trees in the remaining forest. You may return the result in any order.

__Example:__

Example 1:

![Tree](https://assets.leetcode.com/uploads/2019/07/01/screen-shot-2019-07-01-at-53836-pm.png)

Input: root = [1,2,3,4,5,6,7], to_delete = [3,5]
Output: [[1,2,null,4],[6],[7]]

Example 2:

Input: root = [1,2,4,null,3], to_delete = [3]
Output: [[1,2,4]]

__Constraints:__

The number of nodes in the given tree is at most 1000.
Each node has a distinct value between 1 and 1000.
to_delete.length <= 1000
to_delete contains distinct values between 1 and 1000.

__题目描述__:
给出二叉树的根节点 root，树上每个节点都有一个不同的值。

如果节点值在 to_delete 中出现，我们就把该节点从树上删去，最后得到一个森林（一些不相交的树构成的集合）。

返回森林中的每棵树。你可以按任意顺序组织答案。

__示例 :__

示例 1：

![树](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/07/05/screen-shot-2019-07-01-at-53836-pm.png)

输入：root = [1,2,3,4,5,6,7], to_delete = [3,5]
输出：[[1,2,null,4],[6],[7]]

示例 2：

输入：root = [1,2,4,null,3], to_delete = [3]
输出：[[1,2,4]]

__提示:__

树中的节点数最大为 1000。
每个节点都有一个介于 1 到 1000 之间的值，且各不相同。
to_delete.length <= 1000
to_delete 包含一些从 1 到 1000、各不相同的值。

__思路__:

后序遍历
将需要删除的结点置空
同时将结点加入结果中
最后判断根结点是否需要删除
时间复杂度O(n), 空间复杂度O(n)

__代码__:
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
private:
    unordered_set<int> s;
    vector<TreeNode*> result;
    
    void dfs(TreeNode* parent, TreeNode* cur) 
    {
        if (!cur) return;
        dfs(cur, cur -> left);
        dfs(cur, cur -> right);
        if (s.count(cur -> val)) 
        {
            if (parent and parent -> left == cur) parent -> left = nullptr;
            if (parent and parent -> right == cur) parent -> right = nullptr;
            if (cur -> left) result.emplace_back(cur -> left);
            if (cur -> right) result.emplace_back(cur -> right);
        }
    }
public:
    vector<TreeNode*> delNodes(TreeNode* root, vector<int>& to_delete)
    {
        for (const auto& d : to_delete) s.insert(d);
        dfs(nullptr, root);
        if (!s.count(root -> val)) result.emplace_back(root);
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
    private Set<Integer> set = new HashSet<>();
    private List<TreeNode> result = new ArrayList<>();
    
    public List<TreeNode> delNodes(TreeNode root, int[] to_delete) {
        for (int d : to_delete) set.add(d);
        dfs(null, root);
        if (!set.contains(root.val)) result.add(root);
        return result;
    }
    
    private void dfs(TreeNode parent, TreeNode cur) {
        if (cur == null) return;
        dfs(cur, cur.left);
        dfs(cur, cur.right);
        if (set.contains(cur.val)) {
            if (parent != null && parent.left == cur) parent.left = null;
            if (parent != null && parent.right == cur) parent.right = null;
            if (cur.left != null) result.add(cur.left);
            if (cur.right != null) result.add(cur.right);
        }
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
    def delNodes(self, root: TreeNode, to_delete: List[int]) -> List[TreeNode]:
        s, result = set(to_delete), []
        
        def dfs(parent: TreeNode, cur: TreeNode) -> None:
            if not cur:
                return
            dfs(cur, cur.left)
            dfs(cur, cur.right)
            if cur.val in s:
                if parent and parent.left == cur:
                    parent.left = None
                if parent and parent.right == cur:
                    parent.right = None
                if cur.left:
                    result.append(cur.left)
                if cur.right:
                    result.append(cur.right)
        dfs(None, root)
        if root.val not in s:
            result.append(root)
        return result
```
