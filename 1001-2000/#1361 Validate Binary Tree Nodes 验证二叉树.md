# 1361 Validate Binary Tree Nodes 验证二叉树

__Description:__

You have n binary tree nodes numbered from 0 to n - 1 where node i has two children leftChild[i] and rightChild[i], return true if and only if all the given nodes form exactly one valid binary tree.

If node i has no left child then leftChild[i] will equal -1, similarly for the right child.

Note that the nodes have no values and that we only use the node numbers in this problem.

__Example:__

Example 1:

![Binary Tree 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex1.png)

Input: n = 4, leftChild = [1,-1,3,-1], rightChild = [2,-1,-1,-1]
Output: true

Example 2:

![Binary Tree 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex2.png)

Input: n = 4, leftChild = [1,-1,3,-1], rightChild = [2,3,-1,-1]
Output: false

Example 3:

![Binary Tree 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex3.png)

Input: n = 2, leftChild = [1,0], rightChild = [-1,-1]
Output: false

__Constraints:__

n == leftChild.length == rightChild.length
1 <= n <= 10^4
-1 <= leftChild[i], rightChild[i] <= n - 1

__题目描述：__

二叉树上有 n 个节点，按从 0 到 n - 1 编号，其中节点 i 的两个子节点分别是 leftChild[i] 和 rightChild[i]。

只有 所有 节点能够形成且 只 形成 一颗 有效的二叉树时，返回 true；否则返回 false。

如果节点 i 没有左子节点，那么 leftChild[i] 就等于 -1。右子节点也符合该规则。

注意：节点没有值，本问题中仅仅使用节点编号。

__示例：__

示例 1：

![二叉树 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex1.png)

输入：n = 4, leftChild = [1,-1,3,-1], rightChild = [2,-1,-1,-1]
输出：true

示例 2：

![二叉树 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex2.png)

输入：n = 4, leftChild = [1,-1,3,-1], rightChild = [2,3,-1,-1]
输出：false

示例 3：

![二叉树 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex3.png)

输入：n = 2, leftChild = [1,0], rightChild = [-1,-1]
输出：false

示例 4：

![二叉树 4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex4.png)

输入：n = 6, leftChild = [1,-1,-1,4,-1,-1], rightChild = [2,-1,-1,5,-1,-1]
输出：false

__提示：__

1 <= n <= 10^4
leftChild.length == rightChild.length == n
-1 <= leftChild[i], rightChild[i] <= n - 1

__思路：__

BFS
通过计算入度找到入度为 0 的节点, 该结点可能为根结点
从根结点出发, 层序遍历整个二叉树
如果没有环结束循环
最后比较遍历过的结点数和 n 的大小
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    bool validateBinaryTreeNodes(int n, vector<int>& leftChild, vector<int>& rightChild) 
    {
        vector<int> indegree(n);
        for (int i = 0; i < n; ++i) 
        {
            if (leftChild[i] != -1) ++indegree[leftChild[i]];
            if (rightChild[i] != -1) ++indegree[rightChild[i]];
        }
        int root = -1;
        for (int i = 0; i < n; i++) 
        {
            if (!indegree[i]) 
            {
                root = i;
                break;
            }
        }
        if (root == -1) return false;
        unordered_set<int> visited;
        queue<int> q;
        visited.insert(root);
        q.push(root);
        while (!q.empty()) 
        {
            int cur = q.front();
            q.pop();
            if (leftChild[cur] != -1) 
            {
                if (visited.count(leftChild[cur])) return false;
                visited.insert(leftChild[cur]);
                q.push(leftChild[cur]);
            }
            if (rightChild[cur] != -1) 
            {
                if (visited.count(rightChild[cur])) return false;
                visited.insert(rightChild[cur]);
                q.push(rightChild[cur]);
            }
        }
        return visited.size() == n;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean validateBinaryTreeNodes(int n, int[] leftChild, int[] rightChild) {
        int root = -1, indegree[] = new int[n];
        for (int i = 0; i < n; i++) {
            if (leftChild[i] != -1) ++indegree[leftChild[i]];
            if (rightChild[i] != -1) ++indegree[rightChild[i]];
        }
        for (int i = 0; i < n; i++) {
            if (indegree[i] == 0) {
                root = i;
                break;
            }
        }
        if (root == -1) return false;
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(root);
        Set<Integer> visited = new HashSet<>();
        visited.add(root);
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            if (leftChild[cur] != -1) {
                if (visited.contains(leftChild[cur])) return false;
                visited.add(leftChild[cur]);
                queue.offer(leftChild[cur]);
            }
            if (rightChild[cur] != -1) {
                if (visited.contains(rightChild[cur])) return false;
                visited.add(rightChild[cur]);
                queue.offer(rightChild[cur]);
            }
        }
        return visited.size() == n;
    }
}
```

__Python__:

```Python
class Solution:
    def validateBinaryTreeNodes(self, n: int, leftChild: List[int], rightChild: List[int]) -> bool:
        root, indegree = -1, Counter(leftChild[i] for i in range(n)) + Counter(rightChild[i] for i in range(n))
        for i in range(n):
            if not indegree[i]:
                root = i
                break
        if root == -1:
            return False
        queue, visited = deque([root]), {root}
        while queue:
            cur = queue.popleft()
            if leftChild[cur] != -1:
                if leftChild[cur] in visited:
                    return False
                queue.append(leftChild[cur])
                visited.add(leftChild[cur])
            if rightChild[cur] != -1:
                if rightChild[cur] in visited:
                    return False
                queue.append(rightChild[cur])
                visited.add(rightChild[cur])
        return len(visited) == n
```
