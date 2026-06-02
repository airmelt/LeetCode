# 3041 Maximize Consecutive Elements in an Array After Modification 修改数组后最大化数组中的连续元素数目

__Description:__

You are given a __0-indexed__ array `nums` consisting of __positive__ integers.

Initially, you can increase the value of __any__ element in the array by __at most__ `1`.

After that, you need to select __one or more__ elements from the final array such that those elements are __consecutive__ when sorted in increasing order. For example, the elements `[3, 4, 5]` are consecutive while `[3, 4, 6]` and `[1, 1, 2, 3]` are not.

Return the maximum number of elements that you can select.

__Example:__

Example 1:

```text
Input: nums = [2,1,5,1,1]
Output: 3
Explanation: We can increase the elements at indices 0 and 3. The resulting array is nums = [3,1,5,2,1].
We select the elements [3,1,5,2,1] and we sort them to obtain [1,2,3], which are consecutive.
It can be shown that we cannot select more than 3 consecutive elements.
```

Example 2:

```text
Input: nums = [1,4,7,10]
Output: 1
Explanation: The maximum consecutive elements that we can select is 1.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个下标从 __0__ 开始只包含 __正__ 整数的数组 `nums` 。

一开始，你可以将数组中 __任意数量__ 元素增加 __至多__ `1` 。

修改后，你可以从最终数组中选择 __一个或者更多__ 元素，并确保这些元素升序排序后是 __连续__ 的。比方说， `[3, 4, 5]` 是连续的，但是 `[3, 4, 6]` 和 `[1, 1, 2, 3]` 不是连续的。

请你返回 最多 可以选出的元素数目。

__示例:__

示例 1：

```text
输入：nums = [2,1,5,1,1]
输出：3
解释：我们将下标 0 和 3 处的元素增加 1 ，得到结果数组 nums = [3,1,5,2,1] 。
我们选择元素 [3,1,5,2,1] 并将它们排序得到 [1,2,3] ，是连续元素。
最多可以得到 3 个连续元素。
```

示例 2：

```text
输入：nums = [1,4,7,10]
输出：1
解释：我们可以选择的最多元素数目是 1 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`

__思路:__

```text
排序 ➕ 动态规划
由于可以对任意元素进行操作, 结果也和数组原顺序无关, 先对数组进行排序
设 dp[i] 表示以 i 结尾的最长连续数组的长度
如果不操作的时候 dp[i] > dp[i + 1], 那么操作总可以使得 dp[i + 1] = dp[i]
对于元素 num
如果操作那么 num + 1 可以接在 num 之后所以有 dp[num + 1] = dp[num] + 1
如果不操作那么 num 可以接在 num - 1 之后所以有 dp[num] = dp[num - 1] + 1
要么同时更新上述结果, 要么先更新操作的结果避免操作两次 dp[num]
最后返回 max(dp) 即可
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 主要时间复杂度为排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxSelectedElements(vector<int>& nums) 
    {
        ranges::sort(nums);
        unordered_map<int, int> dp;
        for (int num : nums) 
        {
            dp[num + 1] = dp[num] + 1;
            dp[num] = dp[num - 1] + 1;
        }
        return max_element(dp.begin(), dp.end(), [](const auto& a, const auto& b) { return a.second < b.second; }) -> second;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSelectedElements(int[] nums) {
        Arrays.sort(nums);
        var dp = new HashMap<Integer, Integer>();
        for (int num : nums) {
            dp.put(num + 1, dp.getOrDefault(num, 0) + 1);
            dp.put(num, dp.getOrDefault(num - 1, 0) + 1);
        }
        return dp.values().stream().max(Comparator.naturalOrder()).get();
    }
}
```

__Python__:

```Python
class Solution:
    def maxSelectedElements(self, nums: List[int]) -> int:
        dp = defaultdict(int)
        for num in sorted(nums):
            dp[num], dp[num + 1] = dp[num - 1] + 1, dp[num] + 1
        return max(dp.values())
```
