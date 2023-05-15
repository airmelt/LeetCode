# 1636 Sort Array by Increasing Frequency 按照频率将数组升序排序

__Description:__

Given an array of integers `nums`, sort the array in __increasing__ order based on the frequency of the values. If multiple values have the same frequency, sort them in __decreasing__ order.

Return the sorted array.

__Example:__

Example 1:

```text
Input: nums = [1,1,2,2,2,3]
Output: [3,1,1,2,2,2]
Explanation: '3' has a frequency of 1, '1' has a frequency of 2, and '2' has a frequency of 3.
```

Example 2:

```text
Input: nums = [2,3,1,3,2]
Output: [1,3,3,2,2]
Explanation: '2' and '3' both have a frequency of 2, so they are sorted in decreasing order.
```

Example 3:

```text
Input: nums = [-1,1,-6,4,5,-6,1,4,1]
Output: [5,-1,4,4,-6,-6,1,1,1]
```

__Constraints:__

- `1 <= nums.length <= 100`
- `-100 <= nums[i] <= 100`

__题目描述:__

给你一个整数数组 `nums` ，请你将数组按照每个值的频率 __升序__ 排序。如果有多个值的频率相同，请你按照数值本身将它们 __降序__ 排序。

请你返回排序后的数组。

__示例:__

示例 1：

```text
输入：nums = [1,1,2,2,2,3]
输出：[3,1,1,2,2,2]
解释：'3' 频率为 1，'1' 频率为 2，'2' 频率为 3 。
```

示例 2：

```text
输入：nums = [2,3,1,3,2]
输出：[1,3,3,2,2]
解释：'2' 和 '3' 频率都为 2 ，所以它们之间按照数值本身降序排序。
```

示例 3：

```text
输入：nums = [-1,1,-6,4,5,-6,1,4,1]
输出：[5,-1,4,4,-6,-6,1,1,1]
```

__提示：__

- `1 <= nums.length <= 100`
- `-100 <= nums[i] <= 100`

__思路:__

```text
模拟
记录每个数字出现的频率
先按照频率排序, 频率相同的数字再按照数字从大到小排序
数字从小到大也可以是负的数字从小到大
时间复杂度为 O(NlogN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
int arr[201];
class Solution 
{
public:
    vector<int> frequencySort(vector<int>& nums) 
    {
        memset(arr, 0, sizeof arr);
        for (const auto& num : nums) ++arr[num + 100];
        sort(nums.begin(), nums.end(), [&](int a, int b) { return pair{ arr[a + 100], b } < pair{ arr[b + 100], a }; });
        return nums;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] frequencySort(int[] nums) {
        return Arrays.stream(nums).boxed().collect(Collectors.groupingBy(Function.identity(), Collectors.counting())).entrySet().stream().sorted(Map.Entry.<Integer, Long>comparingByValue().thenComparing(Map.Entry.<Integer, Long>comparingByKey().reversed())).flatMapToInt(e -> IntStream.generate(e::getKey).limit(e.getValue())).toArray();
    }
}
```

__Python__:

```Python
class Solution:
    def frequencySort(self, nums: List[int]) -> List[int]:
        return sorted(nums, key=lru_cache(None)(lambda x: (nums.count(x), -x)))
```
