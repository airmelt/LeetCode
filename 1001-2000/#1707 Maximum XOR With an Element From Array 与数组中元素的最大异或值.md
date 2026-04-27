# 1707 Maximum XOR With an Element From Array 与数组中元素的最大异或值

__Description:__

You are given an array `nums` consisting of non-negative integers. You are also given a `queries` array, where `queries[i] = [xi, mi]`.

The answer to the `i ^ th` query is the maximum bitwise `XOR` value of `xi` and any element of `nums` that does not exceed `mi`. In other words, the answer is `max(nums[j] XOR xi)` for all `j` such that `nums[j] <= mi`. If all elements in `nums` are larger than `mi`, then the answer is `-1`.

Return _an integer array_ `answer` _where_ `answer.length == queries.length` _and_ `answer[i]` _is the answer to the_ `i ^ th` _query._

__Example:__

Example 1:

```text
Input: nums = [0,1,2,3,4], queries = [[3,1],[1,3],[5,6]]
Output: [3,3,7]
Explanation:
1) 0 and 1 are the only two integers not greater than 1. 0 XOR 3 = 3 and 1 XOR 3 = 2. The larger of the two is 3.
2) 1 XOR 2 = 3.
3) 5 XOR 2 = 7.
```

Example 2:

```text
Input: nums = [5,2,4,6,6,3], queries = [[12,4],[8,1],[6,3]]
Output: [15,-1,5]
```

__Constraints:__

- `1 <= nums.length, queries.length <= 10 ^ 5`
- `queries[i].length == 2`
- `0 <= nums[j], xi, mi <= 10 ^ 9`

__题目描述:__

给你一个由非负整数组成的数组 `nums` 。另有一个查询数组 `queries` ，其中 `queries[i] = [xi, mi]` 。

第 `i` 个查询的答案是 `xi` 和任何 `nums` 数组中不超过 `mi` 的元素按位异或（ `XOR`）得到的最大值。换句话说，答案是 `max(nums[j] XOR xi)` ，其中所有 `j` 均满足 `nums[j] <= mi` 。如果 `nums` 中的所有元素都大于 `mi`，最终答案就是 `-1` 。

返回一个整数数组 `answer` 作为查询的答案，其中 `answer.length == queries.length` 且 `answer[i]` 是第 `i` 个查询的答案。

__示例:__

示例 1：

```text
输入：nums = [0,1,2,3,4], queries = [[3,1],[1,3],[5,6]]
输出：[3,3,7]
解释：
1) 0 和 1 是仅有的两个不超过 1 的整数。0 XOR 3 = 3 而 1 XOR 3 = 2 。二者中的更大值是 3 。
2) 1 XOR 2 = 3.
3) 5 XOR 2 = 7.
```

示例 2：

```text
输入：nums = [5,2,4,6,6,3], queries = [[12,4],[8,1],[6,3]]
输出：[15,-1,5]
```

__提示：__

- `1 <= nums.length, queries.length <= 10 ^ 5`
- `queries[i].length == 2`
- `0 <= nums[j], xi, mi <= 10 ^ 9`

__思路:__

