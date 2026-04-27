# 2382 Maximum Segment Sum After Removals 删除操作后的最大子段和

__Description:__

You are given two __0-indexed__ integer arrays `nums` and `removeQueries`, both of length `n`. For the `i ^ th` query, the element in `nums` at the index `removeQueries[i]` is removed, splitting `nums` into different segments.

A __segment__ is a contiguous sequence of __positive__ integers in `nums`. A __segment sum__ is the sum of every element in a segment.

Return _an integer array_ `answer`_, of length_ `n`_, where_ `answer[i]` _is the __maximum__ segment sum after applying the_ `i ^ th` _removal._

Note: The same index will not be removed more than once.

__Example:__

Example 1:

```text
Input: nums = [1,2,5,6,1], removeQueries = [0,3,2,4,1]
Output: [14,7,2,2,0]
Explanation: Using 0 to indicate a removed element, the answer is as follows:
Query 1: Remove the 0th element, nums becomes [0,2,5,6,1] and the maximum segment sum is 14 for segment [2,5,6,1].
Query 2: Remove the 3rd element, nums becomes [0,2,5,0,1] and the maximum segment sum is 7 for segment [2,5].
Query 3: Remove the 2nd element, nums becomes [0,2,0,0,1] and the maximum segment sum is 2 for segment [2]. 
Query 4: Remove the 4th element, nums becomes [0,2,0,0,0] and the maximum segment sum is 2 for segment [2]. 
Query 5: Remove the 1st element, nums becomes [0,0,0,0,0] and the maximum segment sum is 0, since there are no segments.
Finally, we return [14,7,2,2,0].
```

Example 2:

```text
Input: nums = [3,2,11,1], removeQueries = [3,2,1,0]
Output: [16,5,3,0]
Explanation: Using 0 to indicate a removed element, the answer is as follows:
Query 1: Remove the 3rd element, nums becomes [3,2,11,0] and the maximum segment sum is 16 for segment [3,2,11].
Query 2: Remove the 2nd element, nums becomes [3,2,0,0] and the maximum segment sum is 5 for segment [3,2].
Query 3: Remove the 1st element, nums becomes [3,0,0,0] and the maximum segment sum is 3 for segment [3].
Query 4: Remove the 0th element, nums becomes [0,0,0,0] and the maximum segment sum is 0, since there are no segments.
Finally, we return [16,5,3,0].
```

__Constraints:__

- `n == nums.length == removeQueries.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `0 <= removeQueries[i] < n`
- All the values of `removeQueries` are __unique__.

__题目描述:__

给你两个下标从 __0__ 开始的整数数组 `nums` 和 `removeQueries` ，两者长度都为 `n` 。对于第 `i` 个查询， `nums` 中位于下标 `removeQueries[i]` 处的元素被删除，将 `nums` 分割成更小的子段。

一个 __子段__ 是 `nums` 中连续 __正__ 整数形成的序列。__子段和__ 是子段中所有元素的和。

请你返回一个长度为 `n` 的整数数组 `answer` ，其中 `answer[i]`是第 `i` 次删除操作以后的 __最大__ 子段和。

注意：一个下标至多只会被删除一次。

__示例:__

示例 1：

```text
输入：nums = [1,2,5,6,1], removeQueries = [0,3,2,4,1]
输出：[14,7,2,2,0]
解释：用 0 表示被删除的元素，答案如下所示：
查询 1 ：删除第 0 个元素，nums 变成 [0,2,5,6,1] ，最大子段和为子段 [2,5,6,1] 的和 14 。
查询 2 ：删除第 3 个元素，nums 变成 [0,2,5,0,1] ，最大子段和为子段 [2,5] 的和 7 。
查询 3 ：删除第 2 个元素，nums 变成 [0,2,0,0,1] ，最大子段和为子段 [2] 的和 2 。
查询 4 ：删除第 4 个元素，nums 变成 [0,2,0,0,0] ，最大子段和为子段 [2] 的和 2 。
查询 5 ：删除第 1 个元素，nums 变成 [0,0,0,0,0] ，最大子段和为 0 ，因为没有任何子段存在。
所以，我们返回 [14,7,2,2,0] 。
```

示例 2：

```text
输入：nums = [3,2,11,1], removeQueries = [3,2,1,0]
输出：[16,5,3,0]
解释：用 0 表示被删除的元素，答案如下所示：
查询 1 ：删除第 3 个元素，nums 变成 [3,2,11,0] ，最大子段和为子段 [3,2,11] 的和 16 。
查询 2 ：删除第 2 个元素，nums 变成 [3,2,0,0] ，最大子段和为子段 [3,2] 的和 5 。
查询 3 ：删除第 1 个元素，nums 变成 [3,0,0,0] ，最大子段和为子段 [3] 的和 3 。
查询 5 ：删除第 0 个元素，nums 变成 [0,0,0,0] ，最大子段和为 0 ，因为没有任何子段存在。
所以，我们返回 [16,5,3,0] 。
```

__提示：__

- `n == nums.length == removeQueries.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `0 <= removeQueries[i] < n`
- `removeQueries` 中所有数字 __互不相同__ 。

