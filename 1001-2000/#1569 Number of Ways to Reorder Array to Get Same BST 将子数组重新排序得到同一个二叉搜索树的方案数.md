# 1569 Number of Ways to Reorder Array to Get Same BST 将子数组重新排序得到同一个二叉搜索树的方案数

__Description:__

Given an array `nums` that represents a permutation of integers from `1` to `n`. We are going to construct a binary search tree (BST) by inserting the elements of `nums` in order into an initially empty BST. Find the number of different ways to reorder `nums` so that the constructed BST is identical to that formed from the original array `nums`.

- For example, given `nums = [2,1,3]`, we will have 2 as the root, 1 as a left child, and 3 as a right child. The array `[2,3,1]` also yields the same BST but `[3,2,1]` yields a different BST.

Return _the number of ways to reorder_ `nums` _such that the BST formed is identical to the original BST formed from_ `nums`.

Since the answer may be very large, __return it modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

![1569-1](https://assets.leetcode.com/uploads/2020/08/12/bb.png)

```text
Input: nums = [2,1,3]
Output: 1
Explanation: We can reorder nums to be [2,3,1] which will yield the same BST. There are no other ways to reorder nums which will yield the same BST.
```

Example 2:

![1569-2](https://assets.leetcode.com/uploads/2020/08/12/ex1.png)

```text
Input: nums = [3,4,5,1,2]
Output: 5
Explanation: The following 5 arrays will yield the same BST: 
[3,1,2,4,5]
[3,1,4,2,5]
[3,1,4,5,2]
[3,4,1,2,5]
[3,4,1,5,2]
```

Example 3:

![1569-3](https://assets.leetcode.com/uploads/2020/08/12/ex4.png)

```text
Input: nums = [1,2,3]
Output: 0
Explanation: There are no other orderings of nums that will yield the same BST.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= nums.length`
- All integers in `nums` are __distinct__.

__题目描述:__

给你一个数组 `nums` 表示 `1` 到 `n` 的一个排列。我们按照元素在 `nums` 中的顺序依次插入一个初始为空的二叉搜索树（BST）。请你统计将 `nums` 重新排序后，统计满足如下条件的方案数：重排后得到的二叉搜索树与 `nums` 原本数字顺序得到的二叉搜索树相同。

比方说，给你 `nums = [2,1,3]`，我们得到一棵 2 为根，1 为左孩子，3 为右孩子的树。数组 `[2,3,1]` 也能得到相同的 BST，但 `[3,2,1]` 会得到一棵不同的 BST 。

请你返回重排 `nums` 后，与原数组 `nums` 得到相同二叉搜索树的方案数。

由于答案可能会很大，请将结果对 `10 ^ 9 + 7` 取余数。

__示例:__

示例 1：

![1569-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/30/bb.png)

```text
输入：nums = [2,1,3]
输出：1
解释：我们将 nums 重排， [2,3,1] 能得到相同的 BST 。没有其他得到相同 BST 的方案了。
```

示例 2：

![1569-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/30/ex1.png)

```text
输入：nums = [3,4,5,1,2]
输出：5
解释：下面 5 个数组会得到相同的 BST：
[3,1,2,4,5]
[3,1,4,2,5]
[3,1,4,5,2]
[3,4,1,2,5]
[3,4,1,5,2]
```

示例 3：

![1569-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/30/ex4.png)

```text
输入：nums = [1,2,3]
输出：0
解释：没有别的排列顺序能得到相同的 BST 。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= nums.length`
- `nums` 中所有数 __互不相同__ 。

__思路:__

```text
1. 递归
分别对左子树和右子树进行递归计算
计算公式为 C(left, left + right), left, right 分别为左右子树的大小, C 表示组合数
递归计算直到子树大小小于等于 1, 递归终点返回 0
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
2. 乘法逆元 ➕ 并查集
一般用到组合数的计算可以考虑使用乘法逆元优化
利用费马小定理, b ≡ a ^ (m − 2)(mod m), m 为质数
这样在计算除法的时候能够利用乘法代替 c / a ≡ cb(mod m)
预处理 fac[i] = i! % m 以及 fac_inv[i] = (i!) ^ (m - 2) % m
计算组合数 C(n, k) = fac[n] * fac_inv[k] * fac_inv[n - k]
记录 inv[i] = i ^ -1, 则 fac_inv[i] = fac_inv[i - 1] * inv[i]
inv[i] = - m / i * inv[m % i] % m = (m - m / i) * inv[m % i] % m, 加上 m 保证 inv 非负
在遍历的时候从后往前遍历, 那么前面的元素是后面元素的父节点
并用一个数组记录所有节点的根节点, 在遍历时更新
这样就可以把所有值集中在根节点
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    long long C[1002][1002];
    const static long long MOD = 1e9 + 7;
    
    long long dfs(vector<int> & nums)
    {
        if (nums.size() <= 1) return 1;
        vector<int> left, right;
        for (int i = 1, n = nums.size(); i < n; i++)
        {
            if (nums[i] < nums.front()) left.emplace_back(nums[i]);
            else right.emplace_back(nums[i]);
        }
        return ((C[left.size() + right.size()][right.size()] * dfs(left)) % MOD * dfs(right)) % MOD;
    }
public:
    int numOfWays(vector<int>& nums) 
    {
        int n = nums.size();
        memset(C, 0, sizeof(C));
        for (int j = 0; j <= n; j++) 
        {
            C[j][j] = 1;
            for (int i = j + 1; i <= n; i++) C[i][j] = j ? (C[i - 1][j - 1] + C[i - 1][j]) % MOD : 1;
        }
        return dfs(nums) - 1;
    }
};
```

__Java__:

```Java
class Solution {
    private long C[][] = new long[1002][1002];
    private final static long MOD = 1_000_000_007;
    
    long dfs(List<Integer> nums) {
        if (nums.size() <= 1) return 1;
        List<Integer> left = new ArrayList<>(), right = new ArrayList<>();
        for (int i = 1, n = nums.size(); i < n; i++) {
            if (nums.get(i) < nums.get(0)) left.add(nums.get(i));
            else right.add(nums.get(i));
        }
        int l = left.size(), r = right.size();
        return ((C[l + r][r] * dfs(left)) % MOD * dfs(right)) % MOD;
    }
    
    public int numOfWays(int[] nums) {
        int n = nums.length;
        for (int j = 0; j <= n; j++) {
            C[j][j] = 1;
            for (int i = j + 1; i <= n; i++) C[i][j] = j != 0 ? (C[i - 1][j - 1] + C[i - 1][j]) % MOD : 1;
        }
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < n; i++) list.add(nums[i]);
        return (int)dfs(list) - 1;
    }
}
```

__Python__:

```Python
class Node:
    def __init__(self):
        self.left = None
        self.right = None
        self.size = 1
        self.result = 0

class UF:
    def __init__(self, n: int) -> None:
        self.parent = [i for i in range(n)]
        self.root = [i for i in range(n)]
        self.weight = [1] * n
        self.count = n
        
    def get_root(self, p: int) -> int:
        return self.root[self.find(p)]

    def union(self, p: int, q: int) -> None:
        """
        连接两个点
        :param p: 一个节点
        :param q: 另一个节点
        :return:
        """
        p = self.find(p)
        q = self.find(q)
        self.root[q] = self.root[p]
        if p == q:
            return
        if self.weight[p] > self.weight[q]:
            self.parent[q] = p
            self.weight[p] += self.weight[q]
        else:
            self.parent[p] = q
            self.weight[q] += self.weight[p]
        self.count -= 1

    def connected(self, p: int, q: int) -> bool:
        """
        检查两个点是否在同一分量
        :param p: 一个节点
        :param q: 另一个节点
        :return: 返回两个点是否在同一个分量
        """
        return self.find(p) == self.find(q)

    def find(self, p: int) -> int:
        """
        查找根节点, 并进行路径压缩
        :param p: 一个节点
        :return: 根节点
        """
        while self.parent[p] != p:
            self.parent[p] = self.parent[self.parent[p]]
            p = self.parent[p]
        return p

class Solution:
    def numOfWays(self, nums: List[int]) -> int:
        if (n := len(nums)) == 1:
            return 0
        mod, fac, inv, fac_inv, uf, nodes = 10 ** 9 + 7, [0] * n, [0] * n, [0] * n, UF(n), {}
        fac[0] = inv[0] = fac_inv[0] = fac[1] = inv[1] = fac_inv[1] = 1
        for i in range(2, n):
            fac[i], inv[i] = fac[i - 1] * i % mod, (mod - mod // i) * inv[mod % i] % mod
            fac_inv[i] = fac_inv[i - 1] * inv[i] % mod
        for i in range(n - 1, -1, -1):
            val, node = nums[i] - 1, Node()
            if val > 0 and val - 1 in nodes:
                node.left = nodes[left := uf.get_root(val - 1)]
                node.size += node.left.size
                uf.union(val, left)
            if val < n - 1 and val + 1 in nodes:
                node.right = nodes[right := uf.get_root(val + 1)]
                node.size += node.right.size
                uf.union(val, right)
            left_size, right_size, left_result, right_result = node.left.size if node.left else 0, node.right.size if node.right else 0, node.left.result if node.left else 1, node.right.result if node.right else 1
            node.result = fac[left_size + right_size] * fac_inv[left_size] * fac_inv[right_size] * left_result * right_result % mod
            nodes[val] = node
        return (nodes[nums[0] - 1].result - 1 + mod) % mod
```
