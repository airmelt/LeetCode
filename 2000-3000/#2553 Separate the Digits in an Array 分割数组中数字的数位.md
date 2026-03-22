# 2553 Separate the Digits in an Array 分割数组中数字的数位

__Description:__

Given an array of positive integers `nums`, return _an array_ `answer` _that consists of the digits of each integer in_ `nums` _after separating them in __the same order__ they appear in_ `nums`.

To separate the digits of an integer is to get all the digits it has in the same order.

- For example, for the integer `10921`, the separation of its digits is `[1,0,9,2,1]`.

__Example:__

Example 1:

```text
Input: nums = [13,25,83,77]
Output: [1,3,2,5,8,3,7,7]
Explanation: 
```

- The separation of 13 is [1,3].
- The separation of 25 is [2,5].
- The separation of 83 is [8,3].
- The separation of 77 is [7,7].

answer = [1,3,2,5,8,3,7,7]. Note that answer contains the separations in the same order.

Example 2:

```text
Input: nums = [7,1,3,9]
Output: [7,1,3,9]
Explanation: The separation of each integer in nums is itself.
answer = [7,1,3,9].
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个正整数数组 `nums` ，请你返回一个数组 `answer` ，你需要将 `nums` 中每个整数进行数位分割后，按照 `nums` 中出现的 __相同顺序__ 放入答案数组中。

对一个整数进行数位分割，指的是将整数各个数位按原本出现的顺序排列成数组。

- 比方说，整数 `10921` ，分割它的各个数位得到 `[1,0,9,2,1]` 。

__示例:__

示例 1：

```text
输入：nums = [13,25,83,77]
输出：[1,3,2,5,8,3,7,7]
解释：
```

- 分割 13 得到 [1,3] 。
- 分割 25 得到 [2,5] 。
- 分割 83 得到 [8,3] 。
- 分割 77 得到 [7,7] 。

answer = [1,3,2,5,8,3,7,7] 。answer 中的数字分割结果按照原数字在数组中的相同顺序排列。

示例 2：

```text
输入：nums = [7,1,3,9]
输出：[7,1,3,9]
解释：nums 中每个整数的分割是它自己。
answer = [7,1,3,9] 。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 5`

__思路:__

```text
模拟
遍历数组中的每个数字
将数字转换为字符串
将字符串中的每个字符转换为整数
或者将数字模 10 存储到一个临时数组中
然后将临时数组反转
时间复杂度为 O(NlogM), 空间复杂度为 O(1), M 为数组中最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> separateDigits(vector<int>& nums) 
    {
        vector<int> result;
        for (int num : nums)
        {
            vector<int> cur;
            while (num)
            {
                cur.emplace_back(num % 10);
                num /= 10;
            }
            reverse(cur.begin(), cur.end());
            for (const auto& i : cur) result.emplace_back(i);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] separateDigits(int[] nums) {
        return Arrays.stream(Arrays.stream(nums).mapToObj(String::valueOf).collect(Collectors.joining()).split("")).mapToInt(Integer::parseInt).toArray();
    }
}
```

__Python__:

```Python
class Solution:
    def separateDigits(self, nums: List[int]) -> List[int]:
        return [int(i) for num in nums for i in str(num)]
```
