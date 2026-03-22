# 2709 Greatest Common Divisor Traversal 最大公约数遍历

__Description:__

You are given a __0-indexed__ integer array `nums`, and you are allowed to __traverse__ between its indices. You can traverse between index `i` and index `j`, `i != j`, if and only if `gcd(nums[i], nums[j]) > 1`, where `gcd` is the __greatest common divisor__.

Your task is to determine if for __every pair__ of indices `i` and `j` in nums, where `i < j`, there exists a __sequence of traversals__ that can take us from `i` to `j`.

Return `true` _if it is possible to traverse between all such pairs of indices,_ _or_ `false` _otherwise._

__Example:__

Example 1:

```text
Input: nums = [2,3,6]
Output: true
Explanation: In this example, there are 3 possible pairs of indices: (0, 1), (0, 2), and (1, 2).
To go from index 0 to index 1, we can use the sequence of traversals 0 -> 2 -> 1, where we move from index 0 to index 2 because gcd(nums[0], nums[2]) = gcd(2, 6) = 2 > 1, and then move from index 2 to index 1 because gcd(nums[2], nums[1]) = gcd(6, 3) = 3 > 1.
To go from index 0 to index 2, we can just go directly because gcd(nums[0], nums[2]) = gcd(2, 6) = 2 > 1. Likewise, to go from index 1 to index 2, we can just go directly because gcd(nums[1], nums[2]) = gcd(3, 6) = 3 > 1.
```

Example 2:

```text
Input: nums = [3,9,5]
Output: false
Explanation: No sequence of traversals can take us from index 0 to index 2 in this example. So, we return false.
```

Example 3:

```text
Input: nums = [4,3,12,8]
Output: true
Explanation: There are 6 possible pairs of indices to traverse between: (0, 1), (0, 2), (0, 3), (1, 2), (1, 3), and (2, 3). A valid sequence of traversals exists for each pair, so we return true.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，你可以在一些下标之间遍历。对于两个下标 `i` 和 `j`（ `i != j`），当且仅当 `gcd(nums[i], nums[j]) > 1` 时，我们可以在两个下标之间通行，其中 `gcd` 是两个数的 __最大公约数__ 。

你需要判断 `nums` 数组中 __任意__ 两个满足 `i < j` 的下标 `i` 和 `j` ，是否存在若干次通行可以从 `i` 遍历到 `j` 。

如果任意满足条件的下标对都可以遍历，那么返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

```text
输入：nums = [2,3,6]
输出：true
解释：这个例子中，总共有 3 个下标对：(0, 1) ，(0, 2) 和 (1, 2) 。
从下标 0 到下标 1 ，我们可以遍历 0 -> 2 -> 1 ，我们可以从下标 0 到 2 是因为 gcd(nums[0], nums[2]) = gcd(2, 6) = 2 > 1 ，从下标 2 到 1 是因为 gcd(nums[2], nums[1]) = gcd(6, 3) = 3 > 1 。
从下标 0 到下标 2 ，我们可以直接遍历，因为 gcd(nums[0], nums[2]) = gcd(2, 6) = 2 > 1 。同理，我们也可以从下标 1 到 2 因为 gcd(nums[1], nums[2]) = gcd(3, 6) = 3 > 1 。
```

示例 2：

```text
输入：nums = [3,9,5]
输出：false
解释：我们没法从下标 0 到 2 ，所以返回 false 。
```

示例 3：

```text
输入：nums = [4,3,12,8]
输出：true
解释：总共有 6 个下标对：(0, 1) ，(0, 2) ，(0, 3) ，(1, 2) ，(1, 3) 和 (2, 3) 。所有下标对之间都存在可行的遍历，所以返回 true 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__思路:__

```text
并查集
用筛法将所有数字做质因数分解
把相同质因数的用并查集连在一起
最后检查是否所有的数字都被连在一起
时间复杂度为 O(NlogM), 空间复杂度为 O(N), M 为 max(nums)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canTraverseAllPairs(vector<int>& nums) 
    {
        int n = nums.size(), count = n;
        vector<int> parent(n);
        iota(parent.begin(), parent.end(), 0);
        unordered_map<int, vector<int>> m;
        for (int i = 0; i < n; i++) 
        {
            for (int d = 2, total = nums[i]; d * d <= total; d++) 
            {
                if (!(nums[i] % d)) 
                {
                    m[d].emplace_back(i);
                    while (!(nums[i] % d)) nums[i] /= d;
                }
            }
            if (nums[i] > 1) m[nums[i]].emplace_back(i);
        }
        auto find = [&](auto&& find, int a) -> int 
        {
            return a == parent[a] ? a : (parent[a] = find(find, parent[a]));
        };
        auto connect = [&](int a, int b) -> bool
        {
            if ((a = find(find, a)) == (b = find(find, b))) return false;
            parent[a] = b;
            return true;
        };
        for (const auto& [_, v] : m) for (const auto& a : v) if (connect(a, v.front())) --count;
        return count == 1;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canTraverseAllPairs(int[] nums) {
        int n = nums.length, parent[] = new int[n], count = n;
        var map = new HashMap<Integer, List<Integer>>();
        for (int i = 1; i < n; i++) parent[i] = i;
        for (int i = 0; i < n; i++) {
            for (int d = 2, total = nums[i]; d * d <= total; d++) {
                if (nums[i] % d == 0) {
                    map.computeIfAbsent(d, k -> new ArrayList<>()).add(i);
                    while (nums[i] % d == 0) nums[i] /= d;
                }
            }
            if (nums[i] > 1) map.computeIfAbsent(nums[i], k -> new ArrayList<>()).add(i);
        }
        for (var v : map.values()) for (int a : v) if (union(a, v.get(0), parent)) --count;
        return count == 1;
    }

    private boolean union(int a, int b, int[] parent) {
        if ((a = find(a, parent)) == (b = find(b, parent))) return false;
        parent[a] = b;
        return true;
    }

    private int find(int a, int[] parent) {
        return a == parent[a] ? a : (parent[a] = find(parent[a], parent));
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
    def canTraverseAllPairs(self, nums: List[int]) -> bool:
        m, uf = defaultdict(list), UF(n := len(nums))
        for i, num in enumerate(nums):
            for d in range(2, int(num ** 0.5) + 1):
                if not num % d:
                    m[d].append(i)
                    while not num % d:
                        num //= d
            if num > 1:
                m[num].append(i)
        for v in m.values():
            for a, b in pairwise(v):
                uf.union(a, b)
        return uf.count == 1
```
