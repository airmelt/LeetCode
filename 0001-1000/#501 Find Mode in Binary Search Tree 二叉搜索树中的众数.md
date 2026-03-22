# 501 Find Mode in Binary Search Tree 二叉搜索树中的众数

__Description__:
Given a binary search tree (BST) with duplicates, find all the mode(s) (the most frequently occurred element) in the given BST.

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than or equal to the node's key.
The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
Both the left and right subtrees must also be binary search trees.

__Example:__

For example:
Given BST [1,null,2,2],

```text
   1
    \
     2
    /
   2
```

return [2].

__Note:__
If a tree has more than one mode, you can return them in any order.

__Follow up:__
Could you do that without using any extra space? (Assume that the implicit stack space incurred due to recursion does not count).

__题目描述__:
给定一个有相同值的二叉搜索树（BST），找出 BST 中的所有众数（出现频率最高的元素）。

假定 BST 有如下定义：

结点左子树中所含结点的值小于等于当前结点的值
结点右子树中所含结点的值大于等于当前结点的值
左子树和右子树都是二叉搜索树

__示例：__

例如：
给定 BST [1,null,2,2],

```text
   1
    \
     2
    /
   2
```

返回[2].

__提示：__
如果众数超过1个，不需考虑输出顺序

__进阶：__
你可以不使用额外的空间吗？（假设由递归产生的隐式调用栈的开销不被计算在内）

__思路__:

二叉搜索树的中序遍历可以返回一个升序的数组

1. 中序遍历时, 对该数进行数量判断, 满足众数条件的就加入(进阶, 不考虑栈的额外开销)
时间复杂度O(n), 空间复杂度O(1)
2. 中序遍历的结果存储在数组中, 再判断众数
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
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution 
{
private:
    void in_order(TreeNode* root, TreeNode*& pre, int& cur_count, int& max_count, vector<int>& result) 
    {
        if (!root) return;
        in_order(root -> left, pre, cur_count, max_count, result);
        if (pre) cur_count = (root -> val == pre -> val) ? cur_count + 1 : 1;
        if (cur_count == max_count) result.push_back(root -> val);
        else if (cur_count > max_count) 
        {
            result.clear();
            result.push_back(root -> val);
            max_count = cur_count;
        }
        pre = root;
        in_order(root -> right, pre, cur_count, max_count, result);
    }
public:
    vector<int> findMode(TreeNode* root) 
    {
        vector<int> result;
        if (!root) return result;
        TreeNode* pre = NULL;
        int cur_count = 1, max_count = 0;
        in_order(root, pre, cur_count, max_count, result);
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
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int[] findMode(TreeNode root) {
        Map<Integer, Integer> map = new HashMap<>();
        inOrder(root, map);
        int max = 0;
        List<Integer> list = new ArrayList<>();
        for (Integer value : map.values()) if (max < value) max = value;
        for (Integer key : map.keySet()) if (map.get(key) == max) list.add(key);
        int result[] = new int[list.size()];
        for (int i = 0; i < list.size(); i++) result[i] = list.get(i);
        return result;
    }

    private void inOrder(TreeNode root, Map<Integer, Integer> map) {
        if (root == null) return;
        inOrder(root.left, map);
        map.put(root.val, map.getOrDefault(root.val, 0) + 1);
        inOrder(root.right, map);
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
from collections import Counter
class Solution:
    def findMode(self, root: TreeNode) -> List[int]:
        result = []
        if not root:
            return result
        self.in_order(root, result)
        dic = Counter(result)
        max = 0
        for k in dic:
            if dic[k] > max:
                max = dic[k]
        result = []
        for k in dic:
            if dic[k] == max:
                result.append(k)
        return result

    def in_order(self, root: TreeNode, result: List[int]):
        if not root:
            return
        self.in_order(root.left, result)
        result.append(root.val)
        self.in_order(root.right, result)
```
