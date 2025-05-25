# 2541 Minimum Operations to Make Array Equal II 使数组中所有元素相等的最小操作数 II

__Description:__

You are given two integer arrays `nums1` and `nums2` of equal length `n` and an integer `k`. You can perform the following operation on `nums1`:

- Choose two indexes `i` and `j` and increment `nums1[i]` by `k` and decrement `nums1[j]` by `k`. In other words, `nums1[i] = nums1[i] + k` and `nums1[j] = nums1[j] - k`.

`nums1` is said to be __equal__ to `nums2` if for all indices `i` such that `0 <= i < n`, `nums1[i] == nums2[i]`.

Return _the __minimum__ number of operations required to make_ `nums1` _equal to_ `nums2`. If it is impossible to make them equal, return `-1`.

__Example:__

Example 1:

```text
Input: nums1 = [4,3,1,4], nums2 = [1,3,7,1], k = 3
Output: 2
Explanation: In 2 operations, we can transform nums1 to nums2.
1st operation: i = 2, j = 0. After applying the operation, nums1 = [1,3,4,4].
2nd operation: i = 2, j = 3. After applying the operation, nums1 = [1,3,7,1].
One can prove that it is impossible to make arrays equal in fewer operations.
```

Example 2:

```text
Input: nums1 = [3,8,5,2], nums2 = [2,4,1,6], k = 1
Output: -1
Explanation: It can be proved that it is impossible to make the two arrays equal.
```

__Constraints:__

- `n == nums1.length == nums2.length`
- `2 <= n <= 10 ^ 5`
- `0 <= nums1[i], nums2[j] <= 10 ^ 9`
- `0 <= k <= 10 ^ 5`

__题目描述:__

给你两个整数数组 `nums1` 和 `nums2` ，两个数组长度都是 `n` ，再给你一个整数 `k` 。你可以对数组 `nums1` 进行以下操作:

- 选择两个下标 `i` 和 `j` ，将 `nums1[i]` 增加 `k` ，将 `nums1[j]` 减少 `k` 。换言之， `nums1[i] = nums1[i] + k` 且 `nums1[j] = nums1[j] - k` 。

如果对于所有满足 `0 <= i < n` 都有 `num1[i] == nums2[i]` ，那么我们称 `nums1` __等于__ `nums2` 。

请你返回使 `nums1` 等于 `nums2` 的 __最少__ 操作数。如果没办法让它们相等，请你返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums1 = [4,3,1,4], nums2 = [1,3,7,1], k = 3
输出：2
解释：我们可以通过 2 个操作将 nums1 变成 nums2 。
第 1 个操作：i = 2 ，j = 0 。操作后得到 nums1 = [1,3,4,4] 。
第 2 个操作：i = 2 ，j = 3 。操作后得到 nums1 = [1,3,7,1] 。
无法用更少操作使两个数组相等。
```

示例 2：

```text
输入：nums1 = [3,8,5,2], nums2 = [2,4,1,6], k = 1
输出：-1
解释：无法使两个数组相等。
```

__提示：__

- `n == nums1.length == nums2.length`
- `2 <= n <= 10 ^ 5`
- `0 <= nums1[i], nums2[j] <= 10 ^ 9`
- `0 <= k <= 10 ^ 5`

__思路:__

```text
模拟
用 nums1[i] 对应的元素减去 nums2[i]
如果 k == 0, 那么 nums1[i] 不为 0 就返回 -1
否则, nums1[i] 必须是 k 的倍数
同时累计 nums1[i] / k 的值记为 s
结果只记录 nums1[i] 大于 0 的值
如果 s 不为 0, 那么返回 -1, 否则返回结果
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minOperations(vector<int>& nums1, vector<int>& nums2, int k) 
    {
        long long result = 0LL, s = 0LL;
        for (int i = 0, n = nums1.size(); i < n; i++) 
        {
            nums1[i] -= nums2[i];
            if (k) 
            {
                if (nums1[i] % k) return -1LL;
                s += nums1[i] / k;
                if (nums1[i] > 0) result += nums1[i] / k;
            } else if (nums1[i]) return -1LL;
        }
        return s ? -1LL : result;
    }
};
```

__Java__:

```Java
class Solution {
    public long minOperations(int[] nums1, int[] nums2, int k) {
        long result = 0L, s = 0L;
        for (int i = 0, n = nums1.length; i < n; i++) {
            nums1[i] -= nums2[i];
            if (k != 0) {
                if (nums1[i] % k != 0) return -1L;
                s += nums1[i] / k;
                if (nums1[i] > 0) result += nums1[i] / k;
            } else if (nums1[i] != 0) return -1L;
        }
        return s == 0L ? result : -1L;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums1: List[int], nums2: List[int], k: int) -> int:
        result, s = 0, 0
        for x, y in zip(nums1, nums2):
            x -= y
            if k:
                if x % k:
                    return -1
                s += x // k
                result += max(x, 0) // k
            elif x:
                return -1
        return -1 if s else result
```
