# 988 Smallest String Starting From Leaf 从叶结点开始的最小字符串

__Description__:
You are given the root of a binary tree where each node has a value in the range [0, 25] representing the letters 'a' to 'z'.

Return the lexicographically smallest string that starts at a leaf of this tree and ends at the root.

As a reminder, any shorter prefix of a string is lexicographically smaller.

For example, "ab" is lexicographically smaller than "aba".
A leaf of a node is a node that has no children.

__Example:__

Example 1:

![tree1](https://assets.leetcode.com/uploads/2019/01/30/tree1.png)

Input: root = [0,1,2,3,4,3,4]
Output: "dba"

Example 2:

![tree2](https://assets.leetcode.com/uploads/2019/01/30/tree2.png)

Input: root = [25,1,3,1,3,0,2]
Output: "adz"

Example 3:

![tree3](https://assets.leetcode.com/uploads/2019/02/01/tree3.png)

Input: root = [2,2,1,null,1,0,null,0]
Output: "abc"

__Constraints:__

The number of nodes in the tree is in the range [1, 8500].
0 <= Node.val <= 25

__题目描述__:
给定一颗根结点为 root 的二叉树，树中的每一个结点都有一个从 0 到 25 的值，分别代表字母 'a' 到 'z'：值 0 代表 'a'，值 1 代表 'b'，依此类推。

找出按字典序最小的字符串，该字符串从这棵树的一个叶结点开始，到根结点结束。

（小贴士：字符串中任何较短的前缀在字典序上都是较小的：例如，在字典序上 "ab" 比 "aba" 要小。叶结点是指没有子结点的结点。）

__示例 :__

示例 1：

![树1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/02/tree1.png)

输入：[0,1,2,3,4,3,4]
输出："dba"
示例 2：

![树2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/02/tree2.png)

输入：[25,1,3,1,3,0,2]
输出："adz"
示例 3：

![树3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/02/tree3.png)

输入：[2,2,1,null,1,0,null,0]
输出："abc"

__提示:__

给定树的结点数介于 1 和 8500 之间。
树中的每个结点都有一个介于 0 和 25 之间的值。

__思路__:

模拟
遍历每一个叶子结点
每次找到更小的就替换
时间复杂度为 O(nh), 空间复杂度为 O(n), h 为二叉树的高度

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
    string smallestFromLeaf(TreeNode* root) 
    {
        string result = "", cur = "";
        dfs(root, cur, result);
        return result;
    }
private:
    void dfs(TreeNode* root, string& cur, string& result)
    {
        if (!root) return;
        cur += (char)('a' + root -> val);
        dfs(root -> left, cur, result);
        dfs(root -> right, cur, result);
        if (!root -> left and !root -> right)
        {
            reverse(cur.begin(), cur.end());
            if (result.size() == 0 or result > cur) result = cur;
            reverse(cur.begin(), cur.end());
        }
        cur.erase(cur.end() - 1);
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
    private String result = null;
    
    public String smallestFromLeaf(TreeNode root) {
        dfs(root, new StringBuilder());
        return result;
    }
    
    public void dfs(TreeNode root, StringBuilder sb) {
        if (root == null) return;
        sb.append((char)(root.val+'a'));
        dfs(root.left, sb);
        dfs(root.right, sb);
        if (root.left == null && root.right == null) {
            String cur = sb.reverse().toString();
            sb.reverse();
            if (result == null || result.compareTo(cur) > 0) result = cur;
        }
        sb.deleteCharAt(sb.length()-1);
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
    def smallestFromLeaf(self, root: Optional[TreeNode]) -> str:
        result, cur = '', ''
        
        def dfs(r: Optional[TreeNode]) -> None:
            nonlocal result, cur
            if not r:
                return
            cur += chr(r.val + ord('a'))
            dfs(r.left)
            dfs(r.right)
            if not r.left and not r.right:
                
                if not result or result > cur[::-1]:
                    result = cur[::-1]
            cur = cur[:-1]
        
        dfs(root)
        return result
```
