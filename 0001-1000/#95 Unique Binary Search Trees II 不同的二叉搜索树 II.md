# 95 Unique Binary Search Trees II 不同的二叉搜索树 II

__Description__:
Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1 ... n.

__Example:__

Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
Explanation:
The above output corresponds to the 5 unique BST's shown below:

```text
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

__Constraints:__

0 <= n <= 8

__题目描述__:
给定一个整数 n，生成所有由 1 ... n 为节点所组成的 二叉搜索树 。

__示例 :__

输入：3
输出：
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
解释：
以上的输出对应以下 5 种不同结构的二叉搜索树：

```text
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

__提示：__

0 <= n <= 8

__思路__:

递归法
将每一个数字作为根节点, 将小于根节点的一边作为左子树, 大于根节点的一边作为右子树, 递归调用, 然后按顺序插入即可
时间复杂度O(4 ^ n / n ^ (1 / 2)), 空间复杂度O(1)

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
public:
    vector<TreeNode*> generateTrees(int n) 
    {
        vector<TreeNode*> result;
        if (n == 0) return result;
        return generateTrees(1, n);
    }
private:
    vector<TreeNode*> generateTrees(int start, int end)
    {
        vector<TreeNode*> result;
        if (start > end) 
        {
            result.push_back(nullptr);
            return result;
        }
        for (int i = start; i <= end; i++)
        {
            vector<TreeNode*> left_tree = generateTrees(start, i - 1);
            vector<TreeNode*> right_tree = generateTrees(i + 1, end);
            for (auto left : left_tree) for (auto right : right_tree)
            {
                TreeNode* root = new TreeNode(i);
                root -> left = left;
                root -> right = right;
                result.push_back(root);
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
    public List<TreeNode> generateTrees(int n) {
        if (n == 0) return new ArrayList<TreeNode>();
        return generateTrees(1, n);
    }
    
    private List<TreeNode> generateTrees(int start, int end) {
        List<TreeNode> result = new ArrayList<>();
        if (start > end) {
            result.add(null);
            return result;
        }
        for (int i = start; i <= end; i++) {
            List<TreeNode> leftTree = generateTrees(start, i - 1);
            List<TreeNode> rightTree = generateTrees(i + 1, end);
            for (TreeNode left : leftTree) for (TreeNode right : rightTree) {
                TreeNode root = new TreeNode(i);
                root.left = left;
                root.right = right;
                result.add(root);
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
    def generateTrees(self, n: int) -> List[TreeNode]:
        if not n:
            return []
        def helper(start: int, end: int) -> List[TreeNode]:
            result = []
            if start > end:
                result.append(None)
                return result
            for i in range(start, end + 1):
                left_tree, right_tree = helper(start, i - 1), helper(i + 1, end)
                for left in left_tree:
                    for right in right_tree:
                        root = TreeNode(i)
                        root.left, root.right = left, right
                        result.append(root)
            return result
        return helper(1, n)
```
