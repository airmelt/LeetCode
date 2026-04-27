# 2499 Minimum Total Cost to Make Arrays Unequal 让数组不相等的最小总代价

__Description:__

You are given two __0-indexed__ integer arrays `nums1` and `nums2`, of equal length `n`.

In one operation, you can swap the values of any two indices of `nums1`. The __cost__ of this operation is the __sum__ of the indices.

Find the __minimum__ total cost of performing the given operation __any__ number of times such that `nums1[i] != nums2[i]` for all `0 <= i <= n - 1` after performing all the operations.

Return _the __minimum total cost__ such that_ `nums1` and `nums2` _satisfy the above condition_. In case it is not possible, return `-1`.

__Example:__

Example 1:

```text
Input: nums1 = [1,2,3,4,5], nums2 = [1,2,3,4,5]
Output: 10
Explanation: 
One of the ways we can perform the operations is:
```

- Swap values at indices 0 and 3, incurring cost = 0 + 3 = 3. Now, nums1 = [4,2,3,1,5]
- Swap values at indices 1 and 2, incurring cost = 1 + 2 = 3. Now, nums1 = [4,3,2,1,5].
- Swap values at indices 0 and 4, incurring cost = 0 + 4 = 4. Now, nums1 =[5,3,2,1,4].

We can see that for each index i, nums1[i] != nums2[i]. The cost required here is 10.

Note that there are other ways to swap values, but it can be proven that it is not possible to obtain a cost less than 10.

Example 2:

```text
Input: nums1 = [2,2,2,1,3], nums2 = [1,2,2,3,3]
Output: 10
Explanation: 
One of the ways we can perform the operations is:
```

- Swap values at indices 2 and 3, incurring cost = 2 + 3 = 5. Now, nums1 = [2,2,1,2,3].
- Swap values at indices 1 and 4, incurring cost = 1 + 4 = 5. Now, nums1 = [2,3,1,2,2].

The total cost needed here is 10, which is the minimum possible.

Example 3:

```text
Input: nums1 = [1,2,2], nums2 = [1,2,2]
Output: -1
Explanation: 
It can be shown that it is not possible to satisfy the given conditions irrespective of the number of operations we perform.
Hence, we return -1.
```

__Constraints:__

- `n == nums1.length == nums2.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums1[i], nums2[i] <= n`

__题目描述:__

给你两个下标从 __0__ 开始的整数数组 `nums1` 和 `nums2` ，两者长度都为 `n` 。

每次操作中，你可以选择交换 `nums1` 中任意两个下标处的值。操作的 __开销__ 为两个下标的 __和__ 。

你的目标是对于所有的 `0 <= i <= n - 1` ，都满足 `nums1[i] != nums2[i]` ，你可以进行 __任意次__ 操作，请你返回达到这个目标的 __最小__ 总代价。

请你返回让 `nums1` 和 `nums2` 满足上述条件的 __最小总代价__ ，如果无法达成目标，返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums1 = [1,2,3,4,5], nums2 = [1,2,3,4,5]
输出：10
解释：
实现目标的其中一种方法为：
```

- 交换下标为 0 和 3 的两个值，代价为 0 + 3 = 3 。现在 nums1 = [4,2,3,1,5] 。
- 交换下标为 1 和 2 的两个值，代价为 1 + 2 = 3 。现在 nums1 = [4,3,2,1,5] 。
- 交换下标为 0 和 4 的两个值，代价为 0 + 4 = 4 。现在 nums1 = [5,3,2,1,4] 。

最后，对于每个下标 i ，都有 nums1[i] != nums2[i] 。总代价为 10 。

还有别的交换值的方法，但是无法得到代价和小于 10 的方案。

示例 2：

```text
输入：nums1 = [2,2,2,1,3], nums2 = [1,2,2,3,3]
输出：10
解释：
实现目标的一种方法为：
```

- 交换下标为 2 和 3 的两个值，代价为 2 + 3 = 5 。现在 nums1 = [2,2,1,2,3] 。
- 交换下标为 1 和 4 的两个值，代价为 1 + 4 = 5 。现在 nums1 = [2,3,1,2,2] 。

总代价为 10 ，是所有方案中的最小代价。

示例 3：

```text
输入：nums1 = [1,2,2], nums2 = [1,2,2]
输出：-1
解释：
不管怎么操作，都无法满足题目要求。
所以返回 -1 。
```

__提示：__

- `n == nums1.length == nums2.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums1[i], nums2[i] <= n`

__思路:__

```text
贪心
总可以使用下标 0 实现交换
这样是不会增加总代价的
先使用摩尔投票法找到 nums1 和 nums2 相等下标的众数 max_value 和出现次数 max_count
如果 max_count 已经太多了, 即超过半数那无论如何也不能交换, 返回 -1
否则需要引入外部的元素, 这个元素不能是 max_value
即 nums1[i] != max_value and nums2[i] != max_value and nums1[i] != nums2[i]
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minimumTotalCost(vector<int>& nums1, vector<int>& nums2) 
    {
        long long result = 0LL;
        int n = nums1.size(), equal_count = 0, max_count = 0, max_value = 0, cur = 0;
        for (int i = 0; i < n; i++) 
        {
            if (nums1[i] == nums2[i]) 
            {
                if (!cur) max_value = nums1[i];
                cur += ((nums1[i] == max_value) << 1) - 1;
                result += i;
                ++equal_count;
            }
        }
        for (int i = 0; i < n; i++) max_count += (nums1[i] == nums2[i] and nums1[i] == max_value);
        for (int i = 0; i < n; i++) 
        {
            if ((max_count << 1) <= equal_count) break;
            if (nums1[i] != nums2[i] and nums1[i] != max_value and nums2[i] != max_value) 
            {
                result += i;
                ++equal_count;
            }
        }
        return (max_count << 1) <= equal_count ? result : -1LL;
    }
};
```

__Java__:

```Java
class Solution {
    public long minimumTotalCost(int[] nums1, int[] nums2) {
        long result = 0L;
        int n = nums1.length, equalCount = 0, maxCount = 0, maxValue = 0, cur = 0;
        for (int i = 0; i < n; i++) {
            if (nums1[i] == nums2[i]) {
                if (cur == 0) maxValue = nums1[i];
                cur += ((nums1[i] == maxValue ? 1 : 0) << 1) - 1;
                result += i;
                ++equalCount;
            }
        }
        for (int i = 0; i < n; i++) maxCount += (nums1[i] == nums2[i] && nums1[i] == maxValue ? 1 : 0);
        for (int i = 0; i < n; i++) {
            if ((maxCount << 1) <= equalCount) break;
            if (nums1[i] != nums2[i] && nums1[i] != maxValue && nums2[i] != maxValue) {
                result += i;
                ++equalCount;
            }
        }
        return (maxCount << 1) <= equalCount ? result : -1L;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumTotalCost(self, nums1: List[int], nums2: List[int]) -> int:
        result, equal_count, max_count, max_value = 0, 0, 0, 0
        for i, (x, y) in enumerate(zip(nums1, nums2)):
            if x == y:
                max_value = x if not max_count else max_value
                max_count += ((x == max_value) << 1) - 1
                result += i
                equal_count += 1
        max_count = sum(x == y == max_value for x, y in zip(nums1, nums2))
        for i, (x, y) in enumerate(zip(nums1, nums2)):
            if (max_count << 1) <= equal_count: 
                break
            if x != y and x != max_value and y != max_value:
                result += i
                equal_count += 1
        return result if (max_count << 1) <= equal_count else -1
```