__思路:__

```text
并查集
正序删除比较麻烦
考虑使用倒序将需要删除的元素添加回去
设一个哑节点使得数组能够从 0 个元素开始构建
每次只合并 x = removeQueries[i] 以及 p = find(x + 1)
并将子段的和计入 sum[p]
sum[p] += sum[x] + nums[x]
这样 result[i - 1] 要么为上一个加入的值 result[i], 要么为这一次加入的子段 sum[p]
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<long long> maximumSegmentSum(vector<int>& nums, vector<int>& removeQueries) 
    {
        int n = nums.size();
        vector<int> parent(n + 1);
        iota(parent.begin(), parent.end(), 0);
        vector<long long> sum(n + 1), result(n);
        function<int(int)> find = [&](int x) -> int { return parent[x] == x ? parent[x] : (parent[x] = find(parent[x])); };
        for (int i = n - 1, x = removeQueries[i], p = 0; i > 0; x = removeQueries[--i])
        {
            parent[x] = (p = find(x + 1));
            sum[p] += sum[x] + nums[x];
            result[i - 1] = max(result[i], sum[p]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int[] parent;

    public long[] maximumSegmentSum(int[] nums, int[] removeQueries) {
        int n = nums.length;
        parent = new int[n + 1];
        for (int i = 0; i <= n; i++) parent[i] = i;
        long[] sum = new long[n + 1], result = new long[n];
        for (int i = n - 1, x = removeQueries[i], p = 0; i > 0; x = removeQueries[--i]) {
            parent[x] = (p = find(x + 1));
            sum[p] += sum[x] + nums[x];
            result[i - 1] = Math.max(result[i], sum[p]);
        }
        return result;
    }

    private int find(int x) {
        return parent[x] == x ? parent[x] : (parent[x] = find(parent[x]));
    }
}
```

__Python__:

```Python
class UF:
    def __init__(self, n: int) -> None:
        self.parent = [i for i in range(n)]
        self.weight = [1] * n
        self.sum = [0] * n
        self.count = n

    def union(self, p: int, q: int) -> None:
        """
        连接两个点
        :param p: 一个节点
        :param q: 另一个节点
        :return:
        """
        p = self.find(p)
        self.parent[q] = p
        self.sum[p] += self.sum[q]
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
    def maximumSegmentSum(self, nums: List[int], removeQueries: List[int]) -> List[int]:
        uf, result = UF((n := len(nums)) + 1), [0] * n
        for i in range(n - 1, 0, -1):
            uf.union(p := uf.find((x := removeQueries[i]) + 1), x)
            uf.sum[p] += nums[x]
            result[i - 1] = max(result[i], uf.sum[p])
        return result
```
