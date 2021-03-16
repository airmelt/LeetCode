# 508 Most Frequent Subtree Sum 出现次数最多的子树元素和

__Description__:
Given the root of a tree, you are asked to find the most frequent subtree sum. The subtree sum of a node is defined as the sum of all the node values formed by the subtree rooted at that node (including the node itself). So what is the most frequent subtree sum value? If there is a tie, return all the values with the highest frequency in any order.

__Example:__

Examples 1
Input:

```text
  5
 /  \
2   -3
```

return [2, -3, 4], since all the values happen only once, return all of them in any order.

Examples 2
Input:

```text
  5
 /  \
2   -5
```

return [2], since 2 happens twice, however -5 only occur once.
__Note:__
You may assume the sum of values in any subtree is in the range of 32-bit signed integer.

__题目描述__:
给你一个二叉树的根结点，请你找出出现次数最多的子树元素和。一个结点的「子树元素和」定义为以该结点为根的二叉树上所有结点的元素之和（包括结点本身）。

你需要返回出现次数最多的子树元素和。如果有多个元素出现的次数相同，返回所有出现次数最多的子树元素和（不限顺序）。

__示例 :__

示例 1：
输入:

```text
  5
 /  \
2   -3
```

返回 [2, -3, 4]，所有的值均只出现一次，以任意顺序返回所有值。

示例 2：
输入：

```text
  5
 /  \
2   -5
```

返回 [2]，只有 2 出现两次，-5 只出现 1 次。

__提示:__
假设任意子树元素和均可以用 32 位有符号整数表示。

__思路__:

递归
找到每一个以本节点为根的子树的和, 存放在哈希表中
遍历哈希表找到最大值, 再次遍历将等于最大值的 key放入结果列表
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
public:
    vector<int> findFrequentTreeSum(TreeNode* root) 
    {
        unordered_map<int, int> m;
        dfs(root, m);
        int max_value = 0;
        for (auto &p : m) max_value = max(p.second, max_value);
        vector<int> result;
        for (auto &p : m) if (p.second == max_value) result.emplace_back(p.first);
        return result;
    }
private:
    int dfs(TreeNode* root, unordered_map<int, int> &m) 
    {
        if (!root) return 0;
        int result = dfs(root -> left, m) + dfs(root -> right, m) + root -> val;
        ++m[result];
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
    public int[] findFrequentTreeSum(TreeNode root) {
        Map<Integer, Integer> map = new HashMap<>();
        dfs(root, map);
        int max = 0;
        for (int i : map.values()) max = Math.max(max, i);
        List<Integer> list = new LinkedList<>();
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) if (entry.getValue() == max) list.add(entry.getKey());
        int result[] = new int[list.size()];
        for (int i = 0; i < list.size(); i++) result[i] = list.get(i);
        return result;
    }
    
    private int dfs(TreeNode root, Map<Integer, Integer> map) {
        if (root == null) return 0;
        int result = dfs(root.left, map) + dfs(root.right, map) + root.val;
        map.put(result, map.getOrDefault(result, 0) + 1);
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
    def findFrequentTreeSum(self, root: TreeNode) -> List[int]:
        d = defaultdict(int)
        def dfs(cur: TreeNode) -> int:
            nonlocal m
            if not cur:
                return 0
            result = dfs(cur.left) + dfs(cur.right) + cur.val
            d[result] += 1
            return result
        dfs(root)
        m = max(d.values(), default=0)
        return [i for i in d.keys() if d[i] == m]
```
