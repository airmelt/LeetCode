# 107 Binary Tree Level Order Traversal II 二叉树的层次遍历 II

__Description__:
Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

__Example__:

Given binary tree [3,9,20,null,null,15,7],

```text
    3
   / \
  9  20
    /  \
   15   7
```

return its bottom-up level order traversal as:

```text
[
  [15,7],
  [9,20],
  [3]
]
```

__题目描述__:
给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

__示例__：

给定二叉树 [3,9,20,null,null,15,7],

```text
    3
   / \
  9  20
    /  \
   15   7
```

返回其自底向上的层次遍历为：

```text
[
  [15,7],
  [9,20],
  [3]
]
```

__思路__:

主体思想是采用[BFS(广度优先搜索)](https://en.wikipedia.org/wiki/Breadth-first_search), 使用队列记录每层结点

1. 迭代, 每次遍历新的一层的时候, 记录下该层结点的数量(队列的长度), 加入到列表中, 最后插入列表的开始位置即可
2. 递归, 用二维数组同时记录下每次遍历的结点的数量和结点
时间复杂度O(n), 空间复杂度O(n), 其中 n为树中的结点数, 因为每个结点都要访问一次

[队列 queue-wiki](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))
> **队列**（queue），是[先进先出](https://zh.wikipedia.org/wiki/%E5%85%88%E9%80%B2%E5%85%88%E5%87%BA "先进先出")（FIFO, First-In-First-Out）的[线性表](https://zh.wikipedia.org/wiki/%E7%BA%BF%E6%80%A7%E8%A1%A8 "线性表")。
> 在具体应用中通常用[链表](https://zh.wikipedia.org/wiki/%E9%93%BE%E8%A1%A8 "链表")或者[数组](https://zh.wikipedia.org/wiki/%E6%95%B0%E7%BB%84 "数组")来实现。队列只允许在后端（称为*rear*）进行插入操作，在前端（称为*front*）进行删除操作。
> 现实生活中可以类比排队, 只能在队尾排队, 队首的先接受服务(弹出)
> 操作, 队列使用两种基本操作：在尾端(rear)插入和在前端(front)删除
> C++中的 STL的queue
> 常用函数有:
>
> ```C
> #include <queue>
> q.push(item);       //将item插入队尾  
> q.pop();            //删除队列前端的元素，但不会返回  
> q.front();          //返回队列前端的元素，但不会删除  
> q.size();           //返回队列中元素的个数  
> q.empty();          //检查队列是否为空，如果为空返回true，否则返回false   
> ```

[BFS(广度优先搜索)](https://en.wikipedia.org/wiki/Breadth-first_search)
> **广度优先搜索算法**（英语：Breadth-First-Search，缩写为BFS），又译作**宽度优先搜索**，或**横向优先搜索**，是一种**图形搜索算法**。
> 简单的说，BFS是从**根节点**开始。如果所有节点均被访问，则算法中止。广度优先搜索的实现一般采用open-closed表。
> BFS是一种[盲目搜索法](https://zh.wikipedia.org/w/index.php?title=%E7%9B%B2%E7%9B%AE%E6%90%9C%E5%B0%8B%E6%B3%95&action=edit&redlink=1 "盲目搜索法（页面不存在）")，目的是系统地展开并检查[图](https://zh.wikipedia.org/wiki/%E5%9B%BE "图")中的所有节点，以找寻结果。换句话说，它并不考虑结果的可能地址，彻底地搜索整张图，直到找到结果为止。BFS并不使用[经验法则算法](https://zh.wikipedia.org/wiki/%E5%90%AF%E5%8F%91%E5%BC%8F%E6%90%9C%E7%B4%A2 "启发式搜索")。
> 所有因为展开节点而得到的子节点都会被加进一个[先进先出](https://zh.wikipedia.org/wiki/%E5%85%88%E9%80%B2%E5%85%88%E5%87%BA "先进先出")的[队列](https://zh.wikipedia.org/wiki/%E9%98%9F%E5%88%97 "队列")中
> 一般的实现里，其邻居节点尚未被检验过的节点会被放置在一个被称为 *open* 的容器中（例如队列或是[链表](https://zh.wikipedia.org/wiki/%E9%80%A3%E7%B5%90%E4%B8%B2%E5%88%97 "链表")），而被检验过的节点则被放置在被称为 *closed* 的容器中。（open-closed表）
> 步骤:
>
> 1. 首先将根节点放入队列中。
> 2. 从队列中取出第一个节点，并检验它是否为目标。如果找到目标，则结束搜索并回传结果。否则将它所有尚未检验过的直接子节点加入队列中。
> 3. 若队列为空，表示整张图都检查过了——亦即图中没有欲搜索的目标。结束搜索并回传“找不到目标”。
> 4. 重复步骤2。

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
    vector<vector<int>> levelOrderBottom(TreeNode* root) 
    {
        vector<vector<int>> result;
        if (!root) return result;
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) 
        {
            vector<int> temp;
            int nums = q.size();
            while (nums) 
            {
                TreeNode* cur = q.front();
                q.pop();
                temp.push_back(cur -> val);
                if (cur -> left) q.push(cur -> left);
                if (cur -> right) q.push(cur -> right);
                nums--;
            }
            result.insert(result.begin(), temp);
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
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        levelOrderBottom(root, 0, result);
        return result;
    }

    private void levelOrderBottom(TreeNode root, int len, List<List<Integer>> result) {
        if (root == null) return;
        if (result.size() <= len) result.add(0, new ArrayList<>());
        result.get(result.size() - len - 1).add(root.val);
        levelOrderBottom(root.left, len + 1, result);
        levelOrderBottom(root.right, len + 1, result);
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
    def levelOrderBottom(self, root: TreeNode) -> List[List[int]]:
        if not root:
            return [];
        result = []
        queue = [root]
        while queue:
            temp = []
            nums = len(queue)
            while nums:
                cur = queue.pop(0)
                temp.append(cur.val)
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
                nums -= 1
            result.insert(0, temp)
        return result
```
