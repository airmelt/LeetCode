# 2040 Kth Smallest Product of Two Sorted Arrays 两个有序数组的第 K 小乘积

__Description:__

Given two __sorted 0-indexed__ integer arrays `nums1` and `nums2` as well as an integer `k`, return the `kth` __(1-based)__ smallest product of `nums1[i] * nums2[j]` where `0 <= i < nums1.length` and `0 <= j < nums2.length`.

__Example:__

Example 1:

```text
Input: nums1 = [2,5], nums2 = [3,4], k = 2
Output: 8
Explanation: The 2 smallest products are:
```

- nums1[0] \* nums2[0] = 2 \* 3 = 6
- nums1[0] \* nums2[1] = 2 \* 4 = 8

The 2nd smallest product is 8.

Example 2:

```text
Input: nums1 = [-4,-2,0,3], nums2 = [2,4], k = 6
Output: 0
Explanation: The 6 smallest products are:
```

- nums1[0] \* nums2[1] = (-4) \* 4 = -16
- nums1[0] \* nums2[0] = (-4) \* 2 = -8
- nums1[1] \* nums2[1] = (-2) \* 4 = -8
- nums1[1] \* nums2[0] = (-2) \* 2 = -4
- nums1[2] \* nums2[0] = 0 \* 2 = 0
- nums1[2] \* nums2[1] = 0 \* 4 = 0

The 6th smallest product is 0.

Example 3:

```text
Input: nums1 = [-2,-1,0,1,2], nums2 = [-3,-1,2,4,5], k = 3
Output: -6
Explanation: The 3 smallest products are:
```

- nums1[0] \* nums2[4] = (-2) \* 5 = -10
- nums1[0] \* nums2[3] = (-2) \* 4 = -8
- nums1[4] \* nums2[0] = 2 \* (-3) = -6

The 3rd smallest product is -6.

__Constraints:__

- `1 <= nums1.length, nums2.length <= 5 * 10 ^ 4`
- `-10 ^ 5 <= nums1[i], nums2[j] <= 10 ^ 5`
- `1 <= k <= nums1.length * nums2.length`
- `nums1` and `nums2` are sorted.

__题目描述:__

给你两个 __从小到大排好序__ 且下标从 __0__ 开始的整数数组 `nums1` 和 `nums2` 以及一个整数 `k` ，请你返回第 `k` （从 __1__ 开始编号）小的 `nums1[i] * nums2[j]` 的乘积，其中 `0 <= i < nums1.length` 且 `0 <= j < nums2.length` 。

__示例:__

示例 1：

```text
输入：nums1 = [2,5], nums2 = [3,4], k = 2
输出：8
解释：第 2 小的乘积计算如下：
```

- nums1[0] \* nums2[0] = 2 \* 3 = 6
- nums1[0] \* nums2[1] = 2 \* 4 = 8

第 2 小的乘积为 8 。

示例 2：

```text
输入：nums1 = [-4,-2,0,3], nums2 = [2,4], k = 6
输出：0
解释：第 6 小的乘积计算如下：
```

- nums1[0] \* nums2[1] = (-4) \* 4 = -16
- nums1[0] \* nums2[0] = (-4) \* 2 = -8
- nums1[1] \* nums2[1] = (-2) \* 4 = -8
- nums1[1] \* nums2[0] = (-2) \* 2 = -4
- nums1[2] \* nums2[0] = 0 \* 2 = 0
- nums1[2] \* nums2[1] = 0 \* 4 = 0

第 6 小的乘积为 0 。

示例 3：

```text
输入：nums1 = [-2,-1,0,1,2], nums2 = [-3,-1,2,4,5], k = 3
输出：-6
解释：第 3 小的乘积计算如下：
```

- nums1[0] \* nums2[4] = (-2) \* 5 = -10
- nums1[0] \* nums2[3] = (-2) \* 4 = -8
- nums1[4] \* nums2[0] = 2 \* (-3) = -6

第 3 小的乘积为 -6 。

__提示：__

- `1 <= nums1.length, nums2.length <= 5 * 10 ^ 4`
- `-10 ^ 5 <= nums1[i], nums2[j] <= 10 ^ 5`
- `1 <= k <= nums1.length * nums2.length`
- `nums1` 和 `nums2` 都是从小到大排好序的。

__思路:__

