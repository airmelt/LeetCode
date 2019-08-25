__Description__:
Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.

__Example:__
Example 1:

Input: 
```
    5
   / \
  3   6
 / \   \
2   4   7
```
Target = 9

Output: True
 

Example 2:

Input: 
```
    5
   / \
  3   6
 / \   \
2   4   7
```
Target = 28

Output: False

__题目描述__:
给定一个二叉搜索树和一个目标结果，如果 BST 中存在两个元素且它们的和等于给定的目标结果，则返回 true。

__示例 :__
案例 1:

输入: 
```
    5
   / \
  3   6
 / \   \
2   4   7
```
Target = 9

输出: True
 

案例 2:

输入: 
```
    5
   / \
  3   6
 / \   \
2   4   7
```
Target = 28

输出: False

__思路__:
1. 遍历 BST, 将结点的值存入 set中, 每次判断元素是否在 set内即可, 可以通过剪枝加快搜索速度
2. 根据 BST的特性, 将其中序遍历转化成升序数组用双指针判断
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool findTarget(TreeNode* root, int k) {
        in_order(root);
        int i = 0, j = list.size() - 1;
        while (i < j) {
            if (list[i] + list[j] == k) return true;
            else if (list[i] + list[j] > k) j--;
            else if (list[i] + list[j] < k) i++;
        }
        return false;
    }
private:
    vector<int> list;
    void in_order(TreeNode* root) {
        if (root) {
            in_order(root -> left);
            list.push_back(root -> val);
            in_order(root -> right);
        }
    }
};
```

__Java__:
```
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
    public boolean findTarget(TreeNode root, int k) {
        Set<Integer> set = new HashSet<>();
        return preOrder(root, set, k);
    }
    
    private boolean preOrder(TreeNode root, Set<Integer> set, int k) {
        if (root == null) return false;
        if (set.contains(k - root.val)) return true;
        set.add(root.val);
        return preOrder(root.left, set, k) || preOrder(root.right, set, k);
    }
}
```

__Python__:
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def findTarget(self, root: TreeNode, k: int) -> bool:
        d, result = {}, False
        def search(root):
            nonlocal result
            if not root or result:
                return
            if root.val in d:
                result = True
            d[k - root.val] = True
            search(root.left)
            search(root.right)
        search(root)
        return result
```