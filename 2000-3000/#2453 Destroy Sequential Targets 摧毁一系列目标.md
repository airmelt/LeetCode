# 2453 Destroy Sequential Targets 摧毁一系列目标

__Description:__

You are given a __0-indexed__ array `nums` consisting of positive integers, representing targets on a number line. You are also given an integer `space`.

You have a machine which can destroy targets. __Seeding__ the machine with some `nums[i]` allows it to destroy all targets with values that can be represented as `nums[i] + c * space`, where `c` is any non-negative integer. You want to destroy the __maximum__ number of targets in `nums`.

Return _the __minimum value__ of_ `nums[i]` _you can seed the machine with to destroy the maximum number of targets._

__Example:__

Example 1:

```text
Input: nums = [3,7,8,1,1,5], space = 2
Output: 1
Explanation: If we seed the machine with nums[3], then we destroy all targets equal to 1,3,5,7,9,... 
In this case, we would destroy 5 total targets (all except for nums[2]). 
It is impossible to destroy more than 5 targets, so we return nums[3].
```

Example 2:

```text
Input: nums = [1,3,5,2,4,6], space = 2
Output: 1
Explanation: Seeding the machine with nums[0], or nums[3] destroys 3 targets. 
It is not possible to destroy more than 3 targets.
Since nums[0] is the minimal integer that can destroy 3 targets, we return 1.
```

Example 3:

```text
Input: nums = [6,2,5], space = 100
Output: 2
Explanation: Whatever initial seed we select, we can only destroy 1 target. The minimal seed is nums[1].
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= space <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的数组 `nums` ，它包含若干正整数，表示数轴上你需要摧毁的目标所在的位置。同时给你一个整数 `space` 。

你有一台机器可以摧毁目标。给机器 __输入__ `nums[i]` ，这台机器会摧毁所有位置在 `nums[i] + c * space` 的目标，其中 `c` 是任意非负整数。你想摧毁 `nums` 中 __尽可能多__ 的目标。

请你返回在摧毁数目最多的前提下， `nums[i]` 的 __最小值__ 。

__示例:__

示例 1：

```text
输入：nums = [3,7,8,1,1,5], space = 2
输出：1
解释：如果我们输入 nums[3] ，我们可以摧毁位于 1,3,5,7,9,... 这些位置的目标。
这种情况下， 我们总共可以摧毁 5 个目标（除了 nums[2]）。
没有办法摧毁多于 5 个目标，所以我们返回 nums[3] 。
```

示例 2：

```text
输入：nums = [1,3,5,2,4,6], space = 2
输出：1
解释：输入 nums[0] 或者 nums[3] 都会摧毁 3 个目标。
没有办法摧毁多于 3 个目标。
由于 nums[0] 是最小的可以摧毁 3 个目标的整数，所以我们返回 1 。
```

示例 3：

```text
输入：nums = [6,2,5], space = 100
输出：2
解释：无论我们输入哪个数字，都只能摧毁 1 个目标。输入的最小整数是 nums[1] 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= space <= 10 ^ 9`

__思路:__

```text
数学
从第 i 个开始可以摧毁 i, i + space, i + 2 * space, i + 3 * space, ...
所以可以按照下标对 space 取模
分组之后取长度最长的组的最小值
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int destroyTargets(vector<int>& nums, int space) 
    {
        unordered_map<int, vector<int>> m;
        for (const auto& num : nums) m[num % space].emplace_back(num);
        int max_value = 0, result = 0;
        for (const auto& [_, v] : m)
        {
            int n = v.size(), min_value = *min_element(v.begin(), v.end());
            if (n > max_value or n == max_value and min_value < result) 
            {
                max_value = n;
                result = min_value;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int destroyTargets(int[] nums, int space) {
        var map = new HashMap<Integer, List<Integer>>();
        for (int num : nums) map.computeIfAbsent(num % space, v -> new ArrayList<>()).add(num);
        int maxValue = 0, result = 0;
        for (List<Integer> v : map.values()) {
            int n = v.size(), minValue = Collections.min(v);
            if (n > maxValue || n == maxValue && minValue < result) {
                maxValue = n;
                result = minValue;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def destroyTargets(self, nums: List[int], space: int) -> int:
        return min(nums, key=lambda num: (-c[num % space], num)) if (c := Counter(num % space for num in nums)) else 0
```