```text
二分查找
二分查找 nums1[i] * nums2[j] <= mid 的个数
将 nums 按照是否大于 0 分成四个数组, 分别为 neg1, neg2, pos1, pos2
以 pos1, pos2 为例, 对于 nums1[i] * nums2[j] <= mid, 总可以找到一个 j, 使得 nums1[i] * nums2[j] <= mid, 此时 nums1[i] * nums2[j] <= mid 的个数为 j + 1
再以 neg1, pos2 为例, 对于 nums1[i] * nums2[j] <= mid, 实际上是 -nums1[i] * nums2[j] >= mid, 总可以反向找到一个 j, 使得 nums1[i] * nums2[j] > mid, 此时 nums1[i] * nums2[j] <= mid 的个数为 n - 1 - j
时间复杂度为 O(NlogNlogM), 空间复杂度为 O(N), 其中 N 为数组的长度, M 为数组的元素的范围, 本题中 M = 10 ^ 5 * 10 ^ 5
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long kthSmallestProduct(vector<int>& nums1, vector<int>& nums2, long long k) 
    {
        vector<int> neg1, neg2, pos1, pos2;
        for (const auto& num : nums1) (num < 0) ? neg1.emplace_back(-num) : pos1.emplace_back(num);
        for (const auto& num : nums2) (num < 0) ? neg2.emplace_back(-num) : pos2.emplace_back(num);
        reverse(neg1.begin(), neg1.end());
        reverse(neg2.begin(), neg2.end());
        long long l = -1e10, r = 1e10;
        while (l < r)
        {
            long long mid = l + ((r - l) >> 1), cur = 0;
            cur += helper(pos1, pos2, mid, false);
            cur += helper(neg1, pos2, -mid, true);
            cur += helper(pos1, neg2, -mid, true);
            cur += helper(neg1, neg2, mid, false);
            if (cur < k) l = mid + 1;
            else r = mid;
        }
        return l;
    }
private:
    long long helper(vector<int>& a, vector<int>& b, long long mid, bool flag)
    {
        long long result = 0;
        if (a.empty() or b.empty()) return result;
        for (int i = 0, m = a.size(), n = b.size(), j = n - 1, k = n - 1; i < m; i++) 
        {
            while (j >= 0 and 1ll \* a[i] \* b[j] > mid) --j;
            while (k >= 0 and 1ll \* a[i] \* b[k] >= mid) --k;
            if (flag) result += n - 1 - k;
            else result += j + 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long kthSmallestProduct(int[] nums1, int[] nums2, long k) {
        List<Integer> neg1 = new ArrayList<>(), neg2 = new ArrayList<>(), pos1 = new ArrayList<>(), pos2 = new ArrayList<>();
        for (int num : nums1) if (num < 0) neg1.add(-num); else pos1.add(num);
        for (int num : nums2) if (num < 0) neg2.add(-num); else pos2.add(num);
        Collections.reverse(neg1);
        Collections.reverse(neg2);
        long l = -10_000_000_000L, r = 10_000_000_000L;
        while (l < r) {
            long mid = l + ((r - l) >> 1), cur = 0;
            cur += helper(pos1, pos2, mid, false);
            cur += helper(neg1, pos2, -mid, true);
            cur += helper(pos1, neg2, -mid, true);
            cur += helper(neg1, neg2, mid, false);
            if (cur < k) l = mid + 1;
            else r = mid;
        }
        return l;
    }

    private long helper(List<Integer> a, List<Integer> b, long mid, boolean flag) {
        long result = 0;
        if (a.isEmpty() || b.isEmpty()) return result;
        for (int i = 0, m = a.size(), n = b.size(), j = n - 1, k = n - 1; i < m; i++) {
            while (j >= 0 && 1L \* a.get(i) \* b.get(j) > mid) --j;
            while (k >= 0 && 1L \* a.get(i) \* b.get(k) >= mid) --k;
            if (flag) result += n - 1 - k;
            else result += j + 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def kthSmallestProduct(self, nums1: List[int], nums2: List[int], k: int) -> int:
        neg1, pos1, neg2, pos2, l, r = [-num for num in nums1[::-1] if num < 0], [num for num in nums1 if num >= 0], [-num for num in nums2[::-1] if num < 0], [num for num in nums2 if num >= 0], -10 ** 10, 10 ** 10
        def helper(a: List[int], b: List[int], mid: int, flag: bool) -> int:
            result, j, m, n, p = 0, len(b) - 1, len(a), len(b), len(b) - 1 
            if not m or not n:
                return result
            for i in range(m):
                while j >= 0 and a[i] \* b[j] > mid:
                    j -= 1
                while p >= 0 and a[i] \* b[p] >= mid:
                    p -= 1
                result += (n - 1 - p) \* flag + (j + 1) \* (not flag)
            return result
        while l < r:
            mid = l + ((r - l) >> 1)
            if sum([helper(pos1, pos2, mid, False), helper(neg1, pos2, -mid, True), helper(pos1, neg2, -mid, True), helper(neg1, neg2, mid, False)]) < k:
                l = mid + 1
            else:
                r = mid
        return l
```
