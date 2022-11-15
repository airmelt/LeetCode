# 1382 Balance a Binary Search Tree 将二叉搜索树变平衡

__Description:__

Given the root of a binary search tree, return a balanced binary search tree with the same node values. If there is more than one answer, return any of them.

A binary search tree is balanced if the depth of the two subtrees of every node never differs by more than 1.

__Example:__

Example 1:

![1382-1](https://assets.leetcode.com/uploads/2021/08/10/balance1-tree.jpg)

Input: root = [1,null,2,null,3,null,4,null,null]
Output: [2,1,3,null,null,null,4]
Explanation: This is not the only correct answer, [3,1,4,null,2] is also correct.

Example 2:

![1382-2](https://assets.leetcode.com/uploads/2021/08/10/balanced2-tree.jpg)

Input: root = [2,1,3]
Output: [2,1,3]

__Constraints:__

The number of nodes in the tree is in the range [1, 10^4].
1 <= Node.val <= 10^5

__描述:__

给你一棵二叉搜索树，请你返回一棵 平衡后 的二叉搜索树，新生成的树应该与原来的树有着相同的节点值。如果有多种构造方法，请你返回任意一种。

如果一棵二叉搜索树中，每个节点的两棵子树高度差不超过 1 ，我们就称这棵二叉搜索树是 平衡的 。

__示例:__

示例 1：

![1382-3](https://assets.leetcode.com/uploads/2021/08/10/balance1-tree.jpg)

输入：root = [1,null,2,null,3,null,4,null,null]
输出：[2,1,3,null,null,null,4]
解释：这不是唯一的正确答案，[3,1,4,null,2,null,null] 也是一个可行的构造方案。

示例 2：

![1382-4](https://assets.leetcode.com/uploads/2021/08/10/balanced2-tree.jpg)

输入: root = [2,1,3]
输出: [2,1,3]

__提示：__

树节点的数目在 [1, 10^4] 范围内。
1 <= Node.val <= 10^5

__思路:__

```text
贪心
二叉搜索树中序遍历生成的一定是有序数组
将这个有序数组二分递归建立二叉搜索树一定是平衡的
因为每次二分两边的结点数量最多相差为 1
时间复杂度为 O(n), 空间复杂度为 O(n)
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
private:
    vector<int> tree;
    
    void traversal(TreeNode* root) 
    {
        if (!root) return;
        traversal(root -> left);
        tree.emplace_back(root -> val);
        traversal(root -> right);
    }
    
    TreeNode* build(int left, int right) 
    {
        if (left > right) return nullptr;
        int mid = left + ((right - left) >> 1);
        TreeNode* root = new TreeNode(tree[mid]);
        root -> left = build(left, mid - 1);
        root -> right = build(mid + 1, right);
        return root;
    }
​
public:
    TreeNode* balanceBST(TreeNode* root) 
    {
        traversal(root);
        return build(0, tree.size() - 1);
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
    
    private List<Integer> tree = new ArrayList<>();
    
    void traversal(TreeNode root) {
        if (root == null) return;
        traversal(root.left);
        tree.add(root.val);
        traversal(root.right);
    }
    
    private TreeNode build(int left, int right) {
        if (left > right) return null;
        int mid = left + ((right - left) >> 1);
        TreeNode root = new TreeNode(tree.get(mid));
        root.left = build(left, mid - 1);
        root.right = build(mid + 1, right);
        return root;
    }
    
    public TreeNode balanceBST(TreeNode root) {
        traversal(root);
        return build(0, tree.size() - 1);
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
    def balanceBST(self, root: TreeNode) -> TreeNode:
        tree = deque()
        
        def traversal(cur: TreeNode) -> None:
            if not cur:
                return
            traversal(cur.left)
            tree.append(cur.val)
            traversal(cur.right)
            
        def build(left: int, right: int) -> TreeNode:
            if left > right:
                return None
            root = TreeNode(tree[mid := left + ((right - left) >> 1)])
            root.left, root.right = build(left, mid - 1), build(mid + 1, right)
            return root
        
        traversal(root)
        return build(0, len(tree) - 1)
```
