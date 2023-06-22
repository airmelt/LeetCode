# 1675 Minimize Deviation in Array 数组的最小偏移量

__Description:__

You are given an array `nums` of `n` positive integers.

You can perform two types of operations on any element of the array any number of times:

- If the element is __even__, __divide__ it by `2`.
  - For example, if the array is `[1,2,3,4]`, then you can do this operation on the last element, and the array will be `[1,2,3,2].`
- If the element is __odd__, __multiply__ it by `2`.
  -For example, if the array is `[1,2,3,4]`, then you can do this operation on the first element, and the array will be `[2,2,3,4].`
  - For example, if the array is `[1,2,3,4]`, then you can do this operation on the first element, and the array will be `[2,2,3,4].`

The deviation of the array is the maximum difference between any two elements in the array.

Return the minimum deviation the array can have after performing some number of operations.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4]
Output: 1
Explanation: You can transform the array to [1,2,3,2], then to [2,2,3,2], then the deviation will be 3 - 2 = 1.
```

Example 2:

```text
Input: nums = [4,1,5,20,3]
Output: 3
Explanation: You can transform the array after two operations to [4,2,5,5,3], then the deviation will be 5 - 2 = 3.
```

Example 3:

```text
Input: nums = [2,10,8]
Output: 3
```

__Constraints:__

- `n == nums.length`
- `2 <= n <= 5 * 10 ^ 4
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个由 `n` 个正整数组成的数组 `nums` 。

你可以对数组的任意元素执行任意次数的两类操作：

- 如果元素是 __偶数__ ，__除以__ `2`
  - 例如，如果数组是 `[1,2,3,4]` ，那么你可以对最后一个元素执行此操作，使其变成 `[1,2,3,__2__]`
- 如果元素是 __奇数__ ，__乘上__ `2`
  - 例如，如果数组是 `[1,2,3,4]` ，那么你可以对第一个元素执行此操作，使其变成 `[__2__,2,3,4]`
- 例如，如果数组是 `[1,2,3,4]` ，那么你可以对第一个元素执行此操作，使其变成 `[__2__,2,3,4]`

数组的 偏移量 是数组中任意两个元素之间的 最大差值 。

返回数组在执行某些操作之后可以拥有的 最小偏移量 。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4]
输出：1
解释：你可以将数组转换为 [1,2,3,2]，然后转换成 [2,2,3,2]，偏移量是 3 - 2 = 1
```

示例 2：

```text
输入：nums = [4,1,5,20,3]
输出：3
解释：两次操作后，你可以将数组转换为 [4,2,5,5,3]，偏移量是 5 - 2 = 3
```

示例 3：

```text
输入：nums = [2,10,8]
输出：3
```

__提示：__

- `n == nums.length`
- `2 <= n <= 5 * 10 ^ <span style="font-size: 10.8333px;">4</span>`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
优先队列
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumDeviation(vector<int>& nums) 
    {
        set<int> s;
        for (const auto& num : nums) s.insert(num & 1 ? (num << 1) : num);
        int result = *s.rbegin() - *s.begin();
        while (result and !(*s.rbegin() & 1)) 
        {
            int value = *s.rbegin();
            s.erase(value);
            s.insert(value >> 1);
            result = min(result, *s.rbegin() - *s.begin());
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumDeviation(int[] nums) {
        TreeSet<Integer> treeSet = new TreeSet<>();
        for (int num : nums) treeSet.add(num % 2 == 0 ? num : (num << 1));
        int result = treeSet.last() - treeSet.first();
        while (result > 0 && (treeSet.last() % 2 == 0)) {
            int value = treeSet.last();
            treeSet.remove(value);
            treeSet.add(value >> 1);
            result = Math.min(result, treeSet.last() - treeSet.first());
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumDeviation(self, nums: List[int]) -> int:
        nums = [-(v << 1) if v & 1 else -v for v in nums]
        min_value = -max(nums)
        heapq.heapify(nums)
        result = -nums[0] - min_value
        while not nums[0] & 1:
            min_value = min(min_value, -nums[0] >> 1)
            heapq.heapreplace(nums,nums[0] >> 1)
            result = min(result, -nums[0] - min_value)
        return result
```
