# 3012 Minimize Length of Array Using Operations 通过操作使数组长度最小

__Description:__

You are given a __0-indexed__ integer array `nums` containing __positive__ integers.

Your task is to __minimize__ the length of `nums` by performing the following operations __any__ number of times (including zero):

- Select __two__ __distinct__ indices `i` and `j` from `nums`, such that `nums[i] > 0` and `nums[j] > 0`.
- Insert the result of `nums[i] % nums[j]` at the end of `nums`.
- Delete the elements at indices `i` and `j` from `nums`.

Return _an integer denoting the __minimum__ __length__ of_ `nums` _after performing the operation any number of times._

__Example:__

Example 1:

```text
Input: nums = [1,4,3,1]
Output: 1
Explanation: One way to minimize the length of the array is as follows:
Operation 1: Select indices 2 and 1, insert nums[2] % nums[1] at the end and it becomes [1,4,3,1,3], then delete elements at indices 2 and 1.
nums becomes [1,1,3].
Operation 2: Select indices 1 and 2, insert nums[1] % nums[2] at the end and it becomes [1,1,3,1], then delete elements at indices 1 and 2.
nums becomes [1,1].
Operation 3: Select indices 1 and 0, insert nums[1] % nums[0] at the end and it becomes [1,1,0], then delete elements at indices 1 and 0.
nums becomes [0].
The length of nums cannot be reduced further. Hence, the answer is 1.
It can be shown that 1 is the minimum achievable length.
```

Example 2:

```text
Input: nums = [5,5,5,10,5]
Output: 2
Explanation: One way to minimize the length of the array is as follows:
Operation 1: Select indices 0 and 3, insert nums[0] % nums[3] at the end and it becomes [5,5,5,10,5,5], then delete elements at indices 0 and 3.
nums becomes [5,5,5,5]. 
Operation 2: Select indices 2 and 3, insert nums[2] % nums[3] at the end and it becomes [5,5,5,5,0], then delete elements at indices 2 and 3. 
nums becomes [5,5,0]. 
Operation 3: Select indices 0 and 1, insert nums[0] % nums[1] at the end and it becomes [5,5,0,0], then delete elements at indices 0 and 1.
nums becomes [0,0].
The length of nums cannot be reduced further. Hence, the answer is 2.
It can be shown that 2 is the minimum achievable length.
```

Example 3:

```text
Input: nums = [2,3,4]
Output: 1
Explanation: One way to minimize the length of the array is as follows: 
Operation 1: Select indices 1 and 2, insert nums[1] % nums[2] at the end and it becomes [2,3,4,3], then delete elements at indices 1 and 2.
nums becomes [2,3].
Operation 2: Select indices 1 and 0, insert nums[1] % nums[0] at the end and it becomes [2,3,1], then delete elements at indices 1 and 0.
nums becomes [1].
The length of nums cannot be reduced further. Hence, the answer is 1.
It can be shown that 1 is the minimum achievable length.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，它只包含 __正__ 整数。

你的任务是通过进行以下操作 __任意次__ （可以是 0 次） __最小化__ `nums` 的长度:

- 在 `nums` 中选择 __两个不同__ 的下标 `i` 和 `j` ，满足 `nums[i] > 0` 且 `nums[j] > 0` 。
- 将结果 `nums[i] % nums[j]` 插入 `nums` 的结尾。
- 将 `nums` 中下标为 `i` 和 `j` 的元素删除。

请你返回一个整数，它表示进行任意次操作以后 `nums` 的 __最小长度__ 。

__示例:__

示例 1：

```text
输入：nums = [1,4,3,1]
输出：1
解释：使数组长度最小的一种方法是：
操作 1 ：选择下标 2 和 1 ，插入 nums[2] % nums[1] 到数组末尾，得到 [1,4,3,1,3] ，然后删除下标为 2 和 1 的元素。
nums 变为 [1,1,3] 。
操作 2 ：选择下标 1 和 2 ，插入 nums[1] % nums[2] 到数组末尾，得到 [1,1,3,1] ，然后删除下标为 1 和 2 的元素。
nums 变为 [1,1] 。
操作 3 ：选择下标 1 和 0 ，插入 nums[1] % nums[0] 到数组末尾，得到 [1,1,0] ，然后删除下标为 1 和 0 的元素。
nums 变为 [0] 。
nums 的长度无法进一步减小，所以答案为 1 。
1 是可以得到的最小长度。
```

示例 2：

```text
输入：nums = [5,5,5,10,5]
输出：2
解释：使数组长度最小的一种方法是：
操作 1 ：选择下标 0 和 3 ，插入 nums[0] % nums[3] 到数组末尾，得到 [5,5,5,10,5,5] ，然后删除下标为 0 和 3 的元素。
nums 变为 [5,5,5,5] 。
操作 2 ：选择下标 2 和 3 ，插入 nums[2] % nums[3] 到数组末尾，得到 [5,5,5,5,0] ，然后删除下标为 2 和 3 的元素。
nums 变为 [5,5,0] 。
操作 3 ：选择下标 0 和 1 ，插入 nums[0] % nums[1] 到数组末尾，得到 [5,5,0,0] ，然后删除下标为 0 和 1 的元素。
nums 变为 [0,0] 。
nums 的长度无法进一步减小，所以答案为 2 。
2 是可以得到的最小长度。
```

示例 3：

```text
输入：nums = [2,3,4]
输出：1
解释：使数组长度最小的一种方法是：
操作 1 ：选择下标 1 和 2 ，插入 nums[1] % nums[2] 到数组末尾，得到 [2,3,4,3] ，然后删除下标为 1 和 2 的元素。
nums 变为 [2,3] 。
操作 2 ：选择下标 1 和 0 ，插入 nums[1] % nums[0] 到数组末尾，得到 [2,3,1] ，然后删除下标为 1 和 0 的元素。
nums 变为 [1] 。
nums 的长度无法进一步减小，所以答案为 1 。
1 是可以得到的最小长度。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
数学
找到最小值 mn
对于所有大于最小值的数 num
num % mn = mn
那么会留下 mn 而移除 num
所以当 mn 唯一或者有不是 mn 倍数的数的时候最后能剩下一个数
其他情况下会剩下 mn 的个数 count
然后两两配对
最后剩下 (count + 1) >> 1 个数
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumArrayLength(vector<int>& nums) 
    {
        int mn = *min_element(nums.begin(), nums.end());
        for (const auto& num : nums) if (num % mn) return 1;
        nums.erase(remove_if(nums.begin(), nums.end(), [&](const auto& num) { return num != mn; }), nums.end());
        return nums.size() + 1 >> 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumArrayLength(int[] nums) {
        int mn = Arrays.stream(nums).min().getAsInt();
        for (int num : nums) if (num % mn > 0) return 1;
        return (int)Arrays.stream(nums).filter(num -> num == mn).count() + 1 >> 1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumArrayLength(self, nums: List[int]) -> int:
        return 1 if (m := min(nums)) and any(x % m for x in nums) else nums.count(m) + 1 >> 1
```
