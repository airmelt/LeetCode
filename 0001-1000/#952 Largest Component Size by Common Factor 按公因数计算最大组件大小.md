# 952 Largest Component Size by Common Factor 按公因数计算最大组件大小

__Description__:
You are given an integer array of unique positive integers nums. Consider the following graph:

There are nums.length nodes, labeled nums[0] to nums[nums.length - 1],
There is an undirected edge between nums[i] and nums[j] if nums[i] and nums[j] share a common factor greater than 1.
Return the size of the largest connected component in the graph.

__Example:__

Example 1:

![ex1](https://assets.leetcode.com/uploads/2018/12/01/ex1.png)

Input: nums = [4,6,15,35]
Output: 4

Example 2:

![ex2](https://assets.leetcode.com/uploads/2018/12/01/ex2.png)

Input: nums = [20,50,9,63]
Output: 2

Example 3:

![ex3](https://assets.leetcode.com/uploads/2018/12/01/ex3.png)

Input: nums = [2,3,6,7,4,12,21,39]
Output: 8

__Constraints:__

1 <= nums.length <= 2 * 10^4
1 <= nums[i] <= 10^5
All the values of nums are unique.

__题目描述__:
给定一个由不同正整数的组成的非空数组 A，考虑下面的图：

有 A.length 个节点，按从 A[0] 到 A[A.length - 1] 标记；
只有当 A[i] 和 A[j] 共用一个大于 1 的公因数时，A[i] 和 A[j] 之间才有一条边。
返回图中最大连通组件的大小。

__示例 :__

示例 1：

输入：[4,6,15,35]
输出：4

![ex1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/01/ex1.png)

示例 2：

输入：[20,50,9,63]
输出：2

![ex2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/01/ex2.png)

示例 3：

输入：[2,3,6,7,4,12,21,39]
输出：8

![ex3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/01/ex3.png)

__提示:__

1 <= A.length <= 20000
1 <= A[i] <= 100000

__思路__:

并查集
用筛法或者遍历法找到有公因子的两个结点加入同一棵树
统计 parent 数组中在原数组中出现的元素对应的最大值
时间复杂度为 O(n), 空间复杂度为 O(n), 其中 n 表示数组中的最大元素

__代码__:
__C++__:

```C++
class Solution 
{
private:
    int visited[100005], count[100005], parent[100005];
    
    int find(int p)
    {
        return p == parent[p] ? p : parent[p] = find(parent[p]);
    }
public:
    int largestComponentSize(vector<int>& nums) 
    {
        int max_value = *max_element(nums.begin(), nums.end()) + 1;
        for (int i = 0; i < max_value; i++) parent[i] = i;
        for (const auto& num : nums) visited[num] = true;
        for (int i = 2; i < max_value; i++) for (int j = i << 1; j < max_value; j += i) if (visited[j]) parent[find(j)] = find(i);
        for (int i = 0; i < max_value; i++) if (visited[i]) ++count[find(i)];
        return *max_element(count, count + max_value);
    }
};
```

__Java__:

```Java
class Solution {
    private int[] parent;
    
    public int largestComponentSize(int[] nums) {
        int n = nums.length, maxValue = nums[0], result = 0;
        for (int num : nums) maxValue = Math.max(num, maxValue);
        parent = new int[maxValue + 1];
        int[] count = new int[maxValue + 1], visited = new int[maxValue + 1];
        for (int i = 0; i <= maxValue; i++) parent[i] = i;
        for (int num : nums) visited[num] = 1;
        for (int i = 2; i <= maxValue; i++) for (int j = (i << 1); j <= maxValue; j += i) if (visited[j] == 1) parent[find(j)] = find(i);
        for (int i = 0; i <= maxValue; i++) if (visited[i] == 1) ++count[find(i)];
        for (int num : count) result = Math.max(num, result);
        return result;
    }
    
    private int find(int p) {
        return (p == parent[p] ? p : (parent[p] = find(parent[p])));
    }
}
```

__Python__:

```Python
class UF:
    def __init__(self, n: int) -> None:
        self.parent = [i for i in range(n)]
        self.weight = [1] * n
        self.count = n

    def union(self, p: int, q: int) -> None:
        """
        连接两个点
        :param p: 一个节点
        :param q: 另一个节点
        :return:
        """
        p = self.find(p)
        q = self.find(q)
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
    def largestComponentSize(self, nums: List[int]) -> int:
        uf = UF(max(nums) + 1)
        for i in nums:
            for j in range(2, int(i ** 0.5) + 1):
                if not i % j:
                    uf.union(j, i)
                    uf.union(j, i // j)
        return max(Counter(uf.find(i) for i in nums).values())
```
