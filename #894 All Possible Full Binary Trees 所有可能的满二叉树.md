# 894 All Possible Full Binary Trees 所有可能的满二叉树

__Description__:
Given an integer n, return a list of all possible full binary trees with n nodes. Each node of each tree in the answer must have Node.val == 0.

Each element of the answer is the root node of one possible tree. You may return the final list of trees in any order.

A full binary tree is a binary tree where each node has exactly 0 or 2 children.

__Example:__

Example 1:

![Full Binary Trees](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/22/fivetrees.png)

Input: n = 7
Output: [[0,0,0,null,null,0,0,null,null,0,0],[0,0,0,null,null,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,null,null,null,null,0,0],[0,0,0,0,0,null,null,0,0]]

Example 2:

Input: n = 3
Output: [[0,0,0]]

__Constraints:__

1 <= n <= 20

__题目描述__:
满二叉树是一类二叉树，其中每个结点恰好有 0 或 2 个子结点。

返回包含 N 个结点的所有可能满二叉树的列表。 答案的每个元素都是一个可能树的根结点。

答案中每个树的每个结点都必须有 node.val=0。

你可以按任何顺序返回树的最终列表。

__示例 :__

输入：7
输出：[[0,0,0,null,null,0,0,null,null,0,0],[0,0,0,null,null,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,null,null,null,null,0,0],[0,0,0,0,0,null,null,0,0]]
解释：

![满二叉树](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/08/24/fivetrees.png)

__提示:__

1 <= N <= 20

__思路__:

递归
按照根左右的方式分别给左子树 1, 3, 5, ..., 2k + 1 个结点, 剩下的递归给右子树
时间复杂度为 O(2 ^ n), 空间复杂度为 O(2 ^ n)

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
    vector<TreeNode*> allPossibleFBT(int n) 
    {
        if (n == 1) return { new TreeNode(0) };
        vector<TreeNode*> result;
        for (int i = 1; i < n - 1; i += 2)
        {
            const auto lefts = allPossibleFBT(i), rights = allPossibleFBT(n - 1 - i);
            for (const auto &left : lefts) for (const auto &right : rights) result.emplace_back(new TreeNode(0, left, right));
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
    public List<TreeNode> allPossibleFBT(int n) {
        if (n == 1) return new ArrayList<>(Arrays.asList(new TreeNode(0)));
        List<TreeNode> result = new ArrayList<>();
        for (int i = 1; i < n - 1; i += 2) {
            List<TreeNode> lefts = allPossibleFBT(i), rights = allPossibleFBT(n - 1 - i);
            for (TreeNode left : lefts) for (TreeNode right : rights) result.add(new TreeNode(0, left, right));
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
    def allPossibleFBT(self, n: int) -> List[TreeNode]:
        if n == 1:
            return [TreeNode(0)]
        result = []
        for i in range(1, n - 1, 2):
            lefts, rights = self.allPossibleFBT(i), self.allPossibleFBT(n - 1 - i)
            for left in lefts:
                for right in rights:
                    result.append(TreeNode(0, left, right))
        return result
```
