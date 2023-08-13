# 1748 Sum of Unique Elements 唯一元素的和

__Description:__

You are given an integer array `nums`. The unique elements of an array are the elements that appear __exactly once__ in the array.

Return _the __sum__ of all the unique elements of_ `nums`.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,2]
Output: 4
Explanation: The unique elements are [1,3], and the sum is 4.
```

Example 2:

```text
Input: nums = [1,1,1,1,1]
Output: 0
Explanation: There are no unique elements, and the sum is 0.
```

Example 3:

```text
Input: nums = [1,2,3,4,5]
Output: 15
Explanation: The unique elements are [1,2,3,4,5], and the sum is 15.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

__题目描述:__

给你一个整数数组 `nums` 。数组中唯一元素是那些只出现 __恰好一次__ 的元素。

请你返回 `nums` 中唯一元素的 __和__ 。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,2]
输出：4
解释：唯一元素为 [1,3] ，和为 4 。
```

示例 2：

```text
输入：nums = [1,1,1,1,1]
输出：0
解释：没有唯一元素，和为 0 。
```

示例 3 ：

```text
输入：nums = [1,2,3,4,5]
输出：15
解释：唯一元素为 [1,2,3,4,5] ，和为 15 。
```

__提示：__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

__思路:__

```text
模拟
一次遍历
记录每个元素出现的次数
如果第一次出现就加上
如果第二次出现就减去
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumOfUnique(vector<int>& nums)
    {
        int result = 0, count[101];
        memset(count, 0, sizeof count);
        for (const auto& num : nums) 
        {
            if (count[num]++ == 0) result += num;
            else if (count[num] == 2) result -= num;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumOfUnique(int[] nums) {
        int result = 0, count[] = new int[101];
        for (int num : nums) {
            if (count[num]++ == 0) result += num;
            else if (count[num] == 2) result -= num;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def sumOfUnique(self, nums: List[int]) -> int:
        return sum(k for k, v in Counter(nums).items() if v == 1)
```
