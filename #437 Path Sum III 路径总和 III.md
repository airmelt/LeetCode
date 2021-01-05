# 437 Path Sum III 路径总和 III

__Description__:
You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

__Example:__

root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

```text
      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1
```

Return 3. The paths that sum to 8 are:

```text
1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```

__题目描述__:
给定一个二叉树，它的每个结点都存放着一个整数值。

找出路径和等于给定数值的路径总数。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。

__示例：__

root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

```text
      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1
```

返回 3。和等于 8 的路径有:

```text
1.  5 -> 3
2.  5 -> 2 -> 1
3.  -3 -> 11
```

__思路__:

1. 深度优先搜索和层次遍历结合
2. 双重递归
时间复杂度O(n ^ 2), 空间复杂度O(n)

__代码__:
__C++__:

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution 
{
public:
    int pathSum(TreeNode* root, int sum) 
    {
        if (!root) return 0;
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) 
        {
            int s = q.size();
            for (int i = 0; i < s; i++) 
            {
                TreeNode* cur = q.front();
                if (cur -> left) q.push(cur -> left);
                if (cur -> right) q.push(cur -> right);
                dfs(cur, sum);
                q.pop();
            }
        }
        return result;
    }
private:
    int result = 0;
    void dfs(TreeNode* root, int sum) 
    {
        if (!root) return;
        sum -= root -> val;
        if (!sum) result++;
        dfs(root -> left, sum);
        dfs(root -> right, sum);
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
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    private int result = 0;
    private void dfs(TreeNode root, int sum) {
        if (root == null) return;
        sum -= root.val;
        if (sum == 0) result++;
        dfs(root.left, sum);
        dfs(root.right, sum);
    }
    public int pathSum(TreeNode root, int sum) {
        if (root == null) return 0;
        List<TreeNode> list = new ArrayList<>();
        list.add(root);
        while (!list.isEmpty()) {
            int s = list.size();
            for (int i = 0; i < s; i++) {
                TreeNode cur = list.get(0);
                if (cur.left != null) list.add(cur.left);
                if (cur.right != null) list.add(cur.right);
                dfs(cur, sum);
                list.remove(0);
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
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> int:
        if not root:
            return 0
        return self.check(root, sum) + self.pathSum(root.left, sum) + self.pathSum(root.right, sum)

    def check(self, root: TreeNode, sum: int) -> int:
        if not root:
            return 0
        result = 0
        if sum == root.val:
            result += 1
        result += self.check(root.left, sum - root.val) + self.check(root.right, sum- root.val)
        return result
```