```text
前缀树
由于需要检查查询中不超过 m 的结果
我们按照 m 给查询数组排序, 并记录对应的原查询数组的下标
给 nums 数组排序, 保证插入到前缀树中的是有序的
查询时, 从最小的查询开始, 依次检查, 每次给 nums 的下标加 1 直到超过 nums 的长度
时间复杂度为 O(NlogN + QlogQ), 空间复杂度为 O(N + Q)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> maximizeXor(vector<int>& nums, vector<vector<int>>& queries) 
    {
        sort(nums.begin(), nums.end());
        int n = nums.size(), q = queries.size(), i = 0;
        for (int j = 0; j < q; j++) queries[j].emplace_back(j);
        sort(queries.begin(), queries.end(), [](const auto& a, const auto& b) { return a[1] < b[1]; });
        root = new Trie();
        vector<int> result(q, -1);
        for (const auto& query : queries)
        {
            int x = query[0], m = query[1], qid = query[2];
            while (i < n and nums[i] <= m) insert(nums[i++]);
            if (i) result[qid] = helper(x);
        }
        return result;
    }
private:
    struct Trie
    {
        Trie *left;
        Trie *right;
        Trie(): left(nullptr), right(nullptr) {}
    };
    
    Trie *root;
    
    void insert(int val)
    {
        Trie *cur = root;
        for (int i = 30; i > -1; i--)
        {
            if (val & (1 << i))
            {
                if (cur -> right) cur = cur -> right;
                else
                {
                    cur -> right = new Trie();
                    cur = cur -> right;
                }
            }
            else
            {
                if (cur -> left) cur = cur -> left;
                else
                {
                    cur -> left = new Trie();
                    cur = cur -> left;
                }
            }
        }
    }
    
    int helper(int val)
    {
        int result = 0;
        Trie *cur = root;
        for (int i = 30; i > -1; i--)
        {
            if (val & (1 << i))
            {
                if (cur -> left)
                {
                    result |= (1 << i);
                    cur = cur -> left;
                }
                else cur = cur -> right;
            }
            else
            {
                if (cur -> right) 
                {
                    result |= (1 << i);
                    cur = cur -> right;
                }
                else cur = cur -> left;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] maximizeXor(int[] nums, int[][] queries) {
        int n = nums.length, q = queries.length, i = 0, result[] = new int[q];
        Arrays.fill(result, -1);
        Integer[] idxs = new Integer[q];
        for (int j = 0; j < q; j++) idxs[j] = j;
        Arrays.sort(nums);
        Arrays.sort(idxs, Comparator.comparingInt(o -> queries[o][1]));
        Arrays.sort(queries, Comparator.comparingInt(o -> o[1]));
        root = new Trie();
        for (int j = 0; j < q; j++) {
            int x = queries[j][0], m = queries[j][1], qid = idxs[j];
            while (i < n && nums[i] <= m) insert(nums[i++]);
            if (i != 0) result[qid] = helper(x);
        }
        return result;
    }
    
    private class Trie {
        Trie left;
        Trie right;
    }
    
    private Trie root;
    
    private void insert(int val) {
        Trie cur = root;
        for (int i = 30; i > -1; i--) {
            if ((val & (1 << i)) != 0) {
                if (cur.right != null) cur = cur.right;
                else {
                    cur.right = new Trie();
                    cur = cur.right;
                }
            }
            else {
                if (cur.left != null) cur = cur.left;
                else {
                    cur.left = new Trie();
                    cur = cur.left;
                }
            }
        }
    }
    
    private int helper(int val) {
        int result = 0;
        Trie cur = root;
        for (int i = 30; i > -1; i--) {
            if ((val & (1 << i)) != 0) {
                if (cur.left != null) {
                    result |= (1 << i);
                    cur = cur.left;
                } else cur = cur.right;
            } else {
                if (cur.right != null) {
                    result |= (1 << i);
                    cur = cur.right;
                } else cur = cur.left;
            }
        }
        return result;
    }
};
```

__Python__:

```Python
class Trie:
    def __init__(self):
        self.left = None
        self.right = None

    def insert(self, val: int) -> None:
        node = self
        for i in range(30, -1, -1):
            if not (val >> i) & 1:
                if not node.left:
                    node.left = Trie()
                node = node.left
            else:
                if not node.right:
                    node.right = Trie()
                node = node.right
    
    def helper(self, val: int) -> int:
        result, node = 0, self
        for i in range(30, -1, -1):
            if not (val >> i) & 1:
                if node.right:
                    node = node.right
                    result |= 1 << i
                else:
                    node = node.left
            else:
                if node.left:
                    node = node.left
                    result |= 1 << i
                else:
                    node = node.right
        return result


class Solution:
    def maximizeXor(self, nums: List[int], queries: List[List[int]]) -> List[int]:
        n, result, root, i, queries = len(nums), [-1] * (q := len(queries)), Trie(), 0, sorted([(x, m, i) for i, (x, m) in enumerate(queries)], key=lambda query: query[1])
        nums.sort()
        for x, m, qid in queries:
            while i < n and nums[i] <= m:
                root.insert(nums[i])
                i += 1
            if i:
                result[qid] = root.helper(x)
        return result
```
