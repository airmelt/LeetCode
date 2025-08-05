# 1775 Equal Sum Arrays With Minimum Number of Operations 通过最少操作次数使数组的和相等

__Description:__

You are given two arrays of integers `nums1` and `nums2`, possibly of different lengths. The values in the arrays are between `1` and `6`, inclusive.

In one operation, you can change any integer's value in __any__ of the arrays to __any__ value between `1` and `6`, inclusive.

Return _the minimum number of operations required to make the sum of values in_ `nums1` _equal to the sum of values in_ `nums2`_._ Return `-1`​​​​​ if it is not possible to make the sum of the two arrays equal.

__Example:__

Example 1:

```text
Input: nums1 = [1,2,3,4,5,6], nums2 = [1,1,2,2,2,2]
Output: 3
Explanation: You can make the sums of nums1 and nums2 equal with 3 operations. All indices are 0-indexed.
- Change nums2[0] to 6. nums1 = [1,2,3,4,5,6], nums2 = [6,1,2,2,2,2].
- Change nums1[5] to 1. nums1 = [1,2,3,4,5,1], nums2 = [6,1,2,2,2,2].
- Change nums1[2] to 2. nums1 = [1,2,2,4,5,1], nums2 = [6,1,2,2,2,2].
```

Example 2:

```text
Input: nums1 = [1,1,1,1,1,1,1], nums2 = [6]
Output: -1
Explanation: There is no way to decrease the sum of nums1 or to increase the sum of nums2 to make them equal.
```

Example 3:

```text
Input: nums1 = [6,6], nums2 = [1]
Output: 3
Explanation: You can make the sums of nums1 and nums2 equal with 3 operations. All indices are 0-indexed. 
- Change nums1[0] to 2. nums1 = [2,6], nums2 = [1].
- Change nums1[1] to 2. nums1 = [2,2], nums2 = [1].
- Change nums2[0] to 4. nums1 = [2,2], nums2 = [4].
```

__Constraints:__

- `1 <= nums1.length, nums2.length <= 10 ^ 5`
- `1 <= nums1[i], nums2[i] <= 6`

__题目描述:__

给你两个长度可能不等的整数数组 `nums1` 和 `nums2` 。两个数组中的所有值都在 `1` 到 `6` 之间（包含 `1` 和 `6`）。

每次操作中，你可以选择 __任意__ 数组中的任意一个整数，将它变成 `1` 到 `6` 之间 __任意__ 的值（包含 `1` 和 `6`）。

请你返回使 `nums1` 中所有数的和与 `nums2` 中所有数的和相等的最少操作次数。如果无法使两个数组的和相等，请返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums1 = [1,2,3,4,5,6], nums2 = [1,1,2,2,2,2]
输出：3
解释：你可以通过 3 次操作使 nums1 中所有数的和与 nums2 中所有数的和相等。以下数组下标都从 0 开始。
- 将 nums2[0] 变为 6 。 nums1 = [1,2,3,4,5,6], nums2 = [6,1,2,2,2,2] 。
- 将 nums1[5] 变为 1 。 nums1 = [1,2,3,4,5,1], nums2 = [6,1,2,2,2,2] 。
- 将 nums1[2] 变为 2 。 nums1 = [1,2,2,4,5,1], nums2 = [6,1,2,2,2,2] 。
```

示例 2：

```text
输入：nums1 = [1,1,1,1,1,1,1], nums2 = [6]
输出：-1
解释：没有办法减少 nums1 的和或者增加 nums2 的和使二者相等。
```

示例 3：

```text
输入：nums1 = [6,6], nums2 = [1]
输出：3
解释：你可以通过 3 次操作使 nums1 中所有数的和与 nums2 中所有数的和相等。以下数组下标都从 0 开始。
- 将 nums1[0] 变为 2 。 nums1 = [2,6], nums2 = [1] 。
- 将 nums1[1] 变为 2 。 nums1 = [2,2], nums2 = [1] 。
- 将 nums2[0] 变为 4 。 nums1 = [2,2], nums2 = [4] 。
```

__提示：__

- `1 <= nums1.length, nums2.length <= 10 ^ 5`
- `1 <= nums1[i], nums2[i] <= 6`

__思路:__

```text
贪心
计算 nums1 的和 s1 和 nums2 的和 s2
目标是将 target = abs(s1 - s2) 减少到 0
将大的和往 1 减少, 每次的变化量为 num - 1
将小的和往 6 增加, 每次的变化量为 6 - num
记录下需要变化的量, 从大到小枚举可以变化的量, 即 5 到 1
如果遍历到 0, 说明不够, 返回 -1
如果 target > change[cur] * cur, 说明可以完全变化
此时 result += change[cur], target -= change[cur] * cur, cur 自减
否则, 只需要修改 (target / cur) + (target % cur == 0) 个数就能完成修改, target % cur 是保证整除的情况
时间复杂度为 O(M + N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& nums1, vector<int>& nums2) 
    {
        int change[6] = {0}, s1 = accumulate(nums1.begin(), nums1.end(), 0), s2 = accumulate(nums2.begin(), nums2.end(), 0), result = 0, target = abs(s1 - s2), cur = 5;
        for (const auto& num : (s1 > s2 ? nums1 : nums2)) ++change[num - 1];
        for (const auto& num : (s1 > s2 ? nums2 : nums1)) ++change[6 - num];
        while (target) 
        {
            if (cur == 0) return -1;
            if (target > change[cur] * cur) 
            {
                result += change[cur];
                target -= change[cur] * (cur--);
            }
            else return result + target / cur + (!!(target % cur));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[] nums1, int[] nums2) {
        int change[] = new int[6], s1 = Arrays.stream(nums1).boxed().toList().stream().reduce(0, Integer::sum), s2 = Arrays.stream(nums2).boxed().toList().stream().reduce(0, Integer::sum), result = 0, target = Math.abs(s1 - s2), cur = 5;
        for (int num : (s1 > s2 ? nums1 : nums2)) ++change[num - 1];
        for (int num : (s1 > s2 ? nums2 : nums1)) ++change[6 - num];
        while (target != 0) {
            if (cur == 0) return -1;
            if (target > change[cur] * cur) {
                result += change[cur];
                target -= change[cur] * (cur--);
            } else return result + target / cur + (target % cur == 0 ? 0 : 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums1: List[int], nums2: List[int]) -> int:
        change, target, result, cur = [0] * 6, abs((s1 := sum(nums1)) - (s2 := sum(nums2))), 0, 5
        if s1 < s2:
            nums1, nums2 = nums2, nums1
        for num in nums1:
            change[num - 1] += 1
        for num in nums2:
            change[6 - num] += 1
        while target:
            if not cur:
                return -1
            if target > change[cur] * cur:
                result += change[cur]
                target -= change[cur] * cur
                cur -= 1
            else:
                return result + target // cur + (not not target % cur)
        return result
```
