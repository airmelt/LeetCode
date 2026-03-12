# 2934 Minimum Operations to Maximize Last Elements in Arrays 最大化数组末位元素的最少操作次数

__Description:__

You are given two __0-indexed__ integer arrays, `nums1` and `nums2`, both having length `n`.

You are allowed to perform a series of operations (possibly none).

In an operation, you select an index `i` in the range `[0, n - 1]` and __swap__ the values of `nums1[i]` and `nums2[i]`.

Your task is to find the minimum number of operations required to satisfy the following conditions:

- `nums1[n - 1]` is equal to the __maximum value__ among all elements of `nums1`, i.e., `nums1[n - 1] = max(nums1[0], nums1[1], ..., nums1[n - 1])`.
- `nums2[n - 1]` is equal to the __maximum__ __value__ among all elements of `nums2`, i.e., `nums2[n - 1] = max(nums2[0], nums2[1], ..., nums2[n - 1])`.

Return _an integer denoting the __minimum__ number of operations needed to meet __both__ conditions_, _or_ `-1` _if it is __impossible__ to satisfy both conditions._

__Example:__

Example 1:

```text
Input: nums1 = [1,2,7], nums2 = [4,5,3]
Output: 1
Explanation: In this example, an operation can be performed using index i = 2.
When nums1[2] and nums2[2] are swapped, nums1 becomes [1,2,3] and nums2 becomes [4,5,7].
Both conditions are now satisfied.
It can be shown that the minimum number of operations needed to be performed is 1.
So, the answer is 1.
```

Example 2:

```text
Input: nums1 = [2,3,4,5,9], nums2 = [8,8,4,4,4]
Output: 2
Explanation: In this example, the following operations can be performed:
First operation using index i = 4.
When nums1[4] and nums2[4] are swapped, nums1 becomes [2,3,4,5,4], and nums2 becomes [8,8,4,4,9].
Another operation using index i = 3.
When nums1[3] and nums2[3] are swapped, nums1 becomes [2,3,4,4,4], and nums2 becomes [8,8,4,5,9].
Both conditions are now satisfied.
It can be shown that the minimum number of operations needed to be performed is 2.
So, the answer is 2.
```

Example 3:

```text
Input: nums1 = [1,5,4], nums2 = [2,5,3]
Output: -1
Explanation: In this example, it is not possible to satisfy both conditions. 
So, the answer is -1.
```

__Constraints:__

- `1 <= n == nums1.length == nums2.length <= 1000`
- `1 <= nums1[i] <= 10 ^ 9`
- `1 <= nums2[i] <= 10 ^ 9`

__题目描述:__

给你两个下标从 __0__ 开始的整数数组 `nums1` 和 `nums2` ，这两个数组的长度都是 `n` 。

你可以执行一系列 操作（可能不执行）。

在每次操作中，你可以选择一个在范围 `[0, n - 1]` 内的下标 `i` ，并交换 `nums1[i]` 和 `nums2[i]` 的值。

你的任务是找到满足以下条件所需的 最小 操作次数：

- `nums1[n - 1]` 等于 `nums1` 中所有元素的 __最大值__ ，即 `nums1[n - 1] = max(nums1[0], nums1[1], ..., nums1[n - 1])` 。
- `nums2[n - 1]` 等于 `nums2` 中所有元素的 __最大值__ ，即 `nums2[n - 1] = max(nums2[0], nums2[1], ..., nums2[n - 1])` 。

以整数形式，表示并返回满足上述 __全部__ 条件所需的 __最小__ 操作次数，如果无法同时满足两个条件，则返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums1 = [1,2,7]，nums2 = [4,5,3]
输出：1
解释：在这个示例中，可以选择下标 i = 2 执行一次操作。
交换 nums1[2] 和 nums2[2] 的值，nums1 变为 [1,2,3] ，nums2 变为 [4,5,7] 。
同时满足两个条件。
可以证明，需要执行的最小操作次数为 1 。
因此，答案是 1 。
```

示例 2：

```text
输入：nums1 = [2,3,4,5,9]，nums2 = [8,8,4,4,4]
输出：2
解释：在这个示例中，可以执行以下操作：
首先，选择下标 i = 4 执行操作。
交换 nums1[4] 和 nums2[4] 的值，nums1 变为 [2,3,4,5,4] ，nums2 变为 [8,8,4,4,9] 。
然后，选择下标 i = 3 执行操作。
交换 nums1[3] 和 nums2[3] 的值，nums1 变为 [2,3,4,4,4] ，nums2 变为 [8,8,4,5,9] 。
同时满足两个条件。 
可以证明，需要执行的最小操作次数为 2 。 
因此，答案是 2 。
```

示例 3：

```text
输入：nums1 = [1,5,4]，nums2 = [2,5,3]
输出：-1
解释：在这个示例中，无法同时满足两个条件。
因此，答案是 -1 。
```

__提示：__

- `1 <= n == nums1.length == nums2.length <= 1000`
- `1 <= nums1[i] <= 10 ^ 9`
- `1 <= nums2[i] <= 10 ^ 9`

__思路:__

```text
模拟
只有两种情况
要么交换 nums1[-1] 和 nums2[-1]
要么不交换 nums1[-1] 和 nums2[-1]
遍历 nums1 和 nums2
如果 nums1[i] > nums1[-1] 或者 nums2[i] > nums2[-1]
说明要发生交换
此时如果 nums1[i] > nums2[-1] 或者 nums2[i] > nums2[-1]
说明换了也没用直接返回 -1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& nums1, vector<int>& nums2) 
    {
        auto helper = [&](const auto& last1, const auto& last2) -> int
        {
            int result = 0;
            for (int i = 0, n = nums1.size(); i < n; i++) 
            {
                if (nums1[i] > last1 or nums2[i] > last2) 
                {
                    if (nums1[i] > last2 or nums2[i] > last1) return -1;
                    ++result;
                }
            }
            return result;
        };
        return min(helper(nums1.back(), nums2.back()), helper(nums2.back(), nums1.back()));
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[] nums1, int[] nums2) {
        return Math.min(helper(nums1, nums2, nums1[nums1.length - 1], nums2[nums1.length - 1]), helper(nums1, nums2, nums2[nums1.length - 1], nums1[nums1.length - 1]));
    }

    private int helper(int[] nums1, int[] nums2, int last1, int last2) {
        int result = 0;
        for (int i = 0, n = nums1.length; i < n; i++) {
            if (nums1[i] > last1 || nums2[i] > last2) {
                if (nums1[i] > last2 || nums2[i] > last1) return -1;
                ++result;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums1: List[int], nums2: List[int]) -> int:
        return min(-1 if any((x > nums1[-1] or y > nums2[-1]) and (y > nums1[-1] or x > nums2[-1]) for x, y in zip(nums1, nums2)) else sum(x > nums1[-1] or y > nums2[-1] for x, y in zip(nums1, nums2)), -1 if any((x > nums2[-1] or y > nums1[-1]) and (y > nums2[-1] or x > nums1[-1]) for x, y in zip(nums1, nums2)) else sum(x > nums2[-1] or y > nums1[-1] for x, y in zip(nums1, nums2)))
```
