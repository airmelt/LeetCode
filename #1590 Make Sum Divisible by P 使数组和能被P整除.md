# 1590 Make Sum Divisible by P 使数组和能被P整除

__Description:__

Given an array of positive integers `nums`, remove the __smallest__ subarray (possibly __empty__) such that the __sum__ of the remaining elements is divisible by `p`. It is __not__ allowed to remove the whole array.

Return _the length of the smallest subarray that you need to remove, or_ `-1` _if it's impossible_.

A subarray is defined as a contiguous block of elements in the array.

__Example:__

Example 1:

```text
Input: nums = [3,1,4,2], p = 6
Output: 1
Explanation: The sum of the elements in nums is 10, which is not divisible by 6. We can remove the subarray [4], and the sum of the remaining elements is 6, which is divisible by 6.
```

Example 2:

```text
Input: nums = [6,3,5,2], p = 9
Output: 2
Explanation: We cannot remove a single element to get a sum divisible by 9. The best way is to remove the subarray [5,2], leaving us with [6,3] with sum 9.
```

Example 3:

```text
Input: nums = [1,2,3], p = 3
Output: 0
Explanation: Here the sum is 6. which is already divisible by 3. Thus we do not need to remove anything.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= p <= 10 ^ 9`

__题目描述:__

给你一个正整数数组 `nums`，请你移除 __最短__ 子数组（可以为 __空__），使得剩余元素的 __和__ 能被 `p` 整除。 __不允许__ 将整个数组都移除。

请你返回你需要移除的最短子数组的长度，如果无法满足题目要求，返回 `-1` 。

子数组 定义为原数组中连续的一组元素。

__示例:__

示例 1：

```text
输入：nums = [3,1,4,2], p = 6
输出：1
解释：nums 中元素和为 10，不能被 p 整除。我们可以移除子数组 [4] ，剩余元素的和为 6 。
```

示例 2：

```text
输入：nums = [6,3,5,2], p = 9
输出：2
解释：我们无法移除任何一个元素使得和被 9 整除，最优方案是移除子数组 [5,2] ，剩余元素为 [6,3]，和为 9 。
```

示例 3：

```text
输入：nums = [1,2,3], p = 3
输出：0
解释：和恰好为 6 ，已经能被 3 整除了。所以我们不需要移除任何元素。
```

示例  4：

```text
输入：nums = [1,2,3], p = 7
输出：-1
解释：没有任何方案使得移除子数组后剩余元素的和被 7 整除。
```

示例 5：

```text
输入：nums = [1000000000,1000000000,1000000000], p = 3
输出：0
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= p <= 10 ^ 9`

__思路:__

```text
前缀和 ➕ 哈希表
初始化结果为数组长度, 这个值无法取到
先计算所有数字的和除 p 的余数, 记为 target, 余数为 0, 可以直接返回 0
目标是找到子数组的和除 p 的余数为 cur_target
将前缀和 cur 存放到哈希表中
将 cur_target 更新为 cur - target 除 p 的余数, 为了防止产生负数, 可以加上 p
如果 cur_target 在哈希表出现过, 说明当前数组减去之前前缀和正好能整除 p, 那么移除中间这段子数组就能整除 p, 更新结果为长度的较小值
比较结果和数组长度的大小返回结果或 -1
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minSubarray(vector<int>& nums, int p) 
    {
        int n = nums.size(), cur = 0, result = n, target = 0, cur_target = 0;
        for (int num: nums) target = (target + num) % p;
        if (target == 0) return target;
        map<int, int> pos;
        pos[0] = -1;
        for (int i = 0; i < n; i++) 
        {
            pos[cur = (cur + nums[i]) % p] = i;
            cur_target = (cur - target + p) % p;
            result = min(result, pos.find(cur_target) != pos.end() ? i - pos[cur_target] : INT_MAX);
        }
        return result == n ? -1 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minSubarray(int[] nums, int p) {
        int n = nums.length, cur = 0, result = n, target = 0, curTarget = 0;
        for (int num: nums) target = (target + num) % p;
        if (target == 0) return target;
        Map<Integer, Integer> pos = new HashMap<>();
        pos.put(0, -1);
        for (int i = 0; i < n; i++) {
            pos.put(cur = (cur + nums[i]) % p, i);
            curTarget = (cur - target + p) % p;
            result = Math.min(result, pos.containsKey(curTarget) ? i - pos.get(curTarget) : Integer.MAX_VALUE);
        }
        return result == n ? -1 : result;
    }
}
```

__Python__:

```Python
class Solution:
    def minSubarray(self, nums: List[int], p: int) -> int:
        target, result, cur, pos, cur_target = sum(nums) % p, (n := len(nums)), 0, defaultdict(int), 0
        if not target:
            return target
        pos[0] = -1
        for i in range(n):
            pos[cur := (cur + nums[i]) % p] = i
            cur_target = (cur - target + p) % p
            result = min(result, i - pos[cur_target] if cur_target in pos else float('inf'))
        return -1 if result == n else result
```
