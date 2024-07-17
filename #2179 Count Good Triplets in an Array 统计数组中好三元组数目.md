# 2179 Count Good Triplets in an Array 统计数组中好三元组数目

__Description:__

You are given two __0-indexed__ arrays `nums1` and `nums2` of length `n`, both of which are __permutations__ of `[0, 1, ..., n - 1]`.

A __good triplet__ is a set of `3` __distinct__ values which are present in __increasing order__ by position both in `nums1` and `nums2`. In other words, if we consider `pos1v` as the index of the value `v` in `nums1` and `pos2v` as the index of the value `v` in `nums2`, then a good triplet will be a set `(x, y, z)` where `0 <= x, y, z <= n - 1`, such that `pos1x < pos1y < pos1z` and `pos2x < pos2y < pos2z`.

Return the total number of good triplets.

__Example:__

Example 1:

```text
Input: nums1 = [2,0,1,3], nums2 = [0,1,2,3]
Output: 1
Explanation: 
There are 4 triplets (x,y,z) such that pos1x < pos1y < pos1z. They are (2,0,1), (2,0,3), (2,1,3), and (0,1,3). 
Out of those triplets, only the triplet (0,1,3) satisfies pos2x < pos2y < pos2z. Hence, there is only 1 good triplet.
```

Example 2:

```text
Input: nums1 = [4,0,1,3,2], nums2 = [4,1,0,2,3]
Output: 4
Explanation: The 4 good triplets are (4,0,3), (4,0,2), (4,1,3), and (4,1,2).
```

__Constraints:__

- `n == nums1.length == nums2.length`
- `3 <= n <= 10 ^ 5`
- `0 <= nums1[i], nums2[i] <= n - 1`
- `nums1` and `nums2` are permutations of `[0, 1, ..., n - 1]`.

__题目描述:__

给你两个下标从 __0__ 开始且长度为 `n` 的整数数组 `nums1` 和 `nums2` ，两者都是 `[0, 1, ..., n - 1]` 的 __排列__ 。

__好三元组__ 指的是 `3` 个 __互不相同__ 的值，且它们在数组 `nums1` 和 `nums2` 中出现顺序保持一致。换句话说，如果我们将 `pos1v` 记为值 `v` 在 `nums1` 中出现的位置， `pos2v` 为值 `v` 在 `nums2` 中的位置，那么一个好三元组定义为 `0 <= x, y, z <= n - 1` ，且 `pos1x < pos1y < pos1z` 和 `pos2x < pos2y < pos2z` 都成立的 `(x, y, z)` 。

请你返回好三元组的 总数目 。

__示例:__

示例 1：

```text
输入：nums1 = [2,0,1,3], nums2 = [0,1,2,3]
输出：1
解释：
总共有 4 个三元组 (x,y,z) 满足 pos1x < pos1y < pos1z ，分别是 (2,0,1) ，(2,0,3) ，(2,1,3) 和 (0,1,3) 。
这些三元组中，只有 (0,1,3) 满足 pos2x < pos2y < pos2z 。所以只有 1 个好三元组。
```

示例 2：

```text
输入：nums1 = [4,0,1,3,2], nums2 = [4,1,0,2,3]
输出：4
解释：总共有 4 个好三元组 (4,0,3) ，(4,0,2) ，(4,1,3) 和 (4,1,2) 。
```

__提示：__

- `n == nums1.length == nums2.length`
- `3 <= n <= 10 ^ 5`
- `0 <= nums1[i], nums2[i] <= n - 1`
- `nums1` 和 `nums2` 是 `[0, 1, ..., n - 1]` 的排列。

__思路:__

```text
映射 ➕ 树状数组
如果 nums1 = [0, 1, 2, ..., n - 1]
那么只需要考虑枚举 nums2 中的 y, 计算 y 的左边有多少个数 left 小于 y, 以及右边有多少个数 right 大于 y
则对于 y 来说贡献为 left * right
设 y 的下标为 left
注意到 y 的左边有 i - left 比 y 大的数, 比 y 大的数总共有 n - 1 - y 个, 则 y 的右边比 y 大的还有 n - 1 - i - (i - left) 个数
则 y 的贡献为 left * (n - 1 - y - (i - left))
将 nums1 映射到 [0, 1, 2, ..., n - 1] 的下标
对应可以将 nums2 也同样映射
比如 nums1 = [4, 0, 1, 3, 2], nums2 = [4, 1, 0, 2, 3]
即
    [4, 0, 1, 3, 2]
=>  [0, 1, 2, 3, 4]
    [4, 1, 0, 2, 3]
=>  [0, 2, 1, 4, 3]
利用树状数组每次求有多少个数在 y 的左边并且比 y 小
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long goodTriplets(vector<int>& nums1, vector<int>& nums2) 
    {
        int n = nums1.size();
        vector<int> p(n), tree(n + 1);
        for (int i = 0; i < n; i++) p[nums1[i]] = i;
        long long result = 0LL;
        for (int i = 1; i < n - 1; i++) 
        {
            for (int j = p[nums2[i - 1]] + 1; j <= n; j += j & -j) ++tree[j];
            int y = p[nums2[i]], left = 0;
            for (int j = y; j > 0; j &= j - 1) left += tree[j];
            result += left * (n - 1LL - y - (i - left));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution:
    def goodTriplets(self, nums1: List[int], nums2: List[int]) -> int:
        from sortedcontainers import SortedList
        p, result, s, n = {x: i for i, x in enumerate(nums1)}, 0, SortedList(), len(nums1)
        for i in range(1, n - 1):
            s.add(p[nums2[i - 1]])
            result += (left := s.bisect_left(y := p[nums2[i]])) * (n - 1 - y - (i - left))
        return result
```

__Python__:

```Python
class Solution:
    def goodTriplets(self, nums1: List[int], nums2: List[int]) -> int:
        from sortedcontainers import SortedList
        p, result, s, n = {x: i for i, x in enumerate(nums1)}, 0, SortedList(), len(nums1)
        for i in range(1, n - 1):
            s.add(p[nums2[i - 1]])
            result += (left := s.bisect_left(y := p[nums2[i]])) * (n - 1 - y - (i - left))
        return result
```
