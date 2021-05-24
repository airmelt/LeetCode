# 652 Find Duplicate Subtrees 寻找重复的子树

__Description__:
Given the root of a binary tree, return all duplicate subtrees.

For each kind of duplicate subtrees, you only need to return the root node of any one of them.

Two trees are duplicate if they have the same structure with the same node values.

__Example:__

Example 1:

![tree 1](https://assets.leetcode.com/uploads/2020/08/16/e1.jpg)

Input: root = [1,2,3,4,null,2,4,null,null,4]
Output: [[2,4],[4]]

Example 2:

![tree 2](https://assets.leetcode.com/uploads/2020/08/16/e2.jpg)

Input: root = [2,1,1]
Output: [[1]]

Example 3:

![tree 3](https://assets.leetcode.com/uploads/2020/08/16/e33.jpg)

Input: root = [2,2,2,3,null,3,null]
Output: [[2,3],[3]]

__Constraints:__

The number of the nodes in the tree will be in the range [1, 10^4]
-200 <= Node.val <= 200

__题目描述__:
给定一棵二叉树，返回所有重复的子树。对于同一类的重复子树，你只需要返回其中任意一棵的根结点即可。

两棵树重复是指它们具有相同的结构以及相同的结点值。

__示例 :__

示例 1：

```text
        1
       / \
      2   3
     /   / \
    4   2   4
       /
      4
```

下面是两个重复的子树：

```text
      2
     /
    4
```

和

```text
    4
```

因此，你需要以列表的形式返回上述重复子树的根结点。

__思路__:

序列化成字符串存入哈希表中
查询到已有的字符串就加入结果
时间复杂度 O(n), 空间复杂度 O(n)

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
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) 
    {
        vector<TreeNode*> result;
        unordered_map<string, int> m;
        helper(root, result, m);
        return result;
    }
private:
    string helper(TreeNode *root, vector<TreeNode*> &result, unordered_map<string,int> &m)
    {
        string cur = "#";
        if (!root) return cur;
        cur = to_string(root -> val) + ' ' + helper(root -> left, result, m) + ' ' + helper(root -> right, result, m);
        if (m[cur] == 1) result.push_back(root);
        ++m[cur];
        return cur;
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
    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        List<TreeNode> result = new ArrayList<>();
        Map<String, Integer> map = new HashMap<>();
        helper(root, result, map);
        return result;
    }
    
    private String helper(TreeNode root, List<TreeNode> result, Map<String, Integer> m) {
        String cur = "#";
        if (root == null) return cur;
        cur = String.valueOf(root.val) + ' ' + helper(root.left, result, m) + ' ' + helper(root.right, result, m);
        if (m.getOrDefault(cur, 0) == 1) result.add(root);
        m.put(cur, m.getOrDefault(cur, 0) + 1);
        return cur;
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
    def findDuplicateSubtrees(self, root: TreeNode) -> List[TreeNode]:
        result, m = [], defaultdict(int)
        def helper(root: TreeNode) -> str:
            cur = "#"
            if not root:
                return cur
            cur = str(root.val) + ' ' + helper(root.left) + ' ' + helper(root.right)
            if m[cur] == 1:
                result.append(root)
            m[cur] += 1
            return cur
        helper(root)
        return result
```
