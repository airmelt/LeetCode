# 3002 Maximum Size of a Set After Removals 移除后集合的最多元素数

__Description:__

You are given two __0-indexed__ integer arrays `nums1` and `nums2` of even length `n`.

You must remove `n / 2` elements from `nums1` and `n / 2` elements from `nums2`. After the removals, you insert the remaining elements of `nums1` and `nums2` into a set `s`.

Return _the __maximum__ possible size of the set_ `s`.

__Example:__

Example 1:

```text
Input: nums1 = [1,2,1,2], nums2 = [1,1,1,1]
Output: 2
Explanation: We remove two occurences of 1 from nums1 and nums2. After the removals, the arrays become equal to nums1 = [2,2] and nums2 = [1,1]. Therefore, s = {1,2}.
It can be shown that 2 is the maximum possible size of the set s after the removals.
```

Example 2:

```text
Input: nums1 = [1,2,3,4,5,6], nums2 = [2,3,2,3,2,3]
Output: 5
Explanation: We remove 2, 3, and 6 from nums1, as well as 2 and two occurrences of 3 from nums2. After the removals, the arrays become equal to nums1 = [1,4,5] and nums2 = [2,3,2]. Therefore, s = {1,2,3,4,5}.
It can be shown that 5 is the maximum possible size of the set s after the removals.
```

Example 3:

```text
Input: nums1 = [1,1,2,2,3,3], nums2 = [4,4,5,5,6,6]
Output: 6
Explanation: We remove 1, 2, and 3 from nums1, as well as 4, 5, and 6 from nums2. After the removals, the arrays become equal to nums1 = [1,2,3] and nums2 = [4,5,6]. Therefore, s = {1,2,3,4,5,6}.
It can be shown that 6 is the maximum possible size of the set s after the removals.
```

__Constraints:__

- `n == nums1.length == nums2.length`
- `1 <= n <= 2 * 10 ^ 4`
- `n` is even.
- `1 <= nums1[i], nums2[i] <= 10 ^ 9`

__题目描述:__

给你两个下标从 `0` 开始的整数数组 `nums1` 和 `nums2` ，它们的长度都是偶数 `n` 。

你必须从 `nums1` 中移除 `n / 2` 个元素，同时从 `nums2` 中也移除 `n / 2` 个元素。移除之后，你将 `nums1` 和 `nums2` 中剩下的元素插入到集合 `s` 中。

返回集合 `s`可能的 __最多__ 包含多少元素。

__示例:__

示例 1：

```text
输入：nums1 = [1,2,1,2], nums2 = [1,1,1,1]
输出：2
解释：从 nums1 和 nums2 中移除两个 1 。移除后，数组变为 nums1 = [2,2] 和 nums2 = [1,1] 。因此，s = {1,2} 。
可以证明，在移除之后，集合 s 最多可以包含 2 个元素。
```

示例 2：

```text
输入：nums1 = [1,2,3,4,5,6], nums2 = [2,3,2,3,2,3]
输出：5
解释：从 nums1 中移除 2、3 和 6 ，同时从 nums2 中移除两个 3 和一个 2 。移除后，数组变为 nums1 = [1,4,5] 和 nums2 = [2,3,2] 。因此，s = {1,2,3,4,5} 。
可以证明，在移除之后，集合 s 最多可以包含 5 个元素。
```

示例 3：

```text
输入：nums1 = [1,1,2,2,3,3], nums2 = [4,4,5,5,6,6]
输出：6
解释：从 nums1 中移除 1、2 和 3 ，同时从 nums2 中移除 4、5 和 6 。移除后，数组变为 nums1 = [1,2,3] 和 nums2 = [4,5,6] 。因此，s = {1,2,3,4,5,6} 。
可以证明，在移除之后，集合 s 最多可以包含 6 个元素。
```

__提示：__

- `n == nums1.length == nums2.length`
- `1 <= n <= 2 * 10 ^ 4`
- `n`是偶数。
- `1 <= nums1[i], nums2[i] <= 10 ^ 9`

__思路:__

```text
贪心
设 nums1 中有 a 个不同元素, nums2 中有 b 个不同元素, set(nums1) 和 set(nums2) 的交集有 c 个元素
如果不移除元素
则一共有 a + b - c 个元素
设 m = n >> 1
1. 移除元素
先在 nums1 中移除元素
可以先在交集中移除
如果交集不够 m, 可以全部移除完, 剩下的从 m - c 个自己的独特元素中移除
如果交集超过 m, 那最后会剩下 c - m 个元素还需要进一步移除
总的来说需要从交集中移除 min(m, c) 个元素
移除这些元素之后如果 a > m, 还需要移除 a - m 个元素
nums2 同理
时间复杂度为 O(N), 空间复杂度为 O(N)
2. 添加元素
对于 nums1 优先选不在交集中的元素, 可以选 a - c 个, 个数不能超过 m, 可以选 c1 = min(a - c, m) 个
对于 nums2 同理可以选 c2 = min(b - c, m) 个
剩下的必须从交集中选, 需要选 n - c1 - c2 不能超过 c 个
所以可以选 min(n - c1 - c2, c)
综上可以选择 c1 + c2 + min(n - c1 - c2, c) 个元素
简化可得 min(n, c + c1 + c2), 或者 min(n, min(a, m + c) + min(b, m + c))
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumSetSize(vector<int>& nums1, vector<int>& nums2) 
    {
        unordered_set<int> set1(nums1.begin(), nums1.end()), set2;
        int cur = set1.size(), n = nums1.size();
        for (const auto& num : nums2) if (set2.insert(num).second) if (!set1.count(num)) ++cur;
        return min(cur, min((int)set1.size(), n >> 1) + min((int)set2.size(), n >> 1));
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumSetSize(int[] nums1, int[] nums2) {
        Set<Integer> set1 = Arrays.stream(nums1).boxed().collect(Collectors.toSet()), set2 = new HashSet<Integer>();
        int cur = set1.size(), n = nums1.length;
        for (int num : nums2) if (set2.add(num)) if (!set1.contains(num)) ++cur;
        return Math.min(cur, Math.min(set1.size(), n >> 1) + Math.min(set2.size(), n >> 1));
    }
}
```

__Python__:

```Python
class Solution:
    def maximumSetSize(self, nums1: List[int], nums2: List[int]) -> int:
        return (a := len(s1 := set(nums1))) + (b := len(s2 := set(nums2))) - (c := len(s1 & s2)) + min(c - max(a - (len(nums1) >> 1), 0) - max(b - (len(nums1) >> 1), 0), 0)
```
