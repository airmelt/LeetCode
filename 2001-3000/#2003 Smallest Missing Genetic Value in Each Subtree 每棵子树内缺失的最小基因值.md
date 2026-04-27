# 2003 Smallest Missing Genetic Value in Each Subtree 每棵子树内缺失的最小基因值

__Description:__

There is a __family tree__ rooted at `0` consisting of `n` nodes numbered `0` to `n - 1`. You are given a __0-indexed__ integer array `parents`, where `parents[i]` is the parent for node `i`. Since node `0` is the __root__, `parents[0] == -1`.

There are `10 ^ 5` genetic values, each represented by an integer in the __inclusive__ range `[1, 10 ^ 5]`. You are given a __0-indexed__ integer array `nums`, where `nums[i]` is a __distinct__ genetic value for node `i`.

Return _an array_ `ans` _of length_ `n` _where_ `ans[i]` _is_ _the __smallest__ genetic value that is __missing__ from the subtree rooted at node_ `i`.

The __subtree__ rooted at a node `x` contains node `x` and all of its __descendant__ nodes.

__Example:__

Example 1:

![2003-1](https://assets.leetcode.com/uploads/2021/08/23/case-1.png)

```text
Input: parents = [-1,0,0,2], nums = [1,2,3,4]
Output: [5,1,1,1]
Explanation: The answer for each subtree is calculated as follows:
```

- 0: The subtree contains nodes [0,1,2,3] with values [1,2,3,4]. 5 is the smallest missing value.
- 1: The subtree contains only node 1 with value 2. 1 is the smallest missing value.
- 2: The subtree contains nodes [2,3] with values [3,4]. 1 is the smallest missing value.
- 3: The subtree contains only node 3 with value 4. 1 is the smallest missing value.

Example 2:

![2003-2](https://assets.leetcode.com/uploads/2021/08/23/case-2.png)

```text
Input: parents = [-1,0,1,0,3,3], nums = [5,4,6,2,1,3]
Output: [7,1,1,4,2,1]
Explanation: The answer for each subtree is calculated as follows:
```

- 0: The subtree contains nodes [0,1,2,3,4,5] with values [5,4,6,2,1,3]. 7 is the smallest missing value.
- 1: The subtree contains nodes [1,2] with values [4,6]. 1 is the smallest missing value.
- 2: The subtree contains only node 2 with value 6. 1 is the smallest missing value.
- 3: The subtree contains nodes [3,4,5] with values [2,1,3]. 4 is the smallest missing value.
- 4: The subtree contains only node 4 with value 1. 2 is the smallest missing value.
- 5: The subtree contains only node 5 with value 3. 1 is the smallest missing value.

Example 3:

```text
Input: parents = [-1,2,3,0,2,4,1], nums = [2,3,4,5,6,7,8]
Output: [1,1,1,1,1,1,1]
Explanation: The value 1 is missing from all the subtrees.
```

__Constraints:__

- `n == parents.length == nums.length`
- `2 <= n <= 10 ^ 5`
- `0 <= parents[i] <= n - 1` for `i != 0`
- `parents[0] == -1`
- `parents` represents a valid tree.
- `1 <= nums[i] <= 10 ^ 5`
- Each `nums[i]` is distinct.

__题目描述:__

有一棵根节点为 `0` 的 __家族树__ ，总共包含 `n` 个节点，节点编号为 `0` 到 `n - 1` 。给你一个下标从 __0__ 开始的整数数组 `parents` ，其中 `parents[i]` 是节点 `i` 的父节点。由于节点 `0` 是 __根__ ，所以 `parents[0] == -1` 。

总共有 `10 ^ 5` 个基因值，每个基因值都用 __闭区间__ `[1, 10 ^ 5]` 中的一个整数表示。给你一个下标从 __0__ 开始的整数数组 `nums` ，其中 `nums[i]` 是节点 `i` 的基因值，且基因值 __互不相同__ 。

请你返回一个数组 `ans` ，长度为 `n` ，其中 `ans[i]` 是以节点 `i` 为根的子树内 _缺失_ 的 __最小__ 基因值。

节点 `x` 为根的 __子树__ 包含节点 `x` 和它所有的 __后代__ 节点。

__示例:__

示例 1：

![2003-3](https://assets.leetcode.com/uploads/2021/08/23/case-1.png)

```text
输入：parents = [-1,0,0,2], nums = [1,2,3,4]
输出：[5,1,1,1]
解释：每个子树答案计算结果如下：
```

- 0：子树包含节点 [0,1,2,3] ，基因值分别为 [1,2,3,4] 。5 是缺失的最小基因值。
- 1：子树只包含节点 1 ，基因值为 2 。1 是缺失的最小基因值。
- 2：子树包含节点 [2,3] ，基因值分别为 [3,4] 。1 是缺失的最小基因值。
- 3：子树只包含节点 3 ，基因值为 4 。1是缺失的最小基因值。

示例 2：

![2003-4](https://assets.leetcode.com/uploads/2021/08/23/case-2.png)

```text
输入：parents = [-1,0,1,0,3,3], nums = [5,4,6,2,1,3]
输出：[7,1,1,4,2,1]
解释：每个子树答案计算结果如下：
```

- 0：子树内包含节点 [0,1,2,3,4,5] ，基因值分别为 [5,4,6,2,1,3] 。7 是缺失的最小基因值。
- 1：子树内包含节点 [1,2] ，基因值分别为 [4,6] 。 1 是缺失的最小基因值。
- 2：子树内只包含节点 2 ，基因值为 6 。1 是缺失的最小基因值。
- 3：子树内包含节点 [3,4,5] ，基因值分别为 [2,1,3] 。4 是缺失的最小基因值。
- 4：子树内只包含节点 4 ，基因值为 1 。2 是缺失的最小基因值。
- 5：子树内只包含节点 5 ，基因值为 3 。1 是缺失的最小基因值。

示例 3：

```text
输入：parents = [-1,2,3,0,2,4,1], nums = [2,3,4,5,6,7,8]
输出：[1,1,1,1,1,1,1]
解释：所有子树都缺失基因值 1 。
```

__提示：__

- `n == parents.length == nums.length`
- `2 <= n <= 10 ^ 5`
- 对于 `i != 0` ，满足 `0 <= parents[i] <= n - 1`
- `parents[0] == -1`
- `parents` 表示一棵合法的树。
- `1 <= nums[i] <= 10 ^ 5`
- `nums[i]` 互不相同。

__思路:__

```text
DFS
如果基因值为 1 的节点不在子树内，则返回全 1
否则，从节点基因值为 1 的节点开始，遍历子树内的所有节点，记录已经出现的基因值
由于父节点一定包括所有子孙节点的基因值，所以只需要遍历到根节点即可
一边遍历一边将所有出现的基因值记录下来
如果基因值已经出现过, 则自增未出现过的值并赋给结果
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> smallestMissingValueSubtree(vector<int>& parents, vector<int>& nums) 
    {
        int n = parents.size(), min_value = *min_element(nums.begin(), nums.end()), cur = 2, node = 0;
        this -> nums = nums;
        vector<int> result(n, 1);
        graph.resize(n);
        if (min_value != 1) return result;
        for (int i = 1; i < n; i++) graph[parents[i]].emplace_back(i);
        for (int i = 0; i < n; i++) if (nums[i] == 1) node = i;
        while (~node)
        {
            dfs(node);
            while (visited.count(cur)) ++cur;
            result[node] = cur;
            node = parents[node];
        }
        return result;
    }
private:
    unordered_set<int> visited;
    vector<vector<int>> graph;
    vector<int> nums;
    void dfs(int u)
    {
        visited.insert(nums[u]);
        for (const auto& v : graph[u]) if (!visited.count(nums[v])) dfs(v);
    }
};
```

__Java__:

```Java
class Solution {
    public int[] smallestMissingValueSubtree(int[] parents, int[] nums) {
        int n = parents.length, result[] = new int[n];
        List<List<Integer>> children = new ArrayList<>(n);
        for (int i = 0; i < n; i++) children.add(new ArrayList<>());
        for (int i = 1; i < n; i++) children.get(parents[i]).add(i);
        dfs(0, result, children, nums);
        return result;
    }

    private BitSet dfs(int node, int[] result, List<List<Integer>> children, int[] nums) {
        BitSet bitSet = null;
        for (int child : children.get(node)) {
            if (bitSet == null) bitSet = dfs(child, result, children, nums);
            else bitSet.or(dfs(child, result, children, nums));
        }
        if (bitSet == null) bitSet = new BitSet();
        bitSet.set(nums[node]);
        result[node] = bitSet.nextClearBit(1);
        return bitSet;
    }
}
```

__Python__:

```Python
class Solution 
{
public:
    vector<int> smallestMissingValueSubtree(vector<int>& parents, vector<int>& nums) 
    {
        int n = parents.size(), min_value = *min_element(nums.begin(), nums.end()), cur = 2, node = 0;
        this -> nums = nums;
        vector<int> result(n, 1);
        graph.resize(n);
        if (min_value != 1) return result;
        for (int i = 1; i < n; i++) graph[parents[i]].emplace_back(i);
        for (int i = 0; i < n; i++) if (nums[i] == 1) node = i;
        while (~node)
        {
            dfs(node);
            while (visited.count(cur)) ++cur;
            result[node] = cur;
            node = parents[node];
        }
        return result;
    }
private:
    unordered_set<int> visited;
    vector<vector<int>> graph;
    vector<int> nums;
    void dfs(int u)
    {
        visited.insert(nums[u]);
        for (const auto& v : graph[u]) if (!visited.count(nums[v])) dfs(v);
    }
};
```
