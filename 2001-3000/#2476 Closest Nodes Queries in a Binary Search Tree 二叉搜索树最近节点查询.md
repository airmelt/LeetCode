# 2476 Closest Nodes Queries in a Binary Search Tree 二叉搜索树最近节点查询

__Description:__

You are given the `root` of a __binary search tree__ and an array `queries` of size `n` consisting of positive integers.

Find a __2D__ array `answer` of size `n` where `answer[i] = [mini, maxi]`:

- `mini` is the __largest__ value in the tree that is smaller than or equal to `queries[i]`. If a such value does not exist, add `-1` instead.
- `maxi` is the __smallest__ value in the tree that is greater than or equal to `queries[i]`. If a such value does not exist, add `-1` instead.

Return _the array_ `answer`.

__Example:__

Example 1:

![2476-1](https://assets.leetcode.com/uploads/2022/09/28/bstreeedrawioo.png)

```text
Input: root = [6,2,13,1,4,9,15,null,null,null,null,null,null,14], queries = [2,5,16]
Output: [[2,2],[4,6],[15,-1]]
Explanation: We answer the queries in the following way:
```

- The largest number that is smaller or equal than 2 in the tree is 2, and the smallest number that is greater or equal than 2 is still 2. So the answer for the first query is [2,2].
- The largest number that is smaller or equal than 5 in the tree is 4, and the smallest number that is greater or equal than 5 is 6. So the answer for the second query is [4,6].
- The largest number that is smaller or equal than 16 in the tree is 15, and the smallest number that is greater or equal than 16 does not exist. So the answer for the third query is [15,-1].

Example 2:

![2476-2](https://assets.leetcode.com/uploads/2022/09/28/bstttreee.png)

```text
Input: root = [4,null,9], queries = [3]
Output: [[-1,4]]
Explanation: The largest number that is smaller or equal to 3 in the tree does not exist, and the smallest number that is greater or equal to 3 is 4. So the answer for the query is [-1,4].
```

__Constraints:__

- The number of nodes in the tree is in the range `[2, 10 ^ 5]`.
- `1 <= Node.val <= 10 ^ 6`
- `n == queries.length`
- `1 <= n <= 10 ^ 5`
- `1 <= queries[i] <= 10 ^ 6`

__题目描述:__

给你一个 __二叉搜索树__ 的根节点 `root` ，和一个由正整数组成、长度为 `n` 的数组 `queries` 。

请你找出一个长度为 `n` 的 __二维__ 答案数组 `answer` ，其中 `answer[i] = [mini, maxi]` :

- `mini` 是树中小于等于 `queries[i]` 的 __最大值__ 。如果不存在这样的值，则使用 `-1` 代替。
- `maxi` 是树中大于等于 `queries[i]` 的 __最小值__ 。如果不存在这样的值，则使用 `-1` 代替。

返回数组 `answer` 。

__示例:__

示例 1 ：

![2476-3](https://assets.leetcode.com/uploads/2022/09/28/bstreeedrawioo.png)

```text
输入：root = [6,2,13,1,4,9,15,null,null,null,null,null,null,14], queries = [2,5,16]
输出：[[2,2],[4,6],[15,-1]]
解释：按下面的描述找出并返回查询的答案：
```

- 树中小于等于 2 的最大值是 2 ，且大于等于 2 的最小值也是 2 。所以第一个查询的答案是 [2,2] 。
- 树中小于等于 5 的最大值是 4 ，且大于等于 5 的最小值是 6 。所以第二个查询的答案是 [4,6] 。
- 树中小于等于 16 的最大值是 15 ，且大于等于 16 的最小值不存在。所以第三个查询的答案是 [15,-1] 。

示例 2 ：

![2476-4](https://assets.leetcode.com/uploads/2022/09/28/bstttreee.png)

```text
输入：root = [4,null,9], queries = [3]
输出：[[-1,4]]
解释：树中不存在小于等于 3 的最大值，且大于等于 3 的最小值是 4 。所以查询的答案是 [-1,4] 。
```

__提示：__

- 树中节点的数目在范围 `[2, 10 ^ 5]` 内
- `1 <= Node.val <= 10 ^ 6`
- `n == queries.length`
- `1 <= n <= 10 ^ 5`
- `1 <= queries[i] <= 10 ^ 6`

__思路:__

```text
二分查找
由于二叉树不保证平衡
每次查询是 O(N)
我们可以先将二叉树的值存入一个数组
使用中序遍历, 这样这个数组就是有序的
然后我们可以使用二分查找来找到小于等于 queries[i] 的最大值
和大于等于 queries[i] 的最小值
注意判断边界条件
时间复杂度为 O(N + QlogN), 空间复杂度为 O(N)
```

__代码:__

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
    vector<vector<int>> closestNodes(TreeNode* root, vector<int>& queries) 
    {
        
        vector<vector<int>> result;
        dfs(root);
        int n = queries.size(), m = nums.size();       
        for (int i = 0; i < n; i++)
        {
            int it = lower_bound(nums.begin(), nums.end(), queries[i]) - nums.begin(), x = it < m ? nums[it] : -1;
            if (it == m or nums[it] != queries[i]) --it;
            result.emplace_back(vector<int>{it > -1 ? nums[it] : -1, x});
        }
        return result;
    }
private:
    void dfs(TreeNode* cur)
    {
        if (cur)
        {
            dfs(cur -> left);
            nums.emplace_back(cur -> val);
            dfs(cur -> right);
        }
    }

    vector<int> nums;
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
    public List<List<Integer>> closestNodes(TreeNode root, List<Integer> queries) {
        List<Integer> nums = new ArrayList<Integer>();
        List<List<Integer>> result = new ArrayList<>();
        dfs(root, nums);
        for (int i : queries) {
            int pos = Collections.binarySearch(nums, i), index = pos < 0 ? -pos - 1 : pos, x = index == nums.size() ? -1 : nums.get(index);
            if (index == nums.size() || nums.get(index) != i) --index;
            result.add(List.of(index < 0 ? -1 : nums.get(index), x));
        }
        return result;
    }

    private void dfs(TreeNode cur, List<Integer> nums) {
        if (cur != null) {
            dfs(cur.left, nums);
            nums.add(cur.val);
            dfs(cur.right, nums);
        }
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
    def closestNodes(self, root: Optional[TreeNode], queries: List[int]) -> List[List[int]]:
        nums = []

        def dfs(cur: Optional[TreeNode]) -> NoReturn:
            if cur:
                dfs(cur.left)
                nums.append(cur.val)
                dfs(cur.right)
        dfs(root)
        n = len(nums)
        return [[nums[l] if (l := bisect_left(nums, q)) < n and nums[l] == q else nums[l - 1] if l > 0 else -1, nums[l] if l < n else nums[l + 1] if l < n - 1 else -1] for q in queries]
```
